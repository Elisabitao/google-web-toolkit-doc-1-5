# What is the Same Origin Policy, and how does it affect GWT? #
Modern browsers implement a security model known as the _Same Origin Policy_ (SOP). Conceptually, it is very simple, but the limitations it applies to JavaScript applications can be quite subtle.

Simply stated, the SOP states that JavaScript code running on a web page may not interact with any resource not originating from the same web site. The reason this security policy exists is to prevent malicious web coders from creating pages that steal web users' information or compromise their privacy. While very necessary, this policy also has the side effect of making web developers' lives difficult.

It's important to note that the SOP issues described below are not specific to GWT; they are true of any AJAX application or framework. This FAQ simply details the specific ways in which GWT is affected by this universal limitation.

## How the SOP Affects GWT ##
When you compile a GWT application, it produces JavaScript code and HTML files. Because a GWT application is broken up into multiple files, the browser will make multiple requests from the HTTP server to fetch all the pieces. Previous to GWT 1.4, this meant that all the GWT files must live on the same web server as the page which launches the GWT application so as not to break the SOP. In GWT 1.4, this is no longer an issue thanks to the new cross-site script inclusion bootstrap process mode.

Typically, the bootstrap process will load the GWT application files from the same web server that is serving the HTML host page. That is, supposing the host HTML file was located at `http://mydomain.com/index.html`, the GWT application files would also be in the same directory (or a subdirectory) as the index.html file. To load the GWT application, the `<`module`>`.nocache.js file would be bootstrapped by having the index.html file include a `<`script`>` tag referencing the `<`module`>`.nocache.js file. This is the standard bootstrap process to load a GWT application. The `<`script`>` tag that needs to be included in the main host HTML page is shown below:
```
<script language="JavaScript" src="http://mydomain.com/<module>.nocache.js"></script>
```
However, many organizations setup their deployment platform in such a way that their main host HTML page is served up from http://mydomain.com/, but any other resources such as images and JavaScript files are served up from a separate static server under `http://static.mydomain.com/`. In older versions of GWT, this configuration would not be possible as the SOP prevented the GWT bootstrap process from allowing script from files that were added from a different server to access the iframe in the main host HTML page. The new GWT 1.4 bootstrap model now provides support for this kind of server configuration.

This is possible due to the `<`module`>`-xs.nocache.js file generated by the GWT compiler. Basically, this is a cross-site version of the `<`module`>`.nocache.js file. To setup the bootstrap process in cross-site mode, the `<`script`>` tag in the main HTML host page just needs to refer the `<`module`>`-xs.nocache.js file rather than the `<`module`>`.nocache.js counterpart. The `<`module`>`-xs.nocache.js file, as well as all the other GWT application resources, can reside on a static server, say http://static.mydomain.com/, while the main HTML host page can be hosted on a separate server on http://mydomain.com/. As long as the script tag is referencing the cross-site version of the .nocache.js file and the other application are in the same path as that file, the application can be loaded using the cross-site bootstrap process. The `<`script`>` tag including the cross-site JS file is shown below:
```
<script language="JavaScript" src="http://static.mydomain.com/<module>-xs.nocache.js"></script>
```

For more details on the GWT 1.4 bootstrap process, see ["What's with all the cache/nocache stuff and weird filenames?"](FAQ_GWTApplicationFiles.md)

## SOP, GWT, and XMLHTTPRequest Calls ##
The second area where the SOP is most likely to affect GWT users is the use of XMLHTTPRequest -- the heart of AJAX. The SOP limits the XMLHTTPRequest browser call to URLs on the same server from which the host page was loaded. This means that it is impossible to make any kind of AJAX-style request to a different site than the one from which the page was loaded. For example, you might want to have your GWT application served up from `http://pages.mydomain.com/` but have it make requests for data to `http://data.mydomain.com/`, or even a different port on the same server, such as `http://pages.mydomain.com:8888/`. Unfortunately, both these scenarios are impossible, prevented by the SOP.

This limitation affects all forms of calls to the server, whether JSON, XML-RPC, or GWT's own RPC library. Generally, you must either run your CGI environment on the same server as the GWT application, or implement a more sophisticated solution such as round-robin DNS load balancing or a reverse-proxy mechanism for your application's CGI calls.

In certain cases, it is possible to work around this limitation. For an example, see ["How can I dynamically fetch JSON feeds from other web domains?"](FAQ_JSONFeedsFromOtherDomain.md)

## SOP and GWT Hosted Mode ##
The SOP applies to all GWT applications, whether running in web (compiled) mode on a web server, or in hosted mode. For example, it is perfectly possible to launch a GWT application from a `file://` URL pointing to a file on your local computer. Though GWT will run in this case, it will be unable to access any resources from a web server because it will be constrained to the "site" from which it was loaded--your computer's filesystem.

This can cause problems if you are attempting to develop a GWT application that uses server-side technologies not supported by the GWT hosted mode, such as EJBs or Python code. In such cases, you can use the `-noserver` argument for hosted mode to launch your GWT application from the server technology of your choice. For more information, see ["How do I use my own server in hosted mode instead of GWT's built-in Tomcat instance?"](FAQ_HostedModeNoServer.md) Even if you choose to use this feature, however, remember that the SOP still applies.