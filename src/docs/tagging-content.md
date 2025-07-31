---
_schema: page
permalink: /docs/tagging-content/
title: Tagging content
layout: layouts/page.html
eleventyNavigation:
  key: Tagging content
  order: 2
tags: page
SEO_options:
  title:
  image:
  description:
draft: false
---

## Manual tagging

To add text content to translate, start tagging your layouts and components with `data-rosey` tags.


Some SSGs - like Eleventy - come with a `slugify` global filter, which you can use to turn the tagged element's text content into an id friendly slug, and use that as the translation key. 


An example tag in [Eleventy](https://www.11ty.dev/) may look like:

```liquid
  <h1 class="heading" data-rosey="{{ heading.heading_text | slugify }}">{{ heading.heading_text }}</h1>
```


If you are using an SSG that doesn't have a `slugify` filter built in - like Astro - you can import a helper function which has been provided in `rosey-cloudcannon-connector/utils`.

An example tag in [Astro](https://astro.build/) may look like:

```html
  ...
  import { generateRoseyId } from "rosey-cloudcannon-connector/utils";

  const { heading } = Astro.props;
  ---

  <h1 class="heading" data-rosey={generateRoseyId(heading.heading_text)}>
    {heading.heading_text}
  </h1>
```

## Automatic tagging

Most sites using an SSG will have at least some content that comes from markdown, which gets run through a `markdownify` filter, or somehow turned from markdown into HTML. 


Any body content in a markdown content file will go through this process as part of the SSG's build. Likewise, some components will have markdown text content as the value of it's props. Both are turned into the HTML needed by your actual live site as part of your build. This markdown content is hard to add `data-rosey` tags to manually.


For this the `rosey-tagger` has been provided. In the [Getting Started](#getting-started) steps, we added it to the CloudCannon `postbuild` so that it runs after every build, but before the rest of the Rosey workflow. 


Tag a parent element with the attribute `data-rosey-tagger` and block elements inside of that parent element will be tagged for translation automatically. The most nested block element will be the one to receive the tag, so that there is never a `data-rosey` tag inside of a `data-rosey` tag.


This is primarily used to tag your markdown, wherever that lives - be it in your layouts or components - but can be also be used on any element. For example you could add it to the `<body>` tag of a page for every block level element in that page to be tagged automatically. 


You shouldn't nest one `data-rosey-tagger` inside of another, however it should respect existing tags you've added manually, and not overwrite them. 


> [!IMPORTANT]
> When using the `rosey-tagger` with markdown, add a Rosey namespace of `data-rosey-ns="rcc-markdown"` on the element containing markdown, so that generated inputs for that content are `type: markdown` in CloudCannon, which will allow editors the same options in the translation input as are allowed for the original content.


If you don't have one of these `data-rosey-tagger` tags on any of your pages it won't do anything, so can be ignored or removed. If no translation is provided for an element, the original will be used. This means even if you tag everything, but don't want to provide a translation for it, the original will be shown in your translated version, rather than a blank space.

## Namespace pages

Sometimes you don't want a piece of content that appears many times on different pages to be represented on each translation page. Duplicate translation keys (translations with the same ID) across pages *will* be kept in sync with each other, but it can clutter up your translation files. The most common example would be text used in your header and footer navigation.


To solve this issue, you can add a [Rosey namespace](https://rosey.app/docs/namespacing/). Add the attribute `data-rosey-ns="common"` to any element and any children with tags inside of that element will receive the namespace. This will cause any translations that are nested under this namespace to appear on a separately generated page, and be ommited from the normal translations for that page. 


By default the value `common` is used as the value for this namespace, as defined in the `rcc.yaml` file under the key `namespace_pages`. You can change this to whatever name you like, or add more than one namespace page.


You cannot use the namespace `rcc-markdown` for a namespace page, as that is reserved for use by the RCC to nominate which inputs should be markdown.


An example for a header component in Astro might look like:

```jsx
---
import { generateRoseyId } from "rosey-cloudcannon-connector/utils";

const { links } = Astro.props;
---

<header
  data-rosey-ns="common">
  <ul>
    {
      links.map((link) => {
        return (
          <li>
            <a
              href={link.link}
              data-rosey={generateRoseyId(link.text)}>
              {link.text}
            </a>
          </li>
        );
      })
    }
  </ul>
</header>
```