---
title: Blackcurrant Jekyll Theme
image: /assets/images/blackcurrant.jpg
author: John Doe
categories:
  - projects
layout: post
---

<small>Image Credits:<a href="https://www.fruitlayer.com"> Fruit Layer</a></small>

{{site.title}} is a Jekyll theme suitable for personal, blog, resume or portfolio websites.

## Features

### Easy customizations

Customizing the theme elements has been made easy. Almost all the customizable elements can be found in one file `_data/main.yml`.

Here is the sample.

{% highlight yml %}

# Image, name, profession in the sidebar

image: /assets/images/author-image.jpg
name: John Doe
profession: A Web Developer
email: john@doe.com # The contact form submission be received in this email address.

# Menu items in the sidebar

menu:

- title: Projects
  link: /projects/

- title: Blog
  link: /blog/

- title: Contact
  link: /contact/

# Social icons in the sidebar

# use font awesome icon code in the 'icon:' value

social:
facebook:
icon: fa-facebook-square
link: https://facebook.com

twitter:
icon: fa-twitter
link: https://twitter.com

linkedin:
icon: fa-linkedin
link: https://linkedin.com

github:
icon: fa-github
link: https://github.com

# Your expertise on homepage

expertise:

- title: HTML5
  level: 45
  color: rgb(247, 205, 182)

- title: CSS3
  level: 65
  color: rgb(113, 178, 222)

- title: jQuery
  level: 50
  color: rgb(80, 185, 185)

- title: PHP
  level: 40
  color: rgb(124, 142, 222)

- title: WordPress
  level: 75
  color: rgb(160, 158, 158)

- title: SEO
  level: 90
  color: rgb(112, 195, 148)

- title: Photoshop
  level: 70
  color: rgb(230, 133, 172)

{% endhighlight %}

### Highlight Your Projects

Adding projects to your portfolio is similar to adding blog posts. You can now add more content to individual projects.

Add new projects inside `_projects` directory. It will be automaticaly listed in the projects page.

### Bootsrap 4

{{site.title}} is built on the latest Bootstrap 4 Framework. The theme can be easily customized using bootstrap elements.

### Attractive Layout

A dedicated and constant sidebar highlights the author, which is ideal for **personal branding**.

### Superfast Loading Speed

