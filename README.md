# app-magento

## Local development
Follow the instructions provided by [this repo](https://github.com/andreaskoch/dockerized-magento) to set up a local docker instance running Magento.

You may encounter an issue the first time you attempt to login to the admin area. If so, follow these instructions:

```bash
./magento magerun cache:dir:flush
./magento magerun cache:clean
./magento magerun index:reindex:all

./magento enter dockerizedmagento_cache_1

# From within the docker container:
redis-cli flushall
```

### Printing Errors
By default, Magento does not print out errors on the error screen for 'safety'. To enable error printouts, do the following:

```bash
cd ~/path/to/dockerized-magento
mv web/errors/local.xml.sample web/errors/local.xml
```

### Symlinking
When a package is added to Magento, files are moved into a handful of places as defined by the `package.xml file`. In order to keep our files here nice and neat, but still have everything work in Magento, we need to set up a handful of symlinks. In your terminal, cd into `dockerized-magento` and then follow these commands:

```bash
# replace '...' with the path to your dev directory
ln -s .../app-magento/app/code/local/ActiveCampaign web/app/code/community/ActiveCampaign

ln -s .../app-magento/app/design/adminhtml/default/default/layout/subscriptions.xml web/app/design/adminhtml/default/default/layout/subscriptions.xml

ln -s .../app-magento/app/design/adminhtml/default/default/template/subscriptions web/app/design/adminhtml/default/default/template/subscriptions

ln -s .../app-magento/app/design/frontend/default/default/layout/subscriptions.xml web/app/design/frontend/default/default/layout/subscriptions.xml

ln -s .../app-magento/app/design/frontend/default/default/template/subscriptions web/app/design/frontend/default/default/template/subscriptions

ln -s .../app-magento/app/etc/modules/ActiveCampaign_Subscriptions.xml web/app/etc/modules/ActiveCampaign_Subscriptions.xml

# This last one is tricky, you'll need to update the symlink every time you make a version change, ie, ActiveCampaign_Subscriptions-1.5.10.xml
ln -s .../app-magento/app/package.xml web/var/package/ActiveCampaign_Subscriptions-{CURRENT VERSION}.xml
```

### Code clean up
Magento has very strict coding standards. Before you create a new package with the changes you've made, you'll need to confirm that they abide by these standards. To do so, you'll use their code standards checking tool. Here's the process:

```bash
cd magento-coding-standard

composer install

# To see only the errors that are required to be fixed for Magento approval
composer run lint

# To autofix any errors or warnings that can be autofixed
composer run autoformat

# To see all warnings
composer run lint-all
```

## Sources
https://github.com/andreaskoch/dockerized-magento
https://github.com/magento/marketplace-eqp
