# adp-down

Download all your PDFs from the ADP World page via script instead of having
to click on/download all of them individually.

There is an existing, more convenient Python script from @asdil12
that unfortunately is limited by its inability to interpret Javascript and
can only download the last 20 PDFs.

The script from this repo allows you to download everything but is more
cumbersome to use.

(This script still works with the July 2020 redesign of ADP World.)

## dependencies

* Firefox (Chrome also works but the UI is a little different and not described
  below)
* curl 7.55 or newer
* basic GNU tools (Bash, sed, cat, ...)

## how?

1. Clone this repo. `cd` to the cloned repo.

2. Open adpworld.de in Firefox. Log in. Wait for everything to load.

3. Open Firefox Developer Tools with F12, open Network panel.

4. Select "50 Zeilen pro Seite". (This is important because that is the first
   XHR request the browser makes and we're going to use the XHR-requested XML
   files for the download later.)

5. Use the the `>` button at the top of the page to click through all of the
   results pages.

6. At the bottom of the network panel, all requests that the browser made in
   the meantime will appear.
   Right-click the first one of Type "xhr", then Copy -> Copy as cUrl

7. Paste this command to a command line window and add a file name to save the
   file to: `curl ... > adp-url-1`. (Make sure to use a file name that starts
   with `adp-url-`, as the script checks for that.)

8. Repeat steps 6 and 7 for each of the XHR requests you see in the Network
   panel. Use a different file name to save to each time: `adp-url-2`, `adp-url-3`, ...

9. In the Network panel, right-click any XHR request and select
   Copy -> Copy Request Headers. Paste the copied request headers into a file
   named `headers.txt`.
   Make sure to remove the first line (the one that starts with GET or POST)!

10. Check that the following is all true:
   * The downloaded XML files and the headers file and the script are all in the
     same directory. This directory must also be your working directory.
   * The downloaded XML files match the glob pattern `adp-url-*`
   * Your headers file is called `headers.txt` and looks approximately like this:
     ```
     Host: www.adpworld.de
     User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
     Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
     Accept-Language: en-US,en;q=0.5
     Accept-Encoding: gzip, deflate, br
     Referer: https://www.adpworld.de/wps/myportal/ADPWorld/Applications/Archiv/ePayslip/
     Connection: keep-alive
     Cookie: <VERY LONG COOKIE STRING> 
     Upgrade-Insecure-Requests: 1
     Pragma: no-cache
     Cache-Control: no-cache
     ```

11. Run the script: `./adp-down`