The website is light and superfast. It loads completely within 3 seconds in most cases. [Check Speed](https://developers.google.com/speed/pagespeed/insights/?url=https%3A%2F%2Fblackcurrant.jekyll-themes.com%2F){: target="\_blank" rel="nofollow" }.

### Multiple color schemes

The default layout boasts a vibrant green color. You can always change this in the configuration file or add your own color and the whole site changes to that scheme.

{% highlight YML  %}
color-scheme: '#1abc9c' # Green

# color-scheme: '#2693e6' # Blue

# color-scheme: '#987ce6' # Purple

# color-scheme: '#ea67a8' # Pink

# color-scheme: '#ec632b' # Orange

{% endhighlight %}

### Responsive videos

Just add a class to your video iframe to make it responsive. For example

{% highlight html %}

<iframe class="video" src="https://www.youtube.com/embed/YE7VzlLtp-4"></iframe>
{% endhighlight %}

Adding `video` class to any iframe makes it responsive.

<iframe class="video" src="https://www.youtube.com/embed/YE7VzlLtp-4?start=53&rel=0" allowfullscreen frameborder="0"></iframe>

### Paginated posts

Only 4 posts are shown in the blog page. Older posts are paginated. [Check it out](/blog){: target="\_blank"}. You can change the number of posts in the configuration file.

`paginate: 4`

### Automatic Breadcrumbs

Breadcrumbs are generated for every page and post automatically. The default one looks like this.

{% include breadcrumbs.html %}

The color changes with the color scheme.

### Auto generated TOC

Table of contents is automatically generated for each post.

### Comments

Disqus comments is pre-installed. Just sign-up, get a shortname and update the variable `disqus` in the configuration.

If you do not mention the `disqus:` value in configuration then the disqus comments code will not be included in the website.

### Track visitors

The website uses Google Analytics for tracking visitors. Use your own UA code in the configuration. Analytics code will not be used in the website if you do not mention UA code.

### MathJax Support

Now render beautiful math formulas by enabling MathJax in the configuration (enabled by default).

$$\sum_{i=0}^n i^2 = \frac{(n^2+n)(2n+1)}{6}$$

### Instant search

Search anything from your articles in an instant using search bar at the top.

### Auto generated feed and sitemap

We have given much importance to SEO and made sure you have the [sitemap]({{site.baseurl}}/sitemap.xml){: target="\_blank"} ready to submit to search engines. A well formatted [feed]({{site.baseurl}}/feed.xml){: target="\_blank"} is readily available for RSS.

### Fully responsive

It will be a pleasure reading content on {{site.title | downcase}} through a smartphone. Try it to know it.

### Chart.js Support

Use amazing charts using Chart.js. Enable Chart.js in the configuration(enabled by default)

<canvas id="myChart" width="400" height="200"></canvas>

<script>
var ctx = document.getElementById("myChart");
var myChart = new Chart(ctx, {
    type: 'bar',
    data: {
        labels: ["Red", "Blue", "Yellow", "Green", "Purple", "Orange"],
        datasets: [{
            label: 'Colors',
            data: [6, 5, 4, 3, 2, 1],
            backgroundColor: [
                'rgba(255, 99, 132, 0.2)',
                'rgba(54, 162, 235, 0.2)',
                'rgba(255, 206, 86, 0.2)',
                'rgba(75, 192, 192, 0.2)',
                'rgba(153, 102, 255, 0.2)',
                'rgba(255, 159, 64, 0.2)'
            ],
            borderColor: [
                'rgba(255,99,132,1)',
                'rgba(54, 162, 235, 1)',
                'rgba(255, 206, 86, 1)',
                'rgba(75, 192, 192, 1)',
                'rgba(153, 102, 255, 1)',
                'rgba(255, 159, 64, 1)'
            ],
            borderWidth: 1
        }]
    },
    options: {
        scales: {
            yAxes: [{
                ticks: {
                    beginAtZero:true
                }
            }]
        }
    }
});
</script>

### Other simple stuff

We have given special importance in designing things like reading time, tags, share buttons, recent articles etc...

## Style Guide

### Typography

This is how the headlines will look like on this website.

<h1>This is a H1</h1>
<h2>This is a H2</h2>
<h3>This is a H3</h3>
<h4>This is H4, H5 and H6</h4>

{% highlight html %}

<h1>This is a H1</h1>
<h2>This is a H2</h2>
<h3>This is a H3</h3>
<h4>This is H4, H5 and H6</h4>

{% endhighlight %}

<p></p>

These are sample paragraphs showing _italics_, **bold** and `code` text style. Fonts are carefully choosen for longer user retention. The text is optimized for legibility. Viewers will get a good experience reading through the blog.

Here is an unordered list

- Item 1
- Item 2
- Item 3

and an ordered list

1. Item 1
2. Item 2
3. Item 3

### Buttons

<button class="btn btn-default">Default</button>
<button class="btn btn-primary">Primary</button>
<button class="btn btn-success">Success</button>
<button class="btn btn-warning">Warning</button>
<button class="btn btn-danger">Danger</button>

<br>

### Syntax Hihglighting

{% highlight html %}

<button class="btn btn-default">Default</button>

<button class="btn btn-primary">Primary</button>

<button class="btn btn-success">Success</button>

<button class="btn btn-warning">Warning</button>

<button class="btn btn-danger">Danger</button>

{% endhighlight %}

### Blockquote

> This is an important sentence from the paragraph.

> Another blockquote.
>
> With multiple lines!

### Table

This is a simple markdown table

| Tables        |      Are      |   Cool |
| ------------- | :-----------: | -----: |
| col 3 is      | right-aligned | \$1600 |
| col 2 is      |   centered    |   \$12 |
| zebra stripes |   are neat    |    \$1 |

<br>

{% highlight html %}
| Tables | Are | Cool |
| ------------- |:-------------:| -----:|
| col 3 is | right-aligned | $1600 |
| col 2 is | centered | $12 |
| zebra stripes | are neat | \$1 |
{% endhighlight %}

## Installation Guide

Right after the purchase, you will get a zip folder with the following files.
{% highlight html %}
.
.
├── \_data
| ├── main.yml
|
├── \_includes
| ├── author.html
| └── header.html
| └── footer.html
| .
| .
|
├── \_layouts
| ├── default.html
| └── post.html
|
├── \_posts
| ├── {{page.path | remove: '_posts/'}}
| └── 2009-04-26-title-of-post.md
|
├── \_pages
| ├── about.md
| └── contact.md
| .
| .
|
├── \_sass
| └── \_auhtor.scss
| └── \_google-fonts.scss
| .
| .
|
├── \_config.yml
├── assets
├── blog
├── images
└── index.html

{% endhighlight %}

You can serve this locally using the command `bundle exec jekyll serve`.

Make necessary changes in the **\_config.yml**, **\_data/main.yml** files.

All these files can be put in a repository(GitHub, GitLab etc) or hosting service where Jekyll is supported.

If Jekyll is not supported, then use the **\_site/** folder, which is actually a complete rendered website in itself.

Do contact us for any help - `hello@webjeda.com`.

With all these goodness, you will also get full support for 3 months.

## Buy {{site.title}} Jekyll Theme

{% include buy-button.html %}

**We provide 3 months support.**

<span class="right"><strong>✓</strong></span> Get your questions answered within 24 hours.

<span class="right"><strong>✓</strong></span> Get assistance with reported bugs and issues.

<span class="right"><strong>✓</strong></span> Help with included 3rd party assets.

You can always leave us an email at `hello@webjeda.com`.
