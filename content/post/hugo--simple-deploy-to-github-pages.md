+++
description = ""
date = "2015-02-26T16:28:14-08:00"
menu = "main"
title = "Hugo -- Simple Deploy to Github Pages"
+++

So, you migrated your coding blog content from Jekyll to Hugo. Right on! Hugo's speed performance makes it more attractive in my opinion. However, Jekyll's setup with Github pages is vastly superior to Hugo at first glance. With Octopress, it is as simple as `rake setup_github_pages` and your Jekyll blog is live in no time. Hugo has a [solution](http://gohugo.io/tutorials/github-pages-blog/) in a more verbose fashion. It's intuitive but daunting for beginner programmers looking for a succinct, straightforward approach.

Utilizing Git's ideas of branches and Hugo's folder structure, we can create a simple deploy and blogging workflow for Hugo with Github Pages. Shout out to [Codegangsta](https://github.com/codegangsta) for the sage advice. Here's my setup to a fresh Hugo repo deployed to GH Pages like Jekyll:

#### **Hugo GH Pages setup:**
1. Make sure Hugo is [installed](http://gohugo.io/overview/installing/) correctly.
2. Go to Github, create a new repository with this convention: *<your_handle>.github.io*. For clarity sake, my repo would be *andy4thehuynh.github.io*. Also, create a local instance of a hugo repo. `Cd` into an empty directory on your local machine and execute `hugo new site ./`. Initialize a git repo with `git init` and add your remote `git remote add origin git@github.com:<your_handle>/<your_handle>.github.io.git`. Cool, we have a fresh blog repo.  
3. Let's add a test post; execute `hugo new post/test.md` and `echo 'Your live on Github Pages' >> ./content/post/test.md`. Set the draft flag to true to make sure your post renders.
4. Tell Hugo to build your site by running `hugo`. Your public directory should be populated with a freshly generated site. Awesome!
5. Here comes the sauce; perform a `echo 'public' >> .gitignore`. Now, Git will have no idea of your public directory (your compiled public content users will view in a browser). You'll see why quickly.
6. Switch out of the master branch with `git checkout -b source`. We do this since GH pages doesn't care about our source code (aka our source branch). It only cares about the public content.
7. Add and commit your source changes. Do a `git add -A` and `git commit -m 'Initial Commit'`. Push your changes with `git push origin source`.
8. Lastly, `cd` into your public folder. Notice Git is not keeping track of changes here. This was for intended purposes. Do a `git init`, `git add -A` and `git commit -m 'Initial commit'`. Push your changes with `git push origin master`. 

Open a browser to your repo named *<your_handle>.github.io* and switch between your source and master branches. All your compiled content should be in your master branch. GH pages will see that and render it at `<your_handle>.github.io`. You'll write your drafts in your source branch. Compile it with the `hugo` command. When your happy with your compiled changes, push your public folder and become a rock star. Enjoy!
