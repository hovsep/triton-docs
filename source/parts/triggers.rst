--------
Triggers
--------

Trigger is the main building block of your transactional ecosystem.
It is an event handler.
Each trigger can handle single type of event (data contract).
When Triton pops an event from queue it looks to it name and tries to find appropriate trigger.
If trigger is found and it is active it is getting pulled and the letter flies away.
Trigger may send only one email per event.

Let’s look over trigger’s nutshell:

- Event
    Which event aka data contract will a given trigger handle.
- Template
    The template that will be used for email.

.. include:: parts/campaign_trigger_common_fields.rst
