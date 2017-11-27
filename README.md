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
