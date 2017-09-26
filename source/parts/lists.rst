-----
Lists
-----

Lists are used to manage recipients. Think of it just as about group of recipients.

List has few general fields: name,description, group.
But the main power is in its columns.

As we say above List is used to store recipients, while recipient may have many different properties such as first name, last name, balance and whatever you want.
So List is very similar to relational database table (actually it is it).
Triton allows you to create lists with custom columns.
There are some restrictions for columns:

- Columns are immutable. You can not edit them later.
- Column name should be valid PHP-identifier.
- Column name is case-insensitive.

For example ``user_age``, ``userSex``, ``TODAY_EARNINGS``, ``link2`` - are valid column names, while ``user age``, ``2link``, ``[;.]`` - are not.

By design each List will contain two default columns: ``email`` and ``user_id``.
These fields are reserved by :ref:`DataContract protocol <sdk>`.

Keep in mind that columns defined within the list will be available from template.
They will be injected as PHP variables on rendering time.