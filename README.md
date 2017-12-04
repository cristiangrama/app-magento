# app-magento
Magento is both an open-source and enterprise-purchased (ie, there's a free and a paid version) ecommerce store framework and CMS. This extension works much like a WordPress plugin - a Magento user installs it and it extends the functionality of Magento. This extension allows Magento users to bulk export all their existing newsletter subscribers to their ActiveCampaign account, as well as sync all new customers that opt-in to their newsletter after a purchase. That sync can also include a form id so that form actions can be performed on a Contact. Currently this extension supports Magento `1.7.0<= && <2.0.0` (most Magento 1 installs are on `1.9.3.7`).

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

### Updating Files
When a package is added to Magento, files are moved into a handful of places as defined by the `package.xml file`. To move the files to their correct locations in a more efficient manner, you can use the `update-files` bash script. From the root of this repo (ie, your path should be `path/to/app-magento`), run the command `./update-files ~/path/to/dockerized-magento`. The script will remove the old files and update Magento with your new files.

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

### Smoke Testing
We currently have no automated testing in place for this extension. Here are the steps for testing that the extension is working:

- Run `./update-files ~/path/to/dockerized-magento` to get fresh files installed
- Open a test account on AC
- Open up the Magento admin section by migrating in the browser to `dockerized-magento.local/admin` and signing in (username: `admin`, password: `password123`)
- Open the extension settings page and add the API credentials from your test AC account
- Ensure that the extension is enabled
- Ensure you have newsletter subscribers
- Run the Bulk Exporter ("Export Magento Subscribers")
- Allow your test account a few minutes to handle the bulk import
- Check that all subscribers have been imported to your AC account
- Place a new order on the local Magento site
- Check that the new customer was imported to your AC account
- Create a new form in AC with an action associated
- Refresh the extension in Magento until the form is displayed as an option in the Forms tab
- Select that Form and save the changes
- Place a new order with a _different_ email address than the first test order
- Check that the contact was imported and the Form actions were performed

If all the above tests pass, you're ready to package the extension!

### Packaging the Extension
If you've added or removed any files, update the file `ActiveCampaign_Subscriptions.xml` in the root of this repo. Additionally, update the versioning and the release notes in that file.

Run the command `./update-files ~/path/to/dockerized-magento` to bring in the fresh, updated files.
 
In the Magento admin section, migrate to `Systems -> Magento Connect -> Package Extensions`. Select the tab on the left-hand side "Load Local Package". You should see a package titled ActiveCampaign_Subscriptions. Click on that package. You should now have all the details from that xml file loaded into the packager. Confirm that everything looks correct, then click the button "Save Data and Create Package" in the top right corner. Once it has completed, you'll find a file in the `dockerized-magento/web/var/connect` directory titled `ActiveCampaign_Subscriptions-{CURRENT_VERSION}.tgz`. Send that tarball to Tim to upload, and the package will be released!

## Sources
https://github.com/andreaskoch/dockerized-magento
https://github.com/magento/marketplace-eqp
