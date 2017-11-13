============
Installation
============

This document assumes some basic familiarity with Unix, Git and Composer.
Triton has a number of system requirements. Please make sure your server meets them.

Server requirements
===================

- Nginx or Apache web server
- PHP >=5.6.4
- PHP extensions: OpenSSL, PDO, Mbstring, Tokenizer, XML, bcmath
- Composer
- MySQL
- RabbitMQ
- Redis

The product has been tested on several modern Linux distributions. It should work on Windows but has not been fully tested there. RabbitMQ and Redis services may run on a separate server and can be accessed remotely via host/port.

Installation steps
==================

(1) **Extract Triton files from archive and move them to your web host document root**

    It is strongly advised to setup a virtual host for accessing Triton via dedicated domain or subdomain.

.. _sdk_installation:

(2) **Triton-SDK**

    Triton uses special protocol for transporting events from your application to queue.
    It is called Data Contract and consists of base event class and some utility classes.
    Within Triton SDK, you will be able to add your custom event classes into it.
    
    Triton and your main application will use same version of SDK via composer.
    The idea is to keep both systems in sync, allowing them to interact using same data contract repository.

    SDK repository may be located at any place accessible from both main application and Triton servers. The recommended approach assumes using private github or bitbucket repo.
    

    Say your SDK copy is located at *github.com:username/tt-sdk-php.git*.
    You should add a "repository" section to both Triton's and main app's composer.json files:

    .. code-block:: javascript

        "repositories": [
            {
              "type": "vcs",
              "url": "git@github.com:username/tt-sdk-php.git"
            }
          ]


    After that you can add a dependency the standard way:

    .. code-block:: javascript

        "triton/triton-sdk-php": "dev-master",

    As you can see, we have "dev-master" version here, it helps during development. You can supply specific versions later.
    It is very important to keep it in sync on both (Triton and main app) systems. :ref:`Learn more about contracts development <sdk>`.

    In the simplest case, whem Triton and your main app run on the same server, you can simply place Triton SDK into a directory and inform Composer where to look for it. For example:

    .. code-block:: javascript

        "repositories": [
            {
              "type": "path",
              "url": "./triton-sdk-php"
            }
          ]

    The above notation means Triton SDK is unpacked into a direct subdirectory of Triton installation.

(3) **Install dependencies**

    Run `composer install` at Triton root directory.

(4) **Web server**

    For the product to work correctly, you need to enable URL rewriting (in case of Apache webserver, make sure `mod_rewrite` module is enabled.)

    Since Triton is a Laravel application, you can refer to `Laravel docs <https://laravel.com/docs/5.4#web-server-configuration>`_ for details on configuring web server.

    .. important:: Directories within the ``storage`` and the ``bootstrap/cache`` directories must be writable by your web server

(5) **Environment**

    Rename `.env.example` file to `.env` and set appropriate values for each parameter.

    Run `php artisan key:generate` to set new app key, which will be used to encrypt user passwords.

    Create new MySQL database if you do not have one yet.

    .. glossary::

    *APP_URL*
        Your web host URL.
    *DB*, *RABBITMQ* and *REDIS*  sections
        Set parameters as per your server.
    *MAIL* section
        Used to describe your default email transport

    .. hint:: Additional transports may be added at config/mail.php

(6) **Migrations and seeds**

    In here, migration means setting up a database and filling it with initial structure and data (referred to as seeds).

    Run `php artisan migrate` to create a database schema.

    Run `php artisan db:seed` to fill db with roles, permissions and initial user account.
    By default, admin account is created with username "admin@<your app url domain>" and password "admin".
    After seeding you will be able to log in into the system and create new accounts.

    .. caution:: Change default password to secure one.

(7) **Cron**

    You only need to add the following Cron entry to your server. `Learn more <https://laravel.com/docs/5.4/scheduling>`_

    `* * * * * php /path-to-triton/artisan schedule:run >> /dev/null 2>&1`

(8) **Running queue workers**

    Run:

    .. code-block:: bash

        php artisan queue:listen --queue=production_stats
        php artisan queue:listen --queue=production_events_failed
        php artisan queue:listen --queue=production_transactional #This queue name must be also used by your main app
        php artisan queue:listen --queue=production_campaigns
        php artisan queue:listen --queue=production_triton #This queue name is configured in your .env file

    .. note:: You may want to use a process monitor such as `Supervisor <https://laravel.com/docs/5.4/queues#supervisor-configuration>`_ to ensure that the queue worker does not stop running.
