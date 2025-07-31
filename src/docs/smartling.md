---
_schema: page
permalink: /docs/smartling/
title: Smartling Integration
layout: layouts/page.html
eleventyNavigation:
  key: Smartling Integration
  order: 2
tags: page
SEO_options:
  title:
  image:
  description:
draft: false
---

This project allows for automatic AI-powered translations through Smartling. It uses the [Node.js SDK](https://help.smartling.com/hc/en-us/articles/4405992477979-Node-js-SDK). You must sign up for an account, and enter your Smartling credentials in the rcc configuration file and site's environment variables for this to work.

To add automatic AI-powered translations - which your editors can then QA in CloudCannon:

1. Add your secret API key to your environment variables with the key of `DEV_USER_SECRET` in CloudCannon on your staging site (or your only site if you're not using a publishing workflow). You can set this locally in a `.env` file if you want to test it in your development environment. Save your changes and wait for the build to finish.

2. Enable Smartling in your `rosey/rcc.yaml` file, by setting `enabled: true` under the Smartling section. 

3. Fill in your `dev_project_id`, and `dev_user_identifier`, with the values given to you by Smartling.

4. Save your changes. Your translations should now be sent to Smartling.

> [!CAUTION]
> Make sure to not push any secret API keys to your source control. Make sure the `.env` file is in your `.gitignore` if you are testing locally.

> [!CAUTION]
> **Be aware these translations have some cost involved**, so make sure you understand the pricing around Smartling machine-translations before enabling this.

## What gets sent to Smartling

A phrase will be sent away to Smartling for automatic translation if, in at least one language:
- there is no translation for the key, and
- we have not received a translation for the key in the past

> [!NOTE]
> If you continue using content for the translation key, as shown in the examples above, and change an original phrase (and don’t change the key yourself), the RCC will detect it's a new phrase because it sees a new key, and it will be sent away to Smartling. If you decide to use a different approach eg. with a fixed key, when the original content changes it won't be sent to Smartling, because the RCC won't detect its a new phrase.


We never archive the Smartling file just in case you disable Smartling, then re-enable it, or remove and then re-add a locale. This helps keep a record of all translations received by Smartling. They're kept in an object, where scaling shouldn’t be a big issue. This helps save money on translations, and avoid the issue where Smartling won't send anything  back if all of the original phrases it’s received have been translated by Smartling before. However, if you disable the Smartling integration and don’t think you’ll ever use it, you can simply delete these files and directories for tidiness if you like.

## Clearing a translation

If you clear a translation, it will be overwritten when the next Smartling call happens - aka when any new phrase is added, and has not been translated by Smartling before. This won’t be a problem if you’ve manually entered a different translation to the input. If you want to use the original untranslated text on the translated page, you cannot rely on the fallback as you could without Smartling, but must enter the original text yourself to avoid it being overwritten.
