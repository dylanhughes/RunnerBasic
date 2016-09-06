# ownCloud
Create and ownCloud server in CLC cloud

## Parameters
* datacenter # ex: GB3
* owncloud_root # ex: /data_oc/www
* htuser # ex: www-data
* htgroup # ex: www-data
* mysqlpass # ex: "abc123!@#"

### DESCRIPTION:
This playbook is designed to be called by Runner and will provision an Ubuntu 14.04 server in CenturyLink Cloud and deploys a working instance of ownCloud to it.
The high level tasks are as follows:
* provision a group and server
* install and harden MySQL and create the owncloud database
* install apache, postfix and some php packages
* make some directory structures, set some permissions & ownership and copy some config files to the server
* configure apache and postfix

### NOTES:
Because ownCloud is a sort of private dropbox it's probable that a disk (partition) will need to be added to the instance to serve as storage, to do this I would move /data_oc to a temporary location, add a 'partition' mounted to /data_oc then move the contents of the original /data_oc into your new partition.
Once the play is successful, hit the private IP at http://aa.bb.cc.dd/owncloud

### PREREQUISITES
You must have a CenturyLink Cloud account to be able to use Runner

### ADDITIONAL STEPS
Once the Server has been configured there are some steps that need to be completed.  When you log into the http address previously mentioned it will ask for an initial login and going through that login process generates the hashed passwords and such like in the config.php file below.

Once that is done, change the http in that config.php file to become https and it should redirect you.
You can also add the public IP here to the 'array' section and I've also added some sample email config that works in CenturyLink Cloud.

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
  'datadirectory' => '/data_oc/www/owncloud/data',
  'overwrite.cli.url' => 'https://IPADDRESS/owncloud',
  'dbtype' => 'mysql',
  'version' => '9.0.2.2',
  'dbname' => 'owncloud',
  'dbhost' => 'localhost',
  'dbtableprefix' => 'oc_',
  'dbuser' => 'oc_admin',
  'dbpassword' => 'BPBPUKXrJ51+AUvT4fUrMPW+FTvBPh',
  'logtimezone' => 'UTC',
  'installed' => true,
  'mail_smtpmode' => 'sendmail',
  'mail_from_address' => 'admin',
  'mail_smtphost' => 'relay@t3mx.com',
  'mail_smtpport' => '25',
  'mail_smtpauthtype' => 'PLAIN',
  'mail_smtpauth' => 1,
  'mail_domain' => 'clctest.com',
  'memcache.local' => '\OC\Memcache\APCu',
);
```
