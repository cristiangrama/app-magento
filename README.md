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
