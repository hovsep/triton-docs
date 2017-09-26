------------
Mail archive
------------

Sometimes you may want to know which mail was exactly sent at which time.
You may investigate some issue or something else, anyway some kind of emails log is very useful.
Triton stores outgoing emails at two places respectively: outbox and archive dir.

Outbox
======
Outbox is an ordinary database table that stores just brief info about sent mail: mail_id, recepient address and subject.
There is no GUI for that table, it is only for developer's use.

Archive dir
===========
Archive stores whole letter as eml file. It contains headers and mail body, so you can open it in any email client.
By default Triton uses "storage/app/archive" directory, you can change it at config/filesystems.php file, in "disks" section.

.. note:: There is no tools for cleaning up archive dir comes with Triton.
You should implement it on your own.
It may be bash script running under cron, for example.

Configuration
=============
By default Triton stores only transactional emails for 2 last months, but you can tune archive behaviour by changing few parameters at config/triton.php file:
- archive_trigger_mails
    Boolean flag
- archive_campaign_mails
    Boolean flag
- outbox_keep_period
    Number of months to keep outbox records only.
    Do not confuse it with archive files, they will be stored always.
