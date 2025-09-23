+++
title = 'Hugo Usage'
date = 2025-09-23T21:27:23+03:00
draft = false
tags = ["hugo", "blog"]
categories = [""]
series = ["Hugo Basics"]
+++

Here are my notes for blogging with hugo. 
- When deploying a Hugo site to GitHub Pages, you can either let GitHub build your site automatically or publish only the generated files. Both approaches work — it’s just a matter of automation versus manual control.

  - **Option 1 (I used):** Keep only the Hugo source in your repo, ignore `public/` and `resources/`, and let GitHub Actions build + deploy automatically to GitHub Pages. 
  - **Option 2:** Manually run `hugo`, commit/push the generated `public/` folder (either in the same repo or a separate one) to the branch that GitHub Pages serves.  

- When you clone your Hugo site repository, the `themes` folder may appear empty because themes added as submodules are not pulled by default. To fix this, you need to add and initialize the theme submodule using the commands below, which will populate the `themes/devise` folder and make your site content visible.  
  {{< highlight html >}}
  git submodule add https://github.com/austingebauer/devise.git themes/devise 
  git submodule update --init --recursive
  {{< /highlight >}}

- To create new blog post we can use this command.
  {{< highlight html >}}
  hugo new content/post/2025-08/hugo-usage.md
  {{< /highlight >}}
Any `.md` file placed under the `content/post` folder will appear as a blog post on your site. Make sure each file starts with a front matter header so Hugo can render it correctly.  

- You can then build the website locally by running
  {{< highlight html >}}
  hugo serve
  {{< /highlight >}}

