# Magento2_SomethingDigital_UpgradeHelper

## Installation 

As this is a local dev tool only, you can quickly drop it into app/code.

E.g.

```
$ tree app/code/SomethingDigital/UpgradeHelper
app/code/SomethingDigital/UpgradeHelper
├── Console
│   └── UpgradeHelperCommand.php
├── README.md
├── etc
│   ├── di.xml
│   └── module.xml
└── registration.php

2 directories, 5 files
```

## Usage

### Generating the diff

> Are you a Something Digital employee? If so, the diff is likely already available for you here: https://github.com/sdinteractive/m2-diffs

- Download a ZIP of the old version
- Download a ZIP of the new version
- Use `diff -r` to generate the diff

Example:

```
diff -r magento-2-2-6-ee magento-2-2-7-ee > magento-2-2-6-ee--2-2-7-ee.diff
```

### How To Run

```
$ bin/magento sd:dev:upgrade-helper magento-2-2-6-ee--2-2-7-ee.diff
```

> **NOTE:** Running on Vagrant without `nfs_guest` is intolerably slow. Either ensure you are running `nfs_guest` or else temporarily move the Magento installation out of `/vagrant` to run

#### Example Output

```
$ bin/magento sd:dev:upgrade-helper magento-2-2-6-ee--2-2-7-ee.diff
-------- PREFERENCES --------
Patched: vendor/magento/module-catalog/Block/Product/ImageBuilder.php
Customized: Company\Module\Block\Product\ImageBuilder
-------- TEMPLATES --------
Patched: vendor/amzn/amazon-pay-and-login-with-amazon-core-module/view/adminhtml/templates/system/config/simplepath_admin.phtml
Customized: vendor/amzn/amazon-pay-and-login-magento-2-module/src/Core/view/adminhtml/templates/system/config/simplepath_admin.phtml
```

In this case `vendor/magento/module-catalog/Block/Product/ImageBuilder.php` will be changed by the patch, however there is a preference (`Company\Module\Block\Product\ImageBuilder`) for that file.

The tool has also flagged the vendor/amzn template, however upon inspection it is clear that this is a false positive (the Amazon pay module simply moved templates out of the src folder).
