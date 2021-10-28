---
title: 🔧 Website Setup
excerpt: "How I set up this website using GitHub Pages, namecheap and Cloudflare"
read_time: true
share: true
related: true
comments: true
tags: [Github Pages, namecheap, Cloudflare]
gallery:
  - image_path: https://cdn-images-1.medium.com/max/1200/1*osyaA6QwQra6llfoFYAOkw.png
    alt: "placeholder image 1"
    title: "Image 1 title caption"
  - image_path: https://cdn-images-1.medium.com/max/600/1*ThcllHZuDgpCMUCjYLqQag.png
    alt: "placeholder image 2"
    title: "Image 2 title caption"
---

As my first post I wanted to write about how I set up this website using GitHub Pages, namecheap and Cloudflare.

# GitHub Pages

[GitHub Pages](https://pages.github.com/) is a free and easy way to host a static website. It also works with [jekyll](https://jekyllrb.com/), making creating a personal blog very easy.

{% include gallery %}

### Selecting a theme

There are a lot of different themes that you can choose from on [GitHub](https://github.com/topics/jekyll-theme).

In my case I pretty much chose the first one: [Minimal Mistakes](https://github.com/mmistakes/minimal-mistakes) and used the [starter](https://github.com/mmistakes/mm-github-pages-starter/generate) from its [guide](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/).

You should see your website up and running at this point.

# Custom Domain

I wanted to use a custom domain name for my site, so I went to [namecheap](https://www.namecheap.com/) and purchased `xiang.dev` given that Google released this domain quite recently.

I used [Cloudflare](https://www.cloudflare.com/) to handle the redirect and SSL certificates for the site, it is completely free and quite easy to use.

This [guide](https://bart.degoe.de/free-ssl-on-github-pages-with-a-custom-domain/) by Bart de Goede is quite detailed and explains this step quite well.

You may also need to set up `A` records on Cloudflare pointing to the [IPs owned by GitHub](https://help.github.com/en/articles/setting-up-an-apex-domain#configuring-a-records-with-your-dns-provider).

{% include figure image_path="/assets/images/cloudflare.png" %}

**Note**: Don't forget to also change the nameservers of your domain to the ones provided by cloudflare, which in my case are:

{% include figure image_path="/assets/images/namecheap-dns.png" %}
