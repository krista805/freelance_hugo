+++
date = "2018-04-06T12:23:46-07:00"
title = "Shopify Theme Development Workflow"

+++

![SHopify Partners](/img/blog/shopify_banner_img.jpg)

This is a simple step-by-step workflow to set up a Shopify Development Environment from the command line. The idea is to keep a similar workflow to other dev environments. A similar workflow would mean managing the theme files locally by making changes and committing those changes with Git while running a command line tool to watch for changes and automatically update the theme on the Shopify store.

This post is a brief primer on setting up your Shopify dev environment. Please check out the  [Shopify Partner's docs](https://www.shopify.com/partners/blog/95401862-3-simple-steps-for-setting-up-a-local-shopify-theme-development-environment) for further details.

## Basic Installation from The Command line

**1. Install ThemeKit**

[Theme Kit](https://shopify.github.io/themekit/) is command line tool used to upload and manage files on Shopify store theme. If you have [Homebrew](https://brew.sh/), installed you can install Theme Kit by running the following commands:

     brew tap shopify/shopify
     brew install themekit

**2. Create a directory**

    mkdir STORE-NAME-HERE
    cd STORE-NAME-HERE

**3. Create** `config-example.yml` **file**

    vim config-example.yml

**4. Add the following to the** `config-example.yml` **file**

    development:
        store: example.myshopify.com
        password: add-password-in-config
        theme_id: "live"
        bucket_size: 40
        refill_rate: 2
        ignore_files:
            - "*.swp"
            - "*~"
            - "config/settings_data.json"


**5. Initialize Repo**

    git init

**6. Create a** `.gitignore` **file**
and add the following:

    config.yml
    config/settings_data.json

**7. Copy the** `config-example.yml` **to the file Theme Kit will need**

    cp config-example.yml config.yml.

**8. Create a private app within the Shopify store admin**

This will yield a password to add to `config.yml`.

![Private App](/img/blog/shopify_app.png)

In Admin API Permissions, make sure the “Theme templates and theme assets” is set to Read and write

![Permissions](/img/blog/shopify_permissions.jpg)

**9. Now go ahead and set the store URL and password in** `config.yml`

    development:
        password: ENTER-SHOPIFY-PRIVATE-APP-PASSWORD
        theme_id: "live"
        store: YOUR-STORE.myshopify.com

**10. Next you need to pull down the theme from Shopify**   

        theme download
Note: If you receive an error saying you cannot load any valid environments, configure the theme like this before downloading the theme:

    Theme configure --password=[your-password] --store=[your-store.myshopify.com] -- example.myshopify.com password: add-password-in-config --themeid=[your-theme-id]

**11. Make first commit**

Once the theme is downloaded and the Git repository has been initialized, go ahead and create the first commit.

Go ahead and add all the files with:

    git add --all and

make a commit with:

    git commit -m "enter your first commit message here"

From here, you can now push up your code to a repo management platform such as Github or Bitbucket

**12. Lastly, you want to give Theme Kit either of the following commands to push updates to your store**

Watch will start a process that will watch your directory for changes and upload them to Shopify.

    theme watch
or

Upload will update all of the current file states to Shopify.

    theme upload

## Multiple Environment Configurations
It's possible to create multiple environments for your Shopify store. This comes in handy, especially if you are looking to make changes in a development environment and have a separate production store to push these changes live. Each environment needs it's own Shopify store to in order to obtain separate private app credentials. Once you create a separate store, you must update the `config-example.yml` file to:

    SHARED: &SHARED
        store: example.myshopify.com
        password: test
        theme_id: 'live'
        bucket_size: 40
        refill_rate: 2
        ignore_files:
            - "*.swp"
            - "*~"
            - "config/settings_data.json"

    development:
        <<: *SHARED
        password: ADD_PASSWORD_HERE
        store: TEST_STORE_NAME.myshopify.com

    production:
        <<: *SHARED
        password: ADD_PASSWORD_HERE
        store: LIVE_STORE_NAME.myshopify.com


Then you would specify which environment you want to watch for changes:

    theme watch --env development

    or

    theme upload --env production

## Closing thoughts

Again, this is a quick set up guide to get you up and running with the default Shopify theme. I came across a few other articles that helped me:

- [3 simple steps for setting up a local Shopify theme development environment](https://www.shopify.com/partners/blog/95401862-3-simple-steps-for-setting-up-a-local-shopify-theme-development-environment)
- [Shopify Theme Development Workflow](http://www.brettchalupa.com/shopify-theme-development-workflow)
- [Shopify Theme Development](https://robots.thoughtbot.com/shopify-theme-development)
