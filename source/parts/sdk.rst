---
SDK
---

For interacting with your main application Triton uses special protocol called Data Contracts or simply DC.
DC comes as a set of PHP-classes which allows you to send data consistently from main app to Triton.
The idea is based on `design by contract paradigm <https://en.wikipedia.org/wiki/Design_by_contract>`_.
SDK with DC implementation is included into Triton package and must be used as composer-dependency (:ref:`Learn more <sdk_installation>`).

The main idea of using DC instead of just sending arrays (e.g. with Mandrill API) is to bring more coherence between two systems.
With DC main app is not able to send incomplete data set, DC will raise an exception, so you will always send all variables required by the other end (Triton template).
From another hand Triton will be able to help user composing the templates.
It will show all variables available in given context.

Installation
============

This document assumes some basic familiarity with git and composer.

- Unpack files from archive
- Init it as git repo
- Add all files, commit & push them to origin master
- Install dependencies by running `composer install`
- Run tests `./vendor/bin/phpunit` or just `phpunit` if you have installed it globally.

Directory structure
===================

SDK is developed according to PSR-4:

.. code-block:: text

    -src
    --Triton
    ---AppContracts - Will contain your app contracts
    ---DataContractProtocol - Contains BaseEventContract that implements DC protocol
    ---Entities - Contains utility classes used by SDK and Triton
    ----Event
    ----Recipient
    ----Variable
    ---Exceptions - Contains exception classes
    -tests - Contains unit tests

As you guessed all your contracts should be placed into AppContracts directory.

Creating new contract
=====================

(1) Create php file at AppContracts directory.
    File name will be your event name, so it should be descriptive and unique.
    File name must be upper camel case.

    .. glossary::

    Good examples:
        ``UserPasswordReset``, ``AdvertiserNewCampaign``, ``AdminNewUser``.
    Bad examples:
        ``Event11``, ``VeryLongNameOfMyCoolEvent``.
(2) Define a class within the file.

    Example:

    .. code-block:: php

        <?php

        namespace Triton\AppContracts;

        use Triton\DataContractProtocol\BaseEventContract;

        class AdvertiserNewCampaign extends BaseEventContract {

        }

(3) Define a constructor with event description and required variables

    Example:

    .. code-block:: php

        public function __construct()
        {
            $this->setDescription('Advertiser created new campaign')
                 ->defineVar('username', 'John Doe', 'User nick name')
                 ->defineVar('balance', 100.00, 'User balance (float)');
        }

    That's it! You just created your first Data Contract! Congratulations!

Going deeper
============

- Each contract must extend the ``BaseEventContract`` class.
- Contract must have a description that will be shown to Triton's user.
- Class name must be equal to file name.
- Class name must be unique and will be used as event name at Triton side.
- Contract may or may not contain variables.
- Each variable must be defined with 3 values: name, test value and description.
- Variable name must be a valid PHP variable name without dollar sign, unique within contract scope.

Actual variables will be accessible from templates using native PHP notation.
For example "username" variable defined in contract above will be accessible in template as ordinary PHP variable $username.

Test value is used when testing trigger or campaign (:ref:`Learn more about emails testing<testing>`).

Sometimes you may want to make contract more strict. You should call ``mustBeStrict()`` method right in your contract constructor.
When strict mode is enabled contract will validate not only variables, but they types as well.

Say if you defined a var ``defineVar('balance', 100.00, 'User balance (float)')`` in flexible (default) mode you will be able to pass data of any type you want, but in strict mode you will be limited by type of the test value (float).
So it is up to you when use the strict mode.
Contract are flexible (not strict) by default.


.. hint::
    Variable description just make it easier for Triton side user to understand its purpose.
    Well written description may save a lot of time, avoiding situations when developer explains variable purpose to email marketer.

Versioning
==========

