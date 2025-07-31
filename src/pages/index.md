---
_schema: page
permalink: /
title: Rosey CloudCannon Connector
layout: layouts/page.html
eleventyNavigation:
  key: Home
  order: 0
tags: page
SEO_options:
  title:
  image:
  description:
draft: false
---

The Rosey CloudCannon Connector provides a way for non-technical editors to create and edit the `locales/*.json` files that [Rosey](https://rosey.app/) uses to generate a multilingual site. 


Translations are displayed to editors in a form-like interface.


![Screenshot of editing interface in CloudCannon](/assets/images/screenshot-editing.png)


## How it works

1. A developer tags HTML elements on your site for translation using `data-rosey` tags.

2. Rosey scans your built static site for `data-rosey` tags and generates a JSON file named `base.json`, containing information about your all of your tagged content.

3. The Rosey CloudCannon Connector generates YAML files which are displayed to editors in the CMS. Editors fill in translations.

4. These YAML files are turned into the `locales/*.json` files which Rosey needs to generate the multilingual site.

3. Rosey ingests the `locales/*.json` files, which contain original phrases paired with user entered translations. Using this data, and your tagged content, Rosey generates a complete multilingual site.


All of this generation of files happens in your site's postbuild, meaning it happens automatically each build.


## Is this workflow right for you?


Depending on your usecase this workflow could be unnecessary, and you would be better suited to simply dividing your different language content into separate directories and maintaining each separately. Read [this blog post](https://cloudcannon.com/blog/managing-multilingual-content-in-cloudcannon/) before getting starting with the RCC. 
