---------
Campaigns
---------

Triton supports not only transactional mailing but also newsletters (bulk emails).
The Campaign is a tool for sending newsletters.

Each campaign must have single Template and single List (you should take care of having them before campaign creation).

Campaign fields are following:

- Name
    May be any string (spaces are allowed).
    Used just for identifying.

- Template
    The template that will be used for emails in given campaign.

- List
    The list of recipients. :ref:`Learn more about lists<lists>`.

- Schedule at
    Date and time (server time used) when campaign should start.

.. include:: parts/campaign_trigger_common_fields.rst


How campaigns works
===================

Campaign may be created or imported from external file (only special .triton files are supported :ref:`learn more <export_import>`).
After campaign addition you will see it on the list.
Newly added campaign will have an **Idle** state.
By the way, possible campaign states are following:

- Idle
    Nothing occurs, because it's not time yet.

- Pending
    Campaign is ready to start and is pending for worker.

- Paused
    Campaign is paused by user.

- Running
    Campaign is running.

- Finished
    Campaign is finished.

Campaign may be started by schedule or immediately. You can run, pause or stop campaign any time you want.
Running a campaign may take up to one minute.
Triton uses two phase sending flow.
On the first phase campaign is getting enqueued (Triton generates a job for each recipient).
On the second phase workers are pulling jobs from queue and sending emails to recipients.
Both processes may proceed in parallel.

You can monitor campaign enqueing and sending progress, just hover cursor to blue label near state badge and you will see four metrics: **Enqueued**, **Sent**, **Skipped** and **Plan**.
Also you will see a progress bar.

A few words on Pause and Stop difference
========================================

When campaign is getting stopped, Triton forgets which recipients already received a newsletter, so if you stop and start the campaign recipients may receive email twice.
When you just pause a campaign, Triton just pauses workers, all info about processed recipients stays, so you can safely resume your campaign and do not worry about duplicating mails, Triton will avoid them.

Summing up, you should Stop&Run campaign if you want to re-send an email to all recipients and Pause&Run if you need to continue sending omitting already processed recipients.


