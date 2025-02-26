---
title: "Make a Website"
date: 2019-07-01T19:15:06
draft: false
toc: true
tags:
    - Website
    - Self-Hosted
---

## But why make a website?

If you can separate from platforms owned by the big companies i.e. Microsoft, Google, Amazon, Apple, and Facebook, you should. 

Independence from platforms owned by large companies, or even just companies in general, benefits you in many ways.

By making your own website, you set your own rules. Setting your own rules let's you *avoid collecting data from people reading your blogs*, 
allows you to share your blogs in your own custom URL, and avoids monetisation efforts by the large companies to profit from your work, without you receiving a cut off the profits.

Instead of limiting yourself to Facebook/Twitter/online blog generators layout, you get to make your own layout: the only limit when making your own website is your knowledge. 

Why limit yourself to some other websites options? There really isn't a reason to except convenience, and in addition there are lots of cons to doing it that way:

1. You limit yourself to the customisation options of that platform.
2. You are willingly handing over your data to that platform.
3. You are relying on their protocols to be secure enough to keep your information secure.
4. You are not learning anything new like you would if you made your own website.

### But how do I make a website?

You first need to learn HTML, which is quite simple. It's very intuitive. You have a range of headings (large to small), and paragraphs. You have the title which is what appears in the tab, and links. These are the basic building blocks of a website, and if you press F12 you can inspect the HTML of this website to see how it's built. There are many tutorials on how to use HTML and I'll link some at the bottom of this page. CSS is used to style your page, customise it's colors/widths/etc... Really you don't need much CSS, you can also view the styles of this page in F12. You can use javascript or php to make your website dynamic, but you don't have to.

You have to buy a VPS (they're usually around 5 dollars per month), and you need to set up nginx or apache2 web server. You also need to buy a domain name at around 10 dollars per year and learn how to link the DNS. To achieve HTTPS status (more secure http), you have to install a program called letsencrypt on your VPS and run it. It's pretty simple to do. I will include links to tutorials for everything mentioned here at the bottom of this page.

- <a href="https://www.learn-html.org/">Here</a>, <a href="https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web">Here</a>,
  and <a href="https://developer.mozilla.org/en-US/docs/Learn/HTML">Here</a> are some resources to help you learn HTML.
- <a href="https://www.codecademy.com/learn/learn-css">Here</a>, <a href="https://css-tricks.com/">Here</a>, and <a href="https://developer.mozilla.org/en-US/docs/Learn/CSS">Here</a> are some resources to help you learn CSS
- You can buy a VPS from <a href="https://www.linode.com/">Linode</a> or <a href="https://www.digitalocean.com/">DigitalOcean</a>. There are other VPS providers but these are really cheap and well known and popular (not sponsored)
- <a href="https://www.namecheap.com/">Namecheap</a> are a pretty popular domain name provider, I've not tried many but people seem to like this one.
- <a href="https://certbot.eff.org/lets-encrypt/ubuntufocal-apache">Certbot</a> will allow you to secure your website with letsencrypt https!
- <a href="https://my.bluehost.com/hosting/help/what-are-dns-records">Learn DNS record
  meanings</a> in order to be fully familiar with the registration process of a website.

So what are you waiting for? Go ahead and make that website!
