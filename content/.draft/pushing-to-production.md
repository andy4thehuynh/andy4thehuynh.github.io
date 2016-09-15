+++
draft = true
author = "Andy Huynh"
date = "2015-06-25T17:26:11-07:00"
title = "Pushing to production at Kajabi"
+++

- Update production

- git merge master

git lg (make sure its looks right

- git push origin production



PUSH TO STAGING FIRST

Check if heroku is running on all cylinders: `heroku status` — Good rule of thumb. we want all greens. Don’t deploy if you see red. 

git push staging production:master && heroku run rake db:migrate -r staging && heroku restart -r staging

Poke around staging to see if the pipeline is in tact.

http://app.newkajabi-staging.com/

dev@kajabi.com
foobar123


PUSH TO PRODUCTION

git push staging production:master && heroku run rake db:migrate -r staging && heroku restart -r staging

wait around honey badger for errors.


heroic releases -r production 
 - Check your production branch and make sure the last sha matches your latest deployed sha (just good practice)


Unknown column errors are normal
