# RunnerBasic
Builds a MySQL server and an apache webserver. 

## Parameters
The root, path and dest variables are designed to make up the path, like if you're installing in /var/www/nextcloud then these should be var, www & nextcloud. Splitting them up in this way made some sections of the ansible play a bit easier.
* datacenter # ex: GB3
* nextcloud_root # ex: data_nc
* nextcloud_path # ex: www
* nextcloud_dest # ex: nextcloud
* nextcloud_version # ex: 10.0.0
* ht_user # ex: www-data
* ht_group # ex: www-data
* mysqlpass # ex: "abc123!@#"

### Description
This playbook is designed to be called by Runner and will provision an Ubuntu 14.04 server in CenturyLink Cloud and deploys a working instance of nextcloud to it.
The high level tasks are as follows:
* provision a group and server
* install and harden MySQL and create the nextcloud database
* install apache, postfix and some php packages
* make some directory structures, set some permissions & ownership and copy some config files to the server
* configure apache and postfix


### Prerequisites
You must have a CenturyLink Cloud account to be able to use Runner

### Additional Steps
Once the Server has been configured there are some steps that need to be completed.  When you log into the http address previously mentioned it will ask for an initial login and going through that login process generates the hashed passwords and such like in the config.php file below.

Once that is done, change the http in that config.php file to become https and it should redirect you.
You can also add the public IP here to the 'array' section and I've also added some sample email config that works in CenturyLink Cloud.

If you don't want new users to have the Documents and Photos folders in their accounts by default change, the config.php line below so that `'skeletondirectory'` points at `=> '/data_oc/www/nextcloud/core/skelempty',` 

### EXAMPLE config.php
```
<?php
$CONFIG = array (
  'instanceid' => 'xxxxxxxxxxxx',
  'passwordsalt' => 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxx',
  'secret' => 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx',
  'trusted_domains' =>
  array (
    0 => 'PRIVATEIPADDRESS',
    1 => 'PUBLICIPADDRESS',
  ),
  'datadirectory' => '/data_oc/www/nextcloud/data',
  'overwrite.cli.url' => 'https://IPADDRESS/nextcloud',
  'dbtype' => 'mysql',
  'version' => '10.0.0',
  'dbname' => 'nextcloud',
  'dbhost' => 'localhost',
  'dbtableprefix' => 'oc_',
  'dbuser' => 'oc_admin',
  'dbpassword' => 'BPBPUKXrJ51+AUvT4fUrMPW+FTvBPh',
  'logtimezone' => 'UTC',
  'installed' => true,
  'skeletondirectory' => '/data_oc/www/nextcloud/core/skeleton',
  'mail_smtpmode' => 'sendmail',
  'mail_from_address' => 'admin',
  'mail_smtphost' => 'relay@t3mx.com',
  'mail_smtpport' => '25',
  'mail_smtpauthtype' => 'PLAIN',
  'mail_smtpauth' => 1,
  'mail_domain' => 'clctest.com',
  'memcache.local' => '\\OC\\Memcache\\APCu',
  'memcache.locking' => '\\OC\\Memcache\\Redis',
  'redis' =>
  array (
    'host' => 'localhost',
    'port' => 6379,
  ),
  'maintenance' => false,
  'loglevel' => 2,
);
```
### Errors
```
Your data directory and your files are probably accessible from the Internet. The .htaccess file is not working. We strongly suggest that you configure your web server in a way that the data directory is no longer accessible or you move the data directory outside the web server document root.
```
An easy fix for this error is to move your data directory up 1 level and edit the line in your config.php, i.e. `  'datadirectory' => '/data_oc/www/nextcloud/data',` would become `  'datadirectory' => '/data_oc/www/data',`, the move command would be `# mv /data_oc/www/nextcloud/data /data_oc/www/`
