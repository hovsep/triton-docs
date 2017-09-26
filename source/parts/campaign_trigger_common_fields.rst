- Email transport
     Which transport should be used for given campaign.

- Subject
     Triton supports three ways of generating email subject:

     - static (default)
        Triton will use same subject-template for all recipients.

     - sequental
        Triton will determine which subject was used for given recipient last time and use next one (round-robin distribution).
        Useful to avoid email chaining by subject (Hello Gmail :)).

     - random
        Triton will use random subject for each recipient.

.. note:: All types of subjects still are Blade templates, therefore you can use any valid blade or php code within a subject.

- Description
     Just optional field for easy management.

- Custom Headers
     This section allows you to add custom HTTP-headers.
     May be useful for tuning your emails (e.g. List-Unsubscribe).

- Custom variables
     The section allows you to add own variables into template rendering context (:ref:`Learn more <context>`).
     For example you may want to add a variable called ``utm_common`` with value like "utm_campaign=camp55&utm_source=email&utm_medium=newsletter".
     Then you are able to use that variable right in template.

     For example:

     .. code-block:: guess

        <a href="http://mysite.com/example?utm_content=ad7&"{{ $utm_common }}>Click me ASAP</a>

     In this example we can add some common params to our links.
     Further, if we consider to change the value of ``$utm_common`` we will be able to modify it from single place.

     You can use this tool on your own case, just find some repeatable data and use it as variable.

- Tracking category
     Specifies the category that will capture sendings and openings.

- Group
     Allows to group things together for easy searching. Learn more about grouping.