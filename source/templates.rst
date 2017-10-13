----------
Templating
----------

Personalizing emails is a positive way to engage your subscribers and increase responses.
For example, it's possible to insert the full name of your recipients in their email, or add a customized greeting in the subject or content of your email.

Triton supports templating like no others.

First of all you should think of templates in terms of Triton as named piece of markup. It doesn't even have to be a complete letter.
Triton uses Blade as its templating engine. It means that you are not limited at all, each your template is a PHP-script.
As you know with great power great responsibility comes, so you should write your templates very carefully.
Blade allows you to use conditions, loops and many many other cool stuff.
Also Blade supports including sub-views, that's why we mentioned that your template doesn't even have to be a complete letter.
You can learn more about Blade on `official Laravel site <https://laravel.com/docs/5.5/blade>`_.

Editor
======

For convenience template editor has HTML-syntax highlighting.
There is no WYSIWYG editor or visual constructor, because we believe it is not Triton's job.

Also you will find additional tabs like "Live preview" and "Used PHP variables".
Preview is used to take a look briefly on how markup is working.

.. caution:: Do not confuse "Live preview" with :ref:`testing <testing>`.


Preview is not rendered template it just helps to see if markup is working well or image has valid urls.

On the last tab you may find the list of PHP-variables used in template at the moment.

On the right side of editor you may find very helpful control.
It shows available variables for different :ref:`contexts <context>`.
Single click by variable will show a popup with useful description of that variable.
Doubleclick on given variable will add it into editor at cursor's position.

Variables
=========

The most powerful templating approach is personalization using variables.
With Triton your emails will rise to another level of personalization, because it is possible to use not only scalars (strings, numbers, booleans) but also arrays!
For example, you can pass to Triton some structured data about your e-shop order and iterate it right in your template:

    .. code-block:: guess

       @foreach($order as $orderItem)
       {{ $orderItem['sku'] }}, {{ $orderItem['price'] }}
       @endforeach

It is transparent and reliable way.

.. _context:

Context
=======

When we talk about variables it is good to touch a term of "context".
In simple words "context" is a set of variables available within the template when it is getting rendered.

So there are two type of context: trigger and campaign.

Following variables are available in trigger's context:

- ``$dc_name`` - Reserved variable that contains Data Contract (event) name
- ``$tracking_category_id`` - Reserved variable with trigger's tracking category id or zero if not defined
- ``$tracking_category_name`` - Reserved variable with trigger's tracking category name or empty string if not defined
- ``$email`` - Reserved variable with recipient email (set in recipient context when queuing a dc at main app)
- ``$user_id`` - Reserved variable with recipient user id (set in recipient context when queuing a dc at main app)
- All optional reipient context data (set when queuing a dc at main app)
- All Data Contract variables
- All trigger's custom variables

Following variables are available in campaign's context:

- ``$tracking_category_id`` - Reserved variable with campaign's tracking category id or zero if not defined
- ``$tracking_category_name`` - Reserved variable with campaign's tracking category name or empty string if not defined
- ``$email`` - Recipient email (from list)
- ``$user_id`` - Recipient user id (from list)
- All List's columns (lowercased)
- All campaign's custom variables

.. _helpers:

Helper functions
================

The process of creating a template is mostly consists of working with a text.
From time to time you may find yourself writing the same  snippet again and again.
Or you may use some text patterns or formatting.
Anyway you can help yourself with helper functions.

Helper function are ordinary PHP functions defined in ``app/Helpers/BladeHelpers.php`` file.
All functions defined there are available for calling from template code.
If you have a look on that file you will find some existing helpers, e.g. ``eventCollectorUrl()`` may help you to generate :ref:`open-tracking pixel <tracking_openings>`.
Feel free to add your own custom functions and use them in your templates, do not repeat yourself and save time.