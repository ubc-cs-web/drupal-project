# Composer template for CS Drupal projects

[![CI](https://github.com/drupal-composer/drupal-project/actions/workflows/ci.yml/badge.svg?branch=9.x)](https://github.com/drupal-composer/drupal-project/actions/workflows/ci.yml)

This project template provides a starter kit for managing your site
dependencies with [Composer](https://getcomposer.org/).

## Usage

First you need to [install Composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx).

> Note: The instructions below refer to the [global Composer installation](https://getcomposer.org/doc/00-intro.md#globally).
You might need to replace `composer` with `php composer.phar` (or similar)
for your setup.

After that you can install the project:

```
composer install
```

## What does the template do?

When installing the given `composer.json` some tasks are taken care of:

* Drupal will be installed in the `public`-directory.
* Autoloader is implemented to use the generated composer autoloader in `vendor/autoload.php`,
  instead of the one provided by Drupal (`public/vendor/autoload.php`).
* Modules (packages of type `drupal-module`) will be placed in `public/modules/contrib/`
* Theme (packages of type `drupal-theme`) will be placed in `public/themes/contrib/`
* Profiles (packages of type `drupal-profile`) will be placed in `public/profiles/contrib/`
* Creates default writable versions of `settings.php` and `services.yml`.
* Creates `public/sites/default/files`-directory.
* Latest version of drush is installed locally for use at `vendor/bin/drush`.
* Latest version of DrupalConsole is installed locally for use at `vendor/bin/drupal`.
* Latest version of deployer is installed locally for use at `vendor/bin/dep`.
* Creates environment variables based on your .env file. See [.env.example](.env.example).

## Updating Drupal Core

This project will attempt to keep all of your Drupal Core files up-to-date; the
project [drupal/core-composer-scaffold](https://github.com/drupal/core-composer-scaffold)
is used to ensure that your scaffold files are updated every time drupal/core is
updated. If you customize any of the "scaffolding" files (commonly `.htaccess`),
you may need to merge conflicts if any of your modified files are updated in a
new release of Drupal core.

Follow the steps below to update your core files.

1. Run `composer update "drupal/core-*" --with-dependencies` to update Drupal Core and its dependencies.
2. Run `git diff` to determine if any of the scaffolding files have changed.
   Review the files for any changes and restore any customizations to
  `.htaccess` or `robots.txt`.
1. Commit everything all together in a single commit, so `public` will remain in
   sync with the `core` when checking out branches or running `git bisect`.
1. In the event that there are non-trivial conflicts in step 2, you may wish
   to perform these steps on a branch, and use `git merge` to combine the
   updated core files with your customized files. This facilitates the use
   of a [three-way merge tool such as kdiff3](http://www.gitshah.com/2010/12/how-to-setup-kdiff-as-diff-tool-for-git.html). This setup is not necessary if your changes are simple;
   keeping all of your modifications at the beginning or end of the file is a
   good strategy to keep merges easy.

## FAQ

### Should I commit the contrib modules I download?

Composer recommends **no**. They provide [argumentation against but also
workrounds if a project decides to do it anyway](https://getcomposer.org/doc/faqs/should-i-commit-the-dependencies-in-my-vendor-directory.md).

### How can I apply patches to downloaded modules?

If you need to apply patches (depending on the project being modified, a pull
request is often a better solution), you can do so with the
[composer-patches](https://github.com/cweagans/composer-patches) plugin.

To add a patch to drupal module foobar insert the patches section in the 
section of composer.patches.json:

```json
{
    "patches": {
       "drupal/foobar": {
            "Patch description": "URL or local path to patch"
        } 
    }
}
```

### How do I specify a PHP version ?

This project supports PHP 7.4.9 as minimum version (see [Environment requirements of Drupal 9](https://www.drupal.org/docs/understanding-drupal/how-drupal-9-was-made-and-what-is-included/environment-requirements-of)), however it's possible that a `composer update` will upgrade some package that will then require PHP 7.4+.

To prevent this you can add this code to specify the PHP version you want to use in the `config` section of `composer.json`:

```json
"config": {
    ...
    "platform": {
        "php": "7.4.9"
    }
},
```
