# app-magento

## Local development
The directory `magento` in this repo has everything you need to boot a local instance of Magento with docker. Before starting, you'll need to stop anything that's using port 80, meaning you'll need to stop localdev if it's running.

```bash
sudo vim /etc/hosts

# In this file, add the following:

127.0.0.1 magento.app

# Exit vim by hitting the escape button and then typing :wq and hitting enter

cd magento

docker-compose up

# The first time you run this command it will take quite awhile. The makers of this compose file are wonderful and included dummy data for the Magento app so you have stuff to interact with. However, installing it takes forever. You'll know that it's done because you'll see printed out:

installer_1      | Installation fininished
installer_1      |
installer_1      | phpMyAdmin: http://magento.app:8080
installer_1      |  - Username: root
installer_1      |  - Password: pw
installer_1      |
installer_1      | Backend: http://magento.app/admin
installer_1      |  - Username: admin
installer_1      |  - Password: password123
installer_1      |
installer_1      | Frontend: http://magento.app/
```

Navigate to `magento.app` and you'll see the site!

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

