-----------------
Export and Import
-----------------

.. _export_import:

Triton allows you to export your inventory to external file.
Triton uses its own protected file format with ".triton" extension.
File content is encrypted and signed, so no one is able to modify it outside of Triton app.

Exporting things into files is useful for sharing across different Triton instances or backup purposes.

You can export and import campaigns, groups (all at once), lists, templates, tracking categories (all at ones) and triggers.

.. note:: As you may know some entities has related things.For example: campaign has related template and list.

Triton does not export related stuff, but only object itself with references.
So if you want to export full campaign data you should export template, list and campaign itself.


.. important:: Triton refs to lists, templates, categories and groups by their name (not id).