While queue is a bridge between your main application and Triton, contracts repository is a phrasebook.
It helps two apps to talk same language and play by same rules.
On the other hand contracts repository may change from time to time.
You may change\\remove your existing contracts or add new ones.
How Triton will support new contracts?
Here where versioning is a clue.

DC works on top of composer and git, so versioning is already in the veins.
We suggest some kind of `Semantic versioning <http://semver.org/>`_ for contracts.

Here are few sample rules:

- Version consist of 3 parts: protocol version.major.minor
- Current protocol version is 1. It may be increased only after braking changes in DC-paradigm.
- Every time you add/remove/edit variable or add/remove contract you should increase major version.
- Every time you make non breaking changes such as code comments, formatting etc you should increase minor version.
- Use git tags for marking a version

First of all add "repositories" section in your sdk composer.json file:

  .. code-block:: javascript

     "repositories": [{
         "type": "package",
         "package": {
            "name": "triton/triton-sdk-php",
            "version": "0.1.0",
            "source": {
                "url": "git@github.com:username/triton-sdk-php.git",
                "type": "git",
                "reference": "v0.1.0"
            }
         }
     }]

  For issuing new version follow steps:
    - Modify composer.json, set new version and tag name to package.version and package.source.reference sections
    - Add & commit files: `git commit -m "My comments on new version"`
    - Create a tag: `git tag v0.2.0`
    - Push tags: `git push --tags`
    - Push master: `git push origin master`

- Use wildcard version in your both composer.json files: "triton/triton-sdk-php": "1.*".
- Run `composer update triton/triton-sdk-php` to get latest version.
- Run `php artisan cache:clear --tags=dc` to clear cached DataContract list.



Filling contract
================

So you have created some contracts for your application and ready to send some events to Triton.
How will you do it?

- Make sure you installed SDK to your app. If no just require it via composer
- Go to place in your app code where you want to send an event.
- Create an instance of appropriate contract, example:

  .. code-block:: php

     $contract = new Triton\AppContracts\AdvertiserNewCampaign();

- Set recipient context (Learn more), example:

  .. code-block:: php

     $contract->setRecipient([
        RecipientField::EMAIL => 'test@test.com'
     ]);

  Here you must fill all required fields (``email`` by default) and may fill optional fields (``user_id`` by default).

  .. note:: You can easily extend ``recipient`` object, adding some custom fields, say ``login_token`` or ``cid`` (:ref:`See below <extending_recipient_fields>`).

- Set actual values for variables, example:

  .. code-block:: php

     $contract->setVar('username', 'Robert Paulson')
              ->setVar('balance', 100.78);

  As you guessed method chaining is allowed, so you may write something like this:

  .. code-block:: php

     $contract = new Triton\AppContracts\AdvertiserNewCampaign();
     $contract->setRecipient([RecipientField::EMAIL => 'test@test.com'])
              ->setVar('username', 'Robert Paulson')
              ->setVar('balance', 100.78);
- Push it to queue
  It depends on your app, you may use any third-party RabbitMQ library, the main thing is to call ``toArray()`` method of contract.
  Example:

  .. code-block:: php

     Queue::push($contract->toArray());

  That's it. Your event will be validated,packed and sent to queue or exception will be raised instead.

.. _extending_recipient_fields:

Extending recipient fields
==========================

Basically for sending an email to recipient we need to know just their email address.
That's why in DC protocol there is only one required recipient field.
For more complex purposes you may want to have more info about recipient.
Say you may want to track stats by your users.
Usually your users will have some kind of numeric ID.
You can send it in recipient object, example:

    .. code-block:: php

       $contract->setRecipient([
            RecipientField::EMAIL => 'test@test.com',
            RecipientField::USER_ID => 22
       ]);

So Triton will be able to track sendings and openings for each user.
You can add custom recipient-fields, make them required or optional depending on your needs.
Please look to ``src/Triton/Entities/Recipient/RecipientField.php`` file.
Adding new constants and putting (or not) them to all/allRequired methods results will do the trick.
