---------
Dashboard
---------

The dashboard provides general information about how your Triton is working.
It is Triton's main page, so you will see it every time you log in.


The page divided to several sections:

- Today's metrics
    Shows operational counters for today. When everything is ok you usually should see zeroes at "Skipped", "Requeued" and "Stats fails" counters.
    Counters are getting updated in near real time.

- System directories
    Shows main directories on your server's file system which must be writable.
    Troubled paths will be marked with red color.
    You should always be sure that that directories have correct file permissions.

- Server health
    Shows some general OS metrics, such as LA (Load Average).

- App health
    Shows Triton-specific metrics such as license expire date, triggers count, etc.
