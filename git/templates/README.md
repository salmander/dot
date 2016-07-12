*** HARA Repo `Post-Merge` hook ***

When you do a `git pull` in one of the HARA repositories (`hara-auth-service`, `hara-productsafety-service` etc.), Git hook will automatically run the script to do a `composer install` (to install any packages required) and database migrate (`phinx migrate`).

Installation Procedure:

1. Copy `post-merge` script into your home directory (i.e. `~/.git-hooks/post-merge`)
2. Make `post-merge` script executable by `sudo chmod +x ~/.git-hooks/post-merge`.
3. Change Git's global hooks path to the new directory by running the following command:
    `git config --global core.hooksPath ~/.git-hooks`
4. cd to all `hara-` directories (i.e. `hara-auth-service`, `hara-productsafety-service` etc.) and do `git init`.
5. Tada... If everything goes, when you do `git pull` the `post-merge` script will install any composer dependencies and also run any DB migrations. An example output is shown below:
```
salman.ahmed@macXXX ~/w/h/hara-auth-service (master)> git pull
remote: Counting objects: 216, done.
remote: Compressing objects: 100% (139/139), done.
remote: Total 216 (delta 87), reused 13 (delta 13), pack-reused 14
Receiving objects: 100% (216/216), 46.34 KiB | 11.00 KiB/s, done.
Resolving deltas: 100% (87/87), completed with 13 local objects.
From github.com:JSainsburyPLC/hara-auth-service
   acf9a6b..76f6f21  master     -> upstream/master
Updating a5a2589..76f6f21
Fast-forward
 composer.json                                                            |   3 +-
 composer.lock                                                            | 175 ++++++++++++++++++++++++++++++-
 config/db/migrations/20160705102202_add_status_field_to_user.php         |  36 +++++++
 .../migrations/20160706092130_add_confirmation_token_field_to_user.php   |  35 +++++++
 config/db/migrations/20160706092655_allow_null_password_for_user.php     |  36 +++++++
 .../migrations/20160706105146_remove_index_in_status_field_for_user.php  |  37 +++++++
 ...6105927_add_unique_constraint_to_confirmation_token_field_in_user.php |  36 +++++++
 config/di/prod.php                                                       |  35 ++++++-
 config/di/testing-overrides.php                                          |   7 ++
 config/routing.php                                                       |  22 ++++
 .../Hara/AuthService/Controller/InputValidator/InputValidator.php        | 107 +++++++++++++++++++
 src/Sainsburys/Hara/AuthService/Controller/LookupController.php          | 192 +++++++++++++++++++++++++++++++++-
 src/Sainsburys/Hara/AuthService/Model/User.php                           |  53 ++++++++--
 src/Sainsburys/Hara/AuthService/Service/EmailService.php                 |  70 +++++++++++++
 src/Sainsburys/Hara/AuthService/Service/PasswordService.php              |   7 +-
 src/Sainsburys/Hara/AuthService/Service/UserService.php                  | 143 ++++++++++++++++++++++---
 .../Hara/AuthService/Test/IntegrationTesting/AuthenticationContext.php   |  80 +++++++++++++-
 tests/behat/feature_files/02-hara-94-auth-service.feature                |   1 +
 tests/behat/feature_files/03-hara-32-service-proxy-compatibility.feature |   6 +-
 tests/behat/feature_files/04-get-psm-list.feature                        |  16 +--
 tests/behat/feature_files/05-get-pt-list.feature                         |  16 +--
 tests/behat/feature_files/06-create-new-user.feature                     |  73 +++++++++++++
 tests/behat/feature_files/07-get-supplier-list.feature                   | 120 +++++++++++++++++++++
 tests/behat/feature_files/08-update-user.feature                         | 101 ++++++++++++++++++
 tests/behat/feature_files/09-activate-account.feature                    |  98 +++++++++++++++++
 .../Sainsburys/Hara/AuthService/Controller/LookupControllerSpec.php      |  77 +++++++++++++-
 .../SainsburysHaraSpec/Sainsburys/Hara/AuthService/Model/UserSpec.php    |  27 ++++-
 .../Sainsburys/Hara/AuthService/Service/UserServiceSpec.php              |  75 +++++++++++--
 28 files changed, 1619 insertions(+), 65 deletions(-)
 create mode 100644 config/db/migrations/20160705102202_add_status_field_to_user.php
 create mode 100644 config/db/migrations/20160706092130_add_confirmation_token_field_to_user.php
 create mode 100644 config/db/migrations/20160706092655_allow_null_password_for_user.php
 create mode 100644 config/db/migrations/20160706105146_remove_index_in_status_field_for_user.php
 create mode 100644 config/db/migrations/20160706105927_add_unique_constraint_to_confirmation_token_field_in_user.php
 create mode 100644 src/Sainsburys/Hara/AuthService/Controller/InputValidator/InputValidator.php
 create mode 100644 src/Sainsburys/Hara/AuthService/Service/EmailService.php
 create mode 100644 tests/behat/feature_files/06-create-new-user.feature
 create mode 100644 tests/behat/feature_files/07-get-supplier-list.feature
 create mode 100644 tests/behat/feature_files/08-update-user.feature
 create mode 100644 tests/behat/feature_files/09-activate-account.feature
>>>> PWD: '/Users/salman.ahmed/www/hara/hara-auth-service/../hara-ops/' <<<<
>>>> HARA Ops dir: '/Users/salman.ahmed/www/hara/hara-auth-service/../hara-ops/' <<<<
>>>> HARA Service repo dir: 'hara-auth-service' <<<<
~~~ Script for 'composer install' and 'database migrations' ~~~
Doing composer install...
Loading composer repositories with package information
Installing dependencies (including require-dev) from lock file
  - Installing guzzlehttp/psr7 (1.3.1)
    Loading from cache

  - Installing guzzlehttp/promises (1.2.0)
    Loading from cache

  - Installing guzzlehttp/guzzle (6.2.0)
    Loading from cache

Generating autoload files

Doing DB migrations...
Phinx by Rob Morgan - https://phinx.org. version 0.5.5

using config file ./phinx.php
using config parser php
using migration path /hara/hara-auth-service/config/db/migrations
using seed path /hara/hara-auth-service/config/db/seeds
warning no environment specified, defaulting to: dev
using adapter pgsql
using database hara

 == 20160705102202 AddStatusFieldToUser: migrating
 == 20160705102202 AddStatusFieldToUser: migrated 0.0727s

 == 20160706092130 AddConfirmationTokenFieldToUser: migrating
 == 20160706092130 AddConfirmationTokenFieldToUser: migrated 0.0040s

 == 20160706092655 AllowNullPasswordForUser: migrating
 == 20160706092655 AllowNullPasswordForUser: migrated 0.0039s

 == 20160706105146 RemoveIndexInStatusFieldForUser: migrating
 == 20160706105146 RemoveIndexInStatusFieldForUser: migrated 0.0067s

 == 20160706105927 AddUniqueConstraintToConfirmationTokenFieldInUser: migrating
 == 20160706105927 AddUniqueConstraintToConfirmationTokenFieldInUser: migrated 0.0084s

All Done. Took 0.1883s
Connection to 127.0.0.1 closed.

==========~~~It's all done---Finito~~~============
==========~~~~~~~~Happy coding~~~~~~~~============
==========~~~~Script by salmander~~~~~============
salman.ahmed@macXXX ~/w/h/hara-auth-service (master)>
```


