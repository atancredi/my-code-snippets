# Avoiding detection

To decrease your chances of ReCAPTCHA detecting automated queries, try the following:

-   Use a custom user-agent header (Make sure it's not a headless user-agent!)
-   Use a hard-to-detect web driver (Good example: [https://github.com/ultrafunkamsterdam/undetected-chromedriver](https://github.com/ultrafunkamsterdam/undetected-chromedriver))
-   Use proxies to avoid IP blacklisting

An example of a good user-agent: Mozilla/5.0 (Windows NT 4.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2049.0 Safari/537.36

An example of a bad user-agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) HeadlessChrome/92.0.4512.0 Safari/537.36

Note the last part of the user-agent; the headless specification is usually there.