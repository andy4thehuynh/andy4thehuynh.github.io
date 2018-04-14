---
layout: post
title:  "Github Pages Hosting using Git Branches"
date:   2015-03-16 13:43:48 -0800
categories: git
---

Utilizing Git's ideas of branches and Jekyll or Hugo blogging services, we can create simple deployment workflows with Github's free hosting. Shout out to [Codegangsta](https://github.com/codegangsta) for inspiring this post. My blog is powered by Hugo, below is my setup deploying a fresh Hugo repo to Github Pages.

We'll assume Hugo is installed correctly and you generated a local hugo repo. Setup an empty Github repo following the convention: **`<your_handle>.github.io`**.  

### Hugo to Github Pages
``` bash
>              cd ~/Code/hugo_repo 
>              git init
>git:(master)  echo 'public' >> .gitignore         # Git will ignore your public folder.
>git:(master)  git checkout -b source              # Github hosting don't care about our source code (aka source branch). It only cares about public facing content.
>git:(source)  git add -A  
>git:(source)  git commit -m "Github Pages will only look at my master branch anyways"
>git:(source)  git remote add origin git@github.com:<your_handle>/<your_handle>.github.io.git
>git:(source)  git push origin source 
>git:(source)  hugo                                # Generate your public folder.
              0 draft content
              0 future content
              1 pages created
              0 paginator pages created
              0 tags created
              0 categories created
              in 5 ms
>git:(source)  cd public/                          # Notice git isn't initialized here.
>              git init
>git:(master)  git add -A
>git:(master)  git commit -m "Github will render stuff in your public folder"
>git:(master)  git push origin master
```

Point your browser to your Github repo **(`<your_handle>.github.io`)** and toggle between source and master. All compiled content should live in master. Github pages will look at master and render it at `<your_handle>.github.io`. Your workflow consists of composing drafts source, compile with your `> hugo` command. When your happy with your compiled changes, push public and prosper.
