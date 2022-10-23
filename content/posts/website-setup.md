---
title: "🔧 Website Setup"
date: 2019-03-09
draft: false

tags: ["Software Engineering"]
---

How I set up this website using GitHub Pages, namecheap and Cloudflare

<!--more-->

{{< notice info >}}
I have migrated my website to [Hugo PaperMod](https://github.com/adityatelange/hugo-PaperMod).

Read the [Papermod Guide](https://adityatelange.github.io/hugo-PaperMod/posts/papermod/papermod-installation/#guide)
and [how to host on Github](https://gohugo.io/hosting-and-deployment/hosting-on-github/) for more info.
{{< /notice >}}

# GitHub Pages

[GitHub Pages](https://pages.github.com/) is a free and easy way to host a static website. It also works with [jekyll](https://jekyllrb.com/), making creating a personal blog very easy.

### Selecting a theme

There are a lot of different themes that you can choose from on [GitHub](https://github.com/topics/jekyll-theme).

In my case I pretty much chose the first one: [Minimal Mistakes](https://github.com/mmistakes/minimal-mistakes)
and used the [starter](https://github.com/mmistakes/mm-github-pages-starter/generate)
from its [guide](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/).

After setting `stradivari96.github.io` as the repository name and enabling github pages
in the settings, the website was up and running.

# Custom Domain

I wanted to use a custom domain name for my site, so I went to [namecheap](https://www.namecheap.com/) and purchased `xiang.dev` given that Google released this domain quite recently.

I used [Cloudflare](https://www.cloudflare.com/) to handle the redirect and SSL certificates for the site, it is completely free and quite easy to use.

This [guide](https://bart.degoe.de/free-ssl-on-github-pages-with-a-custom-domain/) by Bart de Goede is quite detailed and explains this step quite well.

You may also need to set up `A` records on Cloudflare pointing to the [IPs owned by GitHub](https://help.github.com/en/articles/setting-up-an-apex-domain#configuring-a-records-with-your-dns-provider).

**Note**: Don't forget to also change the nameservers of your domain to the ones provided by cloudflare.
