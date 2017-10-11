============
Installation
============

This document assumes some basic familiarity with Unix, Git and Composer.
Triton has a few system requirements.
Please make sure your server meets them.

Server requirements
===================

- Nginx or Apache web server
- PHP >=5.6.4
- PHP extensions: OpenSSL, PDO, Mbstring, Tokenizer, XML, AMQP, bcmath
- Composer
- MySql
- RabbitMQ
- Redis

Installation steps
==================

(1) **Extract Triton files from archive and move them to your web server document root**

.. _sdk_installation:

(2) **Triton-SDK**

    Triton uses special protocol for transporting events from your application to queue.
    It is called Data Contract or SDK and consists of base event class and some utility classes.
    You will be able to add your custom event classes into it.

    SDK repository may be located at any place accessible from both main application and Triton servers (we suggest to use private github or bitbucket repo).
    Triton and your main application will use same version of SDK via composer.
    The idea is to keep both systems in sync, allowing them to interact using same data contract repository.

    Say your SDK copy is located at *github.com:username/tt-sdk-php.git*.
    You should add a "repository" section to both Triton's and main app's composer.json files:

    .. code-block:: javascript

        "repositories": [
            {
              "type": "vcs",
              "url": "git@github.com:username/tt-sdk-php.git"
            }
          ]


    After that you can add a dependency as usual:

    .. code-block:: javascript

        "triton/triton-sdk-php": "dev-master",

    As you see we have version "dev-master" here, it is helpful during development. Subsequently, you can supply specific versions.
    It is very important to keep it in sync on both (Triton and main app) systems. :ref:`Learn more about contracts development <sdk>`.


(3) **Install dependencies**

    Run `composer install` at Triton root directory.


(4) **Web server**

    Triton is a Laravel application, so you can refer to `Laravel docs <https://laravel.com/docs/5.4#web-server-configuration>`_.

    .. important:: Directories within the ``storage`` and the ``bootstrap/cache`` directories must be writable by your web server


(5) **Environment**

    Rename .env.example to .env and set appropriate values for each parameter.

    Run `php artisan key:generate` to set new app key, which will be used to encrypt user passwords.

    Create new MySQL database if you do not have existing one.

    .. glossary::

    *APP_URL*
        Your web host url.
    *DB*, *RABBITMQ* and *REDIS*  sections
        Set parameters as per your server.
    *MAIL* section
        Used to describe your default email transport

    .. hint:: Additional transports may be added at config/mail.php



(6) **Migrations and seeds**

    Run `php artisan migrate` to create a database schema.

    Run `php artisan db:seed` to fill db with roles,permissions and initial user account.
    By default it will create an admin account with username "admin@<your app url domain>" and password "admin".
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

