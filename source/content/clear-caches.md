---
title: Clearing Caches for Drupal and WordPress
description: Learn how to clear cache for Drupal and WordPress sites on Pantheon's Global CDN.
tags: [cacheapp]
categories: []
---
Pantheon extends the core functionality of caching mechanisms within WordPress and Drupal so that caches are cleared within the site's frame and from our [Global CDN](/docs/global-cdn/) service.

## Granular Cache Clearing
Use the Pantheon Advanced Page Cache [plugin](https://wordpress.org/plugins/pantheon-advanced-page-cache/) or [module](https://www.drupal.org/project/pantheon_advanced_page_cache) to automatically clear caches for related pages as content is updated. No configuration needed, simply install and enable to take advantage of the granular cache clearing capabilities of the Global CDN.

Lower-level documentation for custom implementation of caching strategies is also coming soon. Please [contact our enablement team](https://pantheon.io/agencies/learn-pantheon?docs) if you are interested in implementing a custom cache strategy.

## Full Cache Clearing
We refer to this as "full cache clearing" because the operation removes all pages from cache at once. There are three methods you can use to clear all caches for a given [environment](/docs/pantheon-workflow/) of a site:

- Pantheon Dashboard
- Site Admin
- Command Line

**Note:** To clear the Redis cache, please see our [Redis](/docs/redis/#clear-cache) doc.

### Pantheon Dashboard
1. Navigate to the Site Dashboard and select the desired environment.
2. Click the **Clear Caches** button:

  ![Clear Cache Button](/source/docs/assets/images/clear-cache-button.png)

### Site Admin

<TabList>

<Tab name="WordPress" id="wp-1">
1. Navigate to the Site Dashboard and select the desired environment.
2. Click the **Site Admin <span class="glyphicons glyphicons-new-window-alt"></span>** button and login.
3. Navigate to **Settings** > **Pantheon Page Cache**, then click **Clear Cache**:

  ![WordPress Clear Caches](/source/docs/assets/images/wp-cc.png)

</Tab>

<Tab name="Drupal 8" id="d8-2">

1. Navigate to the Site Dashboard and select the desired environment.
2. Click the **Site Admin <span class="glyphicons glyphicons-new-window-alt"></span>** button and login.
3. Navigate to **Configuration** > **Development** > **Performance**, then click **Clear all caches**:

  ![Drupal 8 Clear Caches](/source/docs/assets/images/d8-cc.png)

</Tab>


<Tab name="Drupal 7" id="d7-3">

1. Navigate to the Site Dashboard and select the desired environment.
2. Click the **Site Admin <span class="glyphicons glyphicons-new-window-alt"></span>** button and login.
3. Ensure the [Pantheon Platform API](/docs/modules#pantheon-module-drupal-7) module is enabled:

  ![Pantheon Platform API module](/source/docs/assets/images/pantheon-platform-api-enabled.png)

4. Navigate to **Configuration** > **Development** > **Performance**, then click **Clear all caches**:

  ![Drupal 7 Clear Caches](/source/docs/assets/images/d7-cc.png)

</Tab>

</TabList>


### Command Line
If you're working from a terminal prompt or looking to automate parts of your workflow, use [Terminus](/docs/terminus/) to clear caches from the command line:

```bash
terminus env:clear-cache site.env
```

Replace `site` and `env` with your site name and the correct environment.

## Troubleshooting
### Clear Cache Fails in Dashboard or Terminus
If you see a notification on the Dashboard indicating a failure to clear cache, this is usually due to a PHP error, redirect, or other code-related issue. While the Dashboard notification may not help much to debug, running the same command via [Terminus](/docs/terminus/) will likely provide actionable information.

Some good troubleshooting first steps include:

 - Commenting out redirections in `settings.php`, `wp-config.php`, or elsewhere.

 - Resolve any fatal PHP errors.