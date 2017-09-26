-----
Stats
-----

Triton has basic stats handling system out of the box.
It tracks two main metrics: sendings and openings.

Stats are captured by three keys at same time: day, user_id, tracking category.
Additionally last sending and last opening dates are captured by each user_id and each tracking category.

.. _tracking_openings:

Tracking openings
=================

While tracking sendings is not quite difficult, determining when user opened an email may be tricky.
The most commonly used technique is using an open-pixel - special code snippet, injected into mail body.
Usually it is 1x1 pixel transparent image that forces email client to send an HTTP request back to sender when message is shown.

Triton supports open tracking pixels, but its use is up to you.

If you want opening stats get captured you have to manually put pixel-code into your template.
Basically it looks like that:

.. code-block:: guess

   <img alt="" src="{{ eventCollectorUrl() }}" style="border: 0; height: auto !important; outline: none; text-decoration: none; -ms-interpolation-mode: bicubic;">

As you can see you actually don't need a real single pixel image file. You have to generate special URL, using ``eventCollectorUrl()`` - helper function (:ref:`learn more about helpers <helpers>`).
When called without arguments it will return a string containing an URL to Triton's collector API.

It looks like

.. code-block:: text

   http://triton.yourdomain.com/api/collect?uid=1&cat=2&act=open

where 1 is user id, 2 is tracking category id.
Triton will inject respective values depending on context.

Say we are sending a triggered email to user ``777``. Our imaginary trigger is attached to tracking category ``888``.

When processing our imaginary event:

.. code-block:: guess

   {{ eventCollectorUrl() }}

will be rendered to

.. code-block:: guess

   http://triton.yourdomain.com/api/collect?uid=777&cat=888&act=open

Furthermore, if you use same template with another trigger Triton will inject appropriate tracking category respectively.

It is all about the :ref:`context <context>`.

Also you may want to override default tracking category ``id`` and even ``action`` parameter.
Just look to helper's signature:

.. code-block:: php

   function eventCollectorUrl($trackingCategoryId = null, $action = EventAction::OPEN)