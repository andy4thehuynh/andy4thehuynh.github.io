+++
date = "2015-01-25T11:56:12-08:00"
title = "User Authentication with Cuba"
description = ""
+++

User authentication should be secure and easy to understand. We'll create a basic authentication interface using tools built by the boys at Openredis: [Cuba](https://github.com/soveran/cuba), [Shield](https://github.com/cyx/shield) and [Mongoid](https://github.com/mongoid/mongoid). If you haven't already, check out [Openredis](https://openredis.com/). It's awesome.

### Audience
Hopefully you're familiar with a Ruby framework. If not, Cuba is a fantastic gateway framework to understand the way of the web and how Ruby gets us writing expressive and readable code.

### Tools
- **Cuba**: Popular microframeworks like Sinatra and Ramaze will forever have a place in Rubyland. However, Cuba is concise and succinct for new developers to learn on the fly. At 200 lines of code and packed with Ruby idioms, it's a fun read and an awesome tool.
- **Mongoid**: Data migrations suck. Mongoid is an ODM that presents data like a JSON object.
- **Shield**: A dead simple solution for user authentication written in pure Ruby. It's roughly ~110 lines of code.

### Getting Started
Create a folder `auth_with_cuba` and fill your `app.rb` file.

**./app.rb**
```ruby
require 'cuba'

Cuba.define do
  on get do
    on root do
      res.write "We got routes baby"
    end

    on "login" do
      res.write "Login logic goes here"
    end
  end
end
```

`Cuba#define` takes a block and responds to route matchers. Notice our root and login routes are nested inside `get` blocks. Open a complimentary terminal and run `> ruby app.rb`. Nothing happens. That's because Cuba needs [Rack](http://rack.github.io/) and Rack responds to a rackup file. In the same directory, create a `config.ru` file. 

**./config.ru**
```ruby
require './app'

run Cuba
```

`Config.ru` tells Rack how to execute our app. In this instance, we'll require app file and run the Cuba namespeace. Run `> rackup config.ru` and point your browser to localhost:9292. Then, go to localhost:9292/login. Good, we got routes and rack setup. Next, we need Mongoid and a Mongo server to store some data.

Before we continue, let's track our gems in a Gemfile file:

**./Gemfile**
```ruby
source 'https://rubygems.org'

ruby '2.1.1'

gem 'cuba',    '~> 3.2.0'
gem 'mongoid', '~> 4.0.0'
```

Bundle your dependencies and let's keep trucking.

I'll refer you [here](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-os-x/#install-mongodb-with-homebrew) to get you started with Mongo. Start your Mongo server with `> mongod` and create a Mongoid config file.

**config/mongoid.yml**
```ruby
development:
  sessions:
    default:
      database: auth_with_cuba_development
      hosts: 
        - localhost:27017
```

You can read more on `config/mongoid.yml` [here](http://mongoid.org/en/mongoid/docs/installation.html). In a nutshell, we are hooking our app up to a Mongo database called `auth_with_cuba_development`. We can use Mongoid as an adapter to our app now.

**./app.rb**
```ruby
require 'cuba'
require 'mongoid'

ENV['RACK_ENV'] ||= "development"
Mongoid.load!("#{Dir.pwd}/config/mongoid.yml")

Cuba.define do
  on get do
    on root do
...
```

Mongoid requires an environment variable which we set to "development" and `Mongoid.load` looks to set up an adapter from Mongoid (our ODM) to your database (Mongo). Let's initialize a Mongo database to interact with. Start a Mongo session in a terminal with `> mongo` and instantiate a database.

**Mongo Console**
```ruby
> mongo
MongoDB shell version: 2.3.8
...
> show dbs
admin (empty)
local 0.078GB
> use auth_with_cuba_development
> show dbs
admin (empty)
local 0.078GB
auth_with_cuba_development (empty)
```

`use <name_of_database>` instantiates a new database if there isn't one. Sweet! Time to incorporate Shield and a user model. Create `user.rb` where we'll store our user logic. 

**models/user.rb**
```ruby
class User
  include Shield::Model
  include Mongoid::Document

  field :email, type: String
  field :crypted_password, type: String
  
  def self.fetch(identifier)
    where(email: identifier).first
  end

  def self.[](id)
    find(id)
  end
end
```

Let's break this down:

- Including `Shield::Model` to User equips our class with Shield's methods. Shield expects your user to have `User#fetch` and a `crypted_password` attribute to function properly.
- User#fetch uses Mongoid's where query to find the first instance of an email we desire.
- User `self.[](id)` fetches a user instance by id.

Updating our app file will wire our user model to the rest of the app.

**./app.rb**
```ruby
require 'cuba'
require 'mongoid'
require 'shield'

require './models/user'

ENV['RACK_ENV'] ||= "development"
Mongoid.load!("#{Dir.pwd}/config/mongoid.yml")

Cuba.define do
  on get do
    on root do
...
```

Our code is sufficient to start interacting with a fake user now. Open a console with `irb -r ./app`.

**IRB Console**
```ruby
> user = User.new(email: "foo@bar.com")
=> #<User _id: <LONG_SHA>, email: "foo@bar.com", crypted_password: nil>
> user.password = "foobar123"
=> "foobar123"
> Shield::Password.check("foobar123", user.crypted_password)
=> true 
> nil == User.authenticate("foo@bar.com", "foobar123")
=> true
> user.save
=> true
> nil == User.authenticate("foo@bar.com", "foobar123")
=> false
```

Fun fact: `-r` flag autoloads the file to our irb environment.

Let's double check we wrote to our database. Open mongo again with `> mongo auth_with_cuba_development`:

**Mongo Console**
```ruby
MongoDB shell version: 2.6.3
connecting to: auth_with_cuba_development

> db.users.find({email: "foo@bar.com"})
{ "_id" : ObjectId("54b43b4d416e64b333000000"), "email" : "foo@bar.com", "crypted_password" : "900923b82a97febe274" }
```

From here, you can create a login flow for your users. I recommend using [mote](https://github.com/soveran/mote) for template rendering. I'll follow up with another post soon.
