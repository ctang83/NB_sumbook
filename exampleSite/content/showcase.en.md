---
title: Configuration
weight: 90
---


1. 修改设置，到[原网站寻找线索](https://learn.netlify.app/en/cont/markdown/)
1. 图片设置大小：百分比、像素

```
![数系统5层](https://raw.githubusercontent.com/ctang83/NB_img/main/numbersys.png?width=25pc)

![自然数-整数-有理数-实数-复数](https://raw.githubusercontent.com/ctang83/NB_img/main/自然数整数有理数实数复数.png?width=600px)

![Map of Math](https://raw.githubusercontent.com/ctang83/NB_img/main/mapmath.jpg)
```

## Global site parameters

On top of [Hugo global configuration](https://gohugo.io/overview/configuration/), **Hugo-theme-learn** lets you define the following parameters in your `config.toml` (here, values are default).

Note that some of these parameters are explained in details in other sections of this documentation.

```toml
[params]
  # Prefix URL to edit current page. Will display an "Edit this page" button on top right hand corner of every page.
  # Useful to give opportunity to people to create merge request for your doc.
  # See the config.toml file from this documentation site to have an example.
  editURL = ""
  # Author of the site, will be used in meta information
  author = ""
  # Description of the site, will be used in meta information
  description = ""
```

## Activate search

If not already present, add the follow lines in the same `config.toml` file.

```toml
[outputs]
home = [ "HTML", "RSS", "JSON"]
```

Learn theme uses the last improvement available in hugo version 20+ to generate a json index file ready to be consumed by lunr.js javascript search engine.

> Hugo generate lunrjs index.json at the root of public folder.
> When you build the site with `hugo server`, hugo generates it internally and of course it doesn’t show up in the filesystem

## Mermaid

The mermaid configuration parameters can also be set on a specific page. In this case, the global parameter would be overwritten by the local one.

> Example:
>
> Mermaid is globally disabled. By default it won't be loaded by any page.  
> On page "Architecture" you need a class diagram. You can set the mermaid parameters locally to only load mermaid on this page (not on the others).

You also can disable mermaid for specific pages while globally enabled.

## Home Button Configuration

If the `disableLandingPage` option is set to `false`, an Home button will appear
on the left menu. It is an alternative for clicking on the logo. To edit the
appearance, you will have to configure two parameters for the defined languages:

```toml
[Lanugages]
[Lanugages.en]
...
landingPageURL = "/en"
landingPageName = "<i class='fas fa-home'></i> Redirect to Home"
...
[Lanugages.fr]
...
landingPageURL = "/fr"
landingPageName = "<i class='fas fa-home'></i> Accueil"
...
```

If those params are not configured for a specific language, they will get their
default values:

```toml
landingPageURL = "/"
landingPageName = "<i class='fas fa-home'></i> Home"
```

[Hugo-theme-learn](http://github.com/matcornic/hugo-theme-learn) is a theme for [Hugo](https://gohugo.io/), a fast and modern static website engine written in Go. Where Hugo is often used for blogs, this multilingual-ready theme is **fully designed for documentation**.


