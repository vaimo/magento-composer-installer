# Magento Composer Installer

This is a fork of [Magento Composer Installer](https://github.com/magento-hackathon/magento-composer-installer) that provides support for Magento 2 components.

## Usage

In `composer.json` of the component specify:
- `type` - type of Magento 2 component
- `extra/map` - list of files to move and their paths relative to the application root directory

Note: You need an `extra/map` section only if your component needs to be moved to other place than default vendor.
If your component doesn't require to copy files, you can omit this section.

## Supported Components

### Magento Module

Type: `magento2-module`

Installation location: composer vendor dir or defined in extra->map

Example:
```json
{
    "name": "magento/module-core",
    "description": "N/A",
    "require": {
        ...
    },
    "type": "magento2-module",
    "extra": {
        "map": [
            [
                "*",
                "Magento/Core"
            ]
        ]
    }
}
```

Final location will be `<magento_root>/app/code/Magento/Core`

### Magento Theme

Type: `magento2-theme`

Installation location: `app/design`

Example:
```json
{
    "name": "magento/theme-frontend-plushe",
    "description": "N/A",
    "require": {
        ...
    },
    "type": "magento2-theme",
    "extra": {
        "map": [
            [
                "*",
                "frontend/Magento/plushe"
            ]
        ]
    }
}
```

Final location will be `<magento_root>/app/design/frontend/Magento/plushe`

### Magento Language Package

Type: `magento2-language`

Installation location: `app/i18n`

Example:
```json
{
    "name": "magento/language-de_de",
    "description": "German (Germany) language",
    "require": {
        ...
    },
    "type": "magento2-language",
    "extra": {
        "map": [
            [
                "*",
                "Magento/de_DE"
            ]
        ]
    }
}
```

Final location will be `<magento_root>/app/i18n/Magento/de_DE`

### Magento Library

Support for libraries located in `lib/internal` instead of `vendor` directory.

Installation location: `lib/internal`

Type: `magento2-library`

Example:
```json
{
    "name": "magento/framework",
    "description": "N/A",
    "require": {
       ...
    },
    "type": "magento2-library",
    "extra": {
        "map": [
            [
                "*",
                "Magento/Framework"
            ]
        ]
    }
}
```

Final location will be `<magento_root>/lib/internal/Magento/Framework`

### Magento Component

Default type, if none is specified.

Installation location: `.` (root directory of the code base)

Type: `magento2-component`

Example:
```json
{
    "name": "magento/migration-tool",
    "description": "N/A",
    "require": {
        ...
    },
    "type": "magento2-component",
    "extra": {
        "map": [
            [
                "*",
                "dev/tools/Magento/Tools/Migration"
            ]
        ]
    }
}
```

Final location will be `<magento_root>/tools/Magento/Migration`


## Autoload

After handling all magento components, file `<magento_root>app/etc/vendor_path.php` with path to `vendor` directory is created inside application directory.

This information allows the application to utilize Composer autoloader in case any libraries are installed in `vendor` directory. The path to `vendor` varies between particular installations and depends on `magento-root-dir` setting for the Magento Composer Installer. That's why it should be generated for each installation.

You must run `composer install` to install dependencies for a new application or `composer update` to update dependencies for an existing application.

## Deployment Strategy

The default deployment strategy used by Magneto Composer Installer is `copy`. It will copy each files/directories from `vendor` directory to its designated location based on `extra/map` information stored in each component `composer.json` file.

There are [other deployment strategy](https://github.com/magento/magento-composer-installer/blob/master/doc/Deploy.md) that could be used, however Magento 2.x system does not guarantee its successful operation.
