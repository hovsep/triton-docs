============
Introduction
============

Triton is a server based application for sending transactional and newsletter emails.
Think of it as self hosted alternative to Mailchimp and Mandrill.

Features
========

- Templating
    Templating couldn't be any more pliable.
    Forget about limited merging tags.
    Triton uses Blade templating engine so you can use Blade or even raw PHP syntax!
    Variables, loops, conditions, inheritance, custom functions and whatever you want from the box.
    Of course Triton has built in CSS inliner and other tools for working with email-specific HTML.

- Data contracts
    With strict protocol you will always be sure your templates are always rendered in consistent context, with all needed variables.
    Also contracts will help you compose and test templates thanks to variable hinting.

- Transactional emails
    Use triggers to setup reactions on your app's events.
    Set custom HTTP-headers, define custom variables and switch triggers off when needed.
    Use dynamic subjects for more flexibility.
    Subjects also are Blade-templates, so you are limited only by your imagination!

- Newsletter emails
    We call it campaigns.
    Schedule them or run immediately.
    You can control sending process like an audio player (play\\pause\\stop) without worries that the given recipient will receive same email twice.

- Tracking & Stats
    Use tracking categories to manage statistic flows.
    You can accumulate stats from different triggers or campaigns into single tracking category.
    Sent & Opened metrics are captured by each recipient.

- Groups
    Everything (templates,triggers,campaigns etc) may be organized into groups for easy management.

- Recipient lists
    Import recipients from external CSV files or add them manually right from dashboard.
    You can create custom columns that will be available as variables within template.

- Multi transport
    Configure and use multiple SMTP transport.
    Each trigger or campaign may be configured to use custom transport.

Why Triton?
===========

- Self hosted
    There are lots of SaaS email applications such as Mandrill or SendinBlue.
    They have different features and pricing.
    The main disadvantage is that you have to host your inventory at third-party service.
    With self hosted solution you have full control of your emails ecosystem.
    `Learn more why 37 signals are using own mail servers <https://signalvnoise.com/posts/3096-giving-away-the-secrets-of-993-email-delivery>`_

- Go custom
    Sometimes basic functionality is not enough so you can add some custom features.
    Powered by Laravel Triton has well organized and clean code.
    Extend it yourself or request custom development from us.

- Microservice
    Your emails are hardcoded in monolith?
    Use Triton as your emails microservice.
    Split the responsibility and focus on your app, Triton will take care of emails.

- Fast
    Your users should not wait for email sending.
    Drop events to queue and go ahead enhancing user experience.
    Queue will be processed in background.

- Scalable
    Single instance of Triton may send millions of email daily.
    If it is not enough it is able to scale Triton horizontally thanks to queue-oriented architecture.

- Cheap
    Use your own SMTP servers or cheap alternatives such as Amazon SES and it will be still cheaper compared to SaaS solutions.
    No monthly charges or volume fees.


