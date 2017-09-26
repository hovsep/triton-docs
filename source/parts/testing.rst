-------------
Testing mails
-------------

Of course you should not send your emails before you are not sure it is 100% ok.
No doubt that emails are very important, one broken link may decrease users loyalty.
That's why you should always test your emails before sending.
Triton comes up with basic built in testing tool.

It allows you to render or send a test email with ability to redefine all variables.
Triton will show an error in case the template you are using is broken, so you will be able to fix them up before you go.

You can test both triggers and campaigns.

Why no ability to test template?
================================

In terms of Triton template is just a named piece of Blade markup.
Template can be rendered only in trigger or campaign :ref:`context <context>`, so it makes no sense to test it separately.

Autotesting
===========

Additionally to manual testing Triton has an automated testing service. It is available only for triggers.
The main idea is to test all triggers every day, to be sure nothing is left untested.
Sometime you may modify templates or triggers and forget to test the updated version.
Here there autotest will help to alarm a problem.

Automatic test runs daily at the midnight. Only active triggers are tested.
If troubled triggers are found email notification will be sent to every Admin.

Also you may manually initiate a bulk trigger test from console.
Just run

.. code-block:: bash

   php artisan triggers:test


`-no-email` option may be used to run test without email notification.

Autotest reports (date and status) may be found on trigger list.