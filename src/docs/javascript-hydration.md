---
_schema: page
permalink: /docs/javascript-hydration/
title: JavaScript Hydration
layout: layouts/page.html
eleventyNavigation:
  key: JavaScript Hydration
  order: 2
tags: page
SEO_options:
  title:
  image:
  description:
draft: false
---
The RCC supports any static site (likely an SSG) which Rosey would support. Anything that relies on JS hydration for text values will need workarounds outside of the default workflow. Rosey scans your static HTML for elements with `data-rosey` tags and the text those elements contain. If that text is overwritten by JS the translated text will get clobbered by the untranslated JS values. A couple of workaround ideas:

- Import the locales/*.json file data. Add some logic in your JavaScript to detect which locale you are in via the page's URL. Add more logic to use the translated value from the appropriate locale file if it exists, rather than the original text. [See an example here](https://github.com/tomrcc/rosey-and-react-demo).

- Hardcode the translated values alongside the original text wherever it is defined in your JS. Then detect whichever locale you are in using the page's url, and use the appropriate values in your JS to hydrate the element with the correct translated text.

- Source the text for your JS via a JSON file. Rosey supports translation of JSON files, so you could detect which locale you are in and ingest whichever JSON file corresponds to that locale. Translation of JSON files is not yet supported in the RCC (see below).

> [!NOTE]
> The RCC does not provide any support for the generation of these JSON files yet, but you can write them as you would using Rosey without the RCC - by hand or with your own middleware. If you would like the RCC to support translation of JSON files, please leave an issue on the repository so we can gauge interest.
