---
_schema: page
permalink: /docs/
title: Getting Started
layout: layouts/page.html
eleventyNavigation:
  key: Getting Started
  order: 1
tags: page
SEO_options:
  title:
  image:
  description:
draft: false
---
## CloudCannon setup

### Option 1: A single site

You can use this option if you don't need the default redirect that comes with Rosey. Your original content will be served at the root of your URL, without a prefix.

1. Create a site on CloudCannon.

2. Add the environment variable `SYNC_PATHS`, with the value `/rosey/`.

3. Install the RCC by running `npm i rosey-cloudcannon-connector` in the root of your repository.

4. Add a `.cloudcannon` directory in the root of your project if you don't have one already. Add a `postbuild` file to it, replacing `dist` with the build output directory of your project. If you already have a CloudCannon postbuild file, add this logic to your current one.

  `.cloudcannon/postbuild`

  ```bash
    #!/usr/bin/env bash

    npx rosey-cloudcannon-connector tag --source dist
    npx rosey generate --source dist
    npx rosey-cloudcannon-connector generate

    echo "Translating site with Rosey"
    mv ./dist ./untranslated_site                  
    npx rosey build --source untranslated_site --dest dist --default-language-at-root
  ```


### Option 2: A staging to production workflow

This option is for you if you want to use the default redirect that comes with Rosey, and don't mind your original language being prefixed with a locale code. For example if your original untranslated content is in English, it would be served at `/en/`, like all the other translated locales.

1. Create two sites using a `staging` -> `production` [publishing workflow](https://cloudcannon.com/documentation/articles/what-is-a-publish-branch/) on CloudCannon, if you don't already have one.

2. On your `staging` site add the [environment variable](https://cloudcannon.com/documentation/articles/configure-your-environment-variables/) `SYNC_PATHS`, with the value `/rosey/`.

3. On your `production` site add the environment variable `ROSEYPROD`, with a value of `true`.

4. On staging, install the RCC by running `npm i rosey-cloudcannon-connector` in the root of your repository.

5. On staging, add a `.cloudcannon` directory in the root of your project if you don't have one already. Add a `postbuild` file to it, replacing `dist` with the build output directory of your project. If you already have a CloudCannon postbuild file, add this logic to your current one.

  `.cloudcannon/postbuild`

  ```bash
    #!/usr/bin/env bash

    if [[ $ROSEYPROD != "true" ]];
    then
      npx rosey-cloudcannon-connector tag --source dist
      npx rosey generate --source dist
      npx rosey-cloudcannon-connector generate
    fi

    if [[ $ROSEYPROD == "true" ]];
    then
      node rosey-tagger/main.mjs --source dist
      echo "Translating site with Rosey"
      mv ./dist ./untranslated_site                  
      npx rosey build --source untranslated_site --dest dist 
    fi
  ```

## Rest of the setup

1. Run `npx rosey-cloudcannon-connector generate-config` in the terminal in the root of your directory to generate a config file. If you want you can just skip to the next step and one will be generated for you on the next CloudCannon build.

2. Add a `translations` collection to your `cloudcannon.config.yml`. If you have the [collection_groups](https://cloudcannon.com/documentation/articles/configure-your-site-navigation/#options) configuration key defined, add `translations` to a collection group, so that it is visible in your sidebar in CloudCannon. 

    `cloudcannon.config.yml`

    ```yml
      collections_config:
        translations:
          path: rosey
          icon: translate
          disable_url: true
          disable_add: true
          disable_add_folder: true
          disable_file_actions: false
          glob:
            - rcc.yaml
            - 'translations/**'
          _inputs:
            urlTranslation:
              type: text
              comment: Provide a translated URL, and Rosey will build this page at that address.
    ```

> [!NOTE]
> If your site is nested in a subdirectory and you're using the [source](https://cloudcannon.com/documentation/articles/configuration-file-reference/#source) key you'll need to remove it, and manually add the subdirectory to each of the paths that were relying on it. The `rosey` collection's path does not need the prefix of the subdirectory since it lives in the root of our project. Schema paths in CloudCannon are not affected by the `source` key, so do not need updating.

3. Commit and push your changes, and wait for the CloudCannon build to finish. Then pull your changes down to your local. 

    All the files you need to get going will have been generated as part of the CloudCannon build. 

> [!NOTE]
> Make sure there is **at least one locale code to the `locales` array** in the `rosey/rcc.yml` file. Remember to replace the placeholder urls with your own if using either of the link features.

> [!NOTE]
> Remember to replace the placeholder urls with your own if using either of the link features. 


You are ready to start tagging content for translation.