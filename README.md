# app-magento

## Local development
Follow the instructions provided by [this repo](https://github.com/andreaskoch/dockerized-magento) to set up a local docker instance running Magento.

### Code clean up
Magento has very strict coding standards. Before you create a new package with the changes you've made, you'll need to confirm that they abide by these standards. To do so, you'll use their code standards checking tool. Here's the process:

```bash
cd magento-coding-standard

composer install

# For all errors and warnings
vendor/bin/phpcs ../app --standard=MEQP1

# To autofix any errors or warnings that can be autofixed
vendor/bin/phpcbf ../app --standard=MEQP1

# To see only the errors that are required to be fixed for Magento approval
vendor/bin/phpcs ../app --standard=MEQP1 --severity=10
```

## Sources
https://github.com/andreaskoch/dockerized-magento
https://github.com/magento/marketplace-eqp
