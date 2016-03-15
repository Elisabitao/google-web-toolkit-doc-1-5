# Get JSON via HTTP #

In the world of Web 2.0, [JSON](http://www.json.org/) is an increasingly popular data format used by AJAX developers.  GWT makes it easy to join the party and use JSON within _your_ client-side web applications.

## What is JSON? ##

JSON is a universal, language-independent format for data.  In this way, it's similar to [XML](http://www.w3.org/XML/).  However, whereas XML uses tags, JSON is based on the object-literal notation of JavaScript.  It is therefore simpler than XML and in general, JSON-encoded data is less verbose than the equivalent data in XML.

For example, suppose we wanted to download stock quotes from a web server via HTTP.  If the information was formatted as JSON, it might look something like this:

```
$PP_OFF
[
  {
    "symbol": "BA",
    "price": 87.86,
    "change": -0.41
  },
  {
    "symbol": "KO",
    "price": 62.79,
    "change": 0.49
  },
  {
    "symbol": "JNJ",
    "price": 67.64,
    "change": 0.05
  }
]
```

### JSON vs. XML vs. RPC ###

Of course, transmitting JSON over HTTP is not the only way to communicate with a web server.  We've already seen how we can use [GWT RPC](GettingStartedRPC.md) to send Java objects to and from our backend server.  If you're using XML, GWT also contains a package of [types](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/xml/client/package-summary.html) useful for building and parsing XML documents.

Which data format you ultimately use will largely depend on the server you want to interact with.  If you are simply creating an interface for your application's server-side business logic, [GWT RPC](DevGuideRemoteProcedureCalls.md) is probably your best bet.  If your application talks to a server that cannot host RPC servlets, or one that already uses another data format like JSON or XML, you'll need to use GWT's generic [HTTP client classes](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/package-summary.html).  If you're creating a mashup-type application that needs to use data from one or more remote web servers, you'll likely use GWT's [JavaScript Native Interface (JSNI)](DevGuideJavaScriptNativeInterface.md) to retrieve JSON-encoded data.

This article will only cover making HTTP requests for JSON data (although the code can easily be adapted to work with XML data instead).  There are a variety of public sources of JSON-formatted data you can practice with, including Google's [GData](http://code.google.com/apis/gdata/json.html) and [Yahoo! Web Services](http://developer.yahoo.com/common/json.html).

## Working with JSON data ##

Now that you know what JSON is, we'll talk about how to create and use JSON data.

### In JavaScript ###

Since JSON is essentially a subset of the JavaScript language, it is convenient to [use JSON in JavaScript code](http://www.json.org/js.html).  You can create a JSON object just as you would any JavaScript object (since they are really the same thing), using object literal notation:

```
var point = { "x": 3, "y": 4 };
```

The more interesting case is converting JSON text from the server into a live JavaScript object.  The simplest and fastest way to do this is by using JavaScript's built-in `eval()` function, which can correctly parse valid JSON text and produce a corresponding object:

```
var jsonObject = eval('(' + jsonText + ')');
```

However, because `eval()` can execute _any_ JavaScript code (not just JSON data) this approach has some serious security implications.  A much safer option is to use a dedicated JSON parser instead, which will _only_ parse JSON text and never executable JavaScript code. This can be done by verifying the String to parse with the regular expression defined in [RFC 4627](http://www.ietf.org/rfc/rfc4627.txt).

GWT comes with its own JSON parser, but for the moment doesn't check for unsafe JSON for performance reasons. [There may be some changes to the parser](http://code.google.com/p/google-web-toolkit/issues/detail?id=1749) to add an extra method which does check for unsafe JSON strings before actually parsing them, but for the time being you can easily check against unsafe JSON manually by using the regex defined in RFC 4627 when needed.

For the purposes of JSON support for our StockWatcher application, we'll use the GWT JSON parser and assume that the JSON we're going to parse is safe.

### GWT JSON types ###

As you might expect, JSON data types correspond to the built-in types of JavaScript.  JSON can encode strings, numbers, booleans, and null values, as well as objects and arrays composed of those types.  As in JavaScript, an _object_ is actually just an unordered set of name/value pairs.  In JSON objects, however, the values can only be other JSON types (never functions containing executable JavaScript code).

GWT contains a full set of [JSON  types](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/package-summary.html) for manipulating JSON data.  In the `com.google.gwt.json.client` package you'll find classes for each of the JSON types, for example, [JSONString](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONString.html) and [JSONArray](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONArray.html).  [JSONValue](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONValue.html) is the superclass of all JSON value types, and contains methods for converting a JSONValue to a more specific JSON type.

To actually convert a JSON string into something you can work with, you'll need to use the static [JSONParser.parse(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONParser.html#parse(java.lang.String)) method.  This returns a [JSONValue](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONValue.html).  If you need to do the opposite conversion, from a JSON object back into a `String`, you can use the
[toString()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONValue.html#toString()) method of any JSON value type.

## Making HTTP requests ##

Now then, how do we actually get the JSON text from the server?  In GWT, we'll use the [HTTP client classes](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/package-summary.html) in the `com.google.gwt.http.client` package.  These classes contain the functionality for making asynchronous HTTP requests.

To send a request, you'll need a [RequestBuilder](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html) object.  You'll start by specifying the HTTP method (GET, POST, etc.) and URL in the [constructor](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html#RequestBuilder(com.google.gwt.http.client.RequestBuilder.Method,%20java.lang.String)).  If necessary, you can also set the username, password, timeout, and headers to be used in the HTTP request.  When you're ready to make the request, call [sendRequest(String, RequestCallback)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html#sendRequest(java.lang.String,%20com.google.gwt.http.client.RequestCallback)).

The [RequestCallback](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestCallback.html) argument you pass will handle the response in its [onResponseReceived(Request, Response)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestCallback.html#onResponseReceived(com.google.gwt.http.client.Request,%20com.google.gwt.http.client.Response)) method, which is called when and if the HTTP call completes successfully.  If the call fails (for example, if the HTTP server is not responding), the [onError(Request, Throwable)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestCallback.html#onError(com.google.gwt.http.client.Request,%20java.lang.Throwable)) method is called instead.  The RequestCallback interface is analogous to the [AsyncCallback](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/AsyncCallback.html) interface we learned about in the section on [remote procedure calls](GettingStartedRPC.md).

## Adding JSON support to StockWatcher ##

Ready to see JSON in action?  Right now our StockWatcher application uses [GWT RPC](GettingStartedRPC.md) to retrieve stock quote data from the server.  That's great, but let's suppose that we now need to download the quotes from a server in JSON format.  Let's adapt StockWatcher to make that happen.

### Data source ###

First, we need to create our hypothetical JSON data source.  We'll just go ahead and create a simple [PHP](http://www.php.net/) script called `stockPrices.php` to serve up our stock quotes in JSON format:

```
<?php

  header('Content-Type: text/javascript');
  header('Cache-Control: no-cache');
  header('Pragma: no-cache');
      
  define("MAX_PRICE", 100.0); // $100.00
  define("MAX_PRICE_CHANGE", 0.02); // +/- 2%
  
  echo '[';
  
  $q = trim($_GET['q']);
  if ($q) {
    $symbols = explode(' ', $q);
  
    for ($i=0; $i<count($symbols); $i++) {
      $price = lcg_value() * MAX_PRICE;
      $change = $price * MAX_PRICE_CHANGE * (lcg_value() * 2.0 - 1.0);
      
      echo '{';
      echo "\"symbol\":\"$symbols[$i]\",";
      echo "\"price\":$price,";
      echo "\"change\":$change";
      echo '}';
      
      if ($i < (count($symbols) - 1)) {
        echo ',';
      }
    }
  }
    
  echo ']';
?>
```

We're reusing the logic from our RPC server-side code to generate some random price and change values for each stock.  The only difference is that now we're encoding the data in JSON before sending it back to the client.  Add the `stockPrices.php` script above to the `public` directory of your StockWatcher project.  Then run the `StockWatcher-compile` script to create the [web mode](GettingStartedWebMode.md) files for the application (which will now include `stockPrices.php`).  Now deploy the compiled StockWatcher files in the `www/com.google.gwt.sample.stockwatcher.StockWatcher` directory to a `/StockWatcher` directory in whatever web server you have locally installed (Apache, IIS, etc.).  Just be sure that it has [PHP installed](http://www.php.net/downloads.php).

Try testing our stock quote server by navigating to `http://localhost/StockWatcher/stockPrices.php?q=ABC+DEF` in a web browser.  It should load text that looks something like this, except without the whitespace and indentation (which we omit to keep the download size small):

```
$PP_OFF
[
  {
    "symbol": "ABC",
    "price": 96.204659543522,
    "change": -1.6047997669492
  },
  {
    "symbol": "DEF",
    "price": 61.929176899084,
    "change": 0.22809544419493
  }
]
```

So it looks like we have a working JSON stock quote service.  Now we need to point StockWatcher to the new data source.

### Retrieving JSON data ###

Like the [internationalization types](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/package-summary.html), the HTTP and JSON types are contained within separate GWT modules that we'll need to inherit.  Add the following `<inherits>` tags to your `StockWatcher.gwt.xml` file:

```
<inherits name="com.google.gwt.json.JSON" /> 
<inherits name="com.google.gwt.http.HTTP" />
```

We can then import all of the required Java types into `StockWatcher.java`:

```
import com.google.gwt.json.client.JSONArray;
import com.google.gwt.json.client.JSONException;
import com.google.gwt.json.client.JSONNumber;
import com.google.gwt.json.client.JSONObject;
import com.google.gwt.json.client.JSONParser;
import com.google.gwt.json.client.JSONString;
import com.google.gwt.json.client.JSONValue;
import com.google.gwt.http.client.Request;
import com.google.gwt.http.client.RequestBuilder;
import com.google.gwt.http.client.RequestCallback;
import com.google.gwt.http.client.RequestException;
import com.google.gwt.http.client.Response;
import com.google.gwt.http.client.URL;

import java.util.Iterator;
```

Rather than hard-coding the URL for the JSON server, let's add a constant to the StockWatcher class:

```
private static final String JSON_URL = "http://localhost/StockWatcher/stockPrices.php?q=";
```

Now for the real fun: making the HTTP request and parsing the JSON response.  Go ahead and comment out the existing refreshWatchList() function in StockWatcher and add this new version, which we'll walk through together:

```
private void refreshWatchList() {
  if (stocks.size() == 0) {
    return;
  }
  
  // add watch list stock symbols to URL
  String url = JSON_URL;
  
  Iterator<String> iter = stocks.iterator();
  while (iter.hasNext()) {
    url += iter.next();
      if (iter.hasNext()) {
        url += "+";
      }
  }
  
  url = URL.encode(url);
  
  RequestBuilder builder = new RequestBuilder(RequestBuilder.GET, url);
  
  try {
    Request request = builder.sendRequest(null, new RequestCallback() {
      public void onError(Request request, Throwable exception) {
         displayError("Couldn't retrieve JSON");         
      }

      public void onResponseReceived(Request request, Response response) {
        if (200 == response.getStatusCode()) {
          try {
            // parse the response text into JSON
            JSONValue jsonValue = JSONParser.parse(response.getText());
            JSONArray jsonArray = jsonValue.isArray();
            
            if (jsonArray != null) {
              updateTable(jsonArray);
            } else {
              throw new JSONException(); 
            }
          } catch (JSONException e) {
            displayError("Could not parse JSON");
          }
        } else {
          displayError("Couldn't retrieve JSON (" + response.getStatusText() + ")");
        }
      }       
    });
  } catch (RequestException e) {
    displayError("Couldn't retrieve JSON");         
  }
}
```

As you can see, our new refreshWatchList() method starts out by tacking the stock symbols onto the end of the service URL's query string.  We use use GWT's [URL.encode(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/URL.html#encode(java.lang.String)) static method to take care of any special characters.  Once we have the final URL in hand, we pass it to a new [RequestBuilder](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html) that sends the request to the JSON server.

If the server replies with a "Success" (200) status code, we get the JSON string out of the response text and run it through the [JSONParser](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONParser.html).  If the JSON was well-formed, we get back a [JSONValue](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONValue.html).  In our case, this JSONValue will actually be a [JSONArray](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONArray.html) containing our stock quotes.  If all goes well, we forward the JSONArray on to a new overload of `updateTable()`, which we'll implement in just a bit.

If something breaks along the way (e.g, the server is offline, or the JSON is malformed), we'll trap the error and display the message using a new function: displayError(String).

```
private void displayError(String error) {
  errorMsgLabel.setText(messages.errorMsg(error));
  errorMsgLabel.setVisible(true);
}
```

You'll notice that we're not retrieving strings from the [Constants](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Constants.html) or [Messages](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/i18n/client/Messages.html) classes that we set up in the Getting Started section on [internationalization](GettingStartedI18n.md).  Thus, our JSON error messages will always be displayed in English regardless of the user's locale.  It should be pretty easy to replace them with translated strings, and if this were a real application, we would probably want to translate _all_ strings in the UI.  However, to keep things simple, we'll stick to English for the new code in our StockWatcher sample.

Getting back on track, we now need to add a new updateTable() overload that accepts a [JSONArray](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONArray.html).  Go ahead and add this function to the StockWatcher class:

```
private void updateTable(JSONArray array) {
  ArrayList<StockPrice> stockPrices = new ArrayList<StockPrice>();
  JSONValue jsonValue;
  
  for (int i=0; i<array.size(); i++) {
    JSONObject jsStock;
    JSONString jsSymbol;
    JSONNumber jsPrice, jsChange;
    
    if ((jsStock = array.get(i).isObject()) == null) continue;
    
    if ((jsonValue = jsStock.get("symbol")) == null) continue;
    if ((jsSymbol = jsonValue.isString()) == null) continue;
    
    if ((jsonValue = jsStock.get("price")) == null) continue;
    if ((jsPrice = jsonValue.isNumber()) == null) continue;
    
    if ((jsonValue = jsStock.get("change")) == null) continue;
    if ((jsChange = jsonValue.isNumber()) == null) continue;
    
    stockPrices.add(new StockPrice(jsSymbol.stringValue(),
        jsPrice.getValue(),
        jsChange.getValue()));
  }

  updateTable(stockPrices.toArray(new StockPrice[0]));
}
```

Here, we simply iterate through the JSON array and extract the values from each  [JSONObject](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONObject.html).  As we navigate the JSON object hierarchy, we check to ensure that the data is structured as we expect it.  If anything is out of place, we simply discard that stock's data and move on (you could throw an exception instead, of course, to prevent partial updates).  Once we have the actual values (which we got via the [JSONString.stringValue()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONString.html#stringValue()) and [JSONNumber.getValue()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONNumber.html#getValue()) methods), we defer to the existing updateTable(StockPrice[.md](.md)) function to update the UI.

### Testing HTTP/JSON ###

Alright then, time to fire it up and test the changes.  Run the `StockWatcher-compile` script to translate the application to [web mode](GettingStartedWebMode.md) again and then redeploy it to your local web server.  If you run StockWatcher now... well, nothing _looks_ any different, but the quote data is now coming from our JSON-serving `.php` script, not an RPC servlet.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherJson.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherJson.png)

So is that all there is to it?  If you're retrieving JSON data from your own web server, then yes.  If not, things get a little more interesting.

## Cross-site JSON ##

Let's change the StockWatcher equation ever so slightly.  Suppose that instead of hosting the stock quote service on our own web server, we had to talk to somebody _else's_ server to get the data (this is the idea behind AJAX [mashups](http://en.wikipedia.org/wiki/Mashup_(web_application_hybrid)), for example).  So let's try that and see what happens.

First, we'll need another server.  Now, you could go ahead and actually set up a second web server on another computer or in a virtual machine (we're already using `localhost` for serving up the StockWatcher application).  However, for our purposes, we'll just run another HTTP server on our own computer under a different port number (your `localhost` server probably uses the standard port 80).  To accomplish this, we'll create a bare-bones web server using the following [Python](http://www.python.org/) script:

`quoteServer.py`:

```
#!/usr/bin/env python2.4
#
# Copyright 2007 Google Inc. All Rights Reserved.

import BaseHTTPServer
import SimpleHTTPServer
import urllib
import random

MAX_PRICE = 100.0
MAX_PRICE_CHANGE = 0.02

class MyHandler(SimpleHTTPServer.SimpleHTTPRequestHandler):
  
  def do_GET(self):
    form = {}
    if self.path.find('?') > -1:
      queryStr = self.path.split('?')[1]
      form = dict([queryParam.split('=') for queryParam in queryStr.split('&')])
    
    body = '['

    if 'q' in form:
      quotes = []

      for symbol in urllib.unquote_plus(form['q']).split(' '):
        price = random.random() * MAX_PRICE
        change = price * MAX_PRICE_CHANGE * (random.random() * 2.0 - 1.0)
        quotes.append(('{"symbol":"%s","price":%f,"change":%f}'
                       % (symbol, price, change)))

      body += ','.join(quotes)

    body += ']'

    if 'callback' in form:
      body = ('%s(%s);' % (form['callback'], body))
       
    self.send_response(200)
    self.send_header('Content-Type', 'text/javascript')
    self.send_header('Content-Length', len(body))
    self.send_header('Expires', '-1')
    self.send_header('Cache-Control', 'no-cache')
    self.send_header('Pragma', 'no-cache')
    self.end_headers()
    
    self.wfile.write(body)
    self.wfile.flush()
    self.connection.shutdown(1)
    
bhs = BaseHTTPServer.HTTPServer(('', 8000), MyHandler)
bhs.serve_forever()
```

Don't worry if you don't know Python; the script should be fairly easy to follow.  It basically does the same thing as the `.php` script we saw earlier.  It's just generating random price and change values for each stock symbol, and wrapping up the output in JSON format.  Notice in the `BaseHTTPServer.HTTPServer` constructor that it will be running on port 8000.

Save the script to the main `StockWatcher` directory and then launch it from a command shell with the following command (make sure the Python interpreter is on your PATH):

```
python quoteServer.py
```

The server will start, although you won't see any output immediately (it will log each HTTP request).

Let's try it out.  Open a web browser and navigate to:

```
$PP_OFF
http://localhost:8000/?q=ABC+DEF
```

The output it generates should be pretty similar to what you saw from `stockPrices.php`.  The JSON format is exactly the same as before, so let's plug in the new URL in StockWatcher.  In `StockWatcher.java`, change the `JSON_URL` constant as follows:

```
private static final String JSON_URL = "http://localhost:8000/?q=";
```

Now recompile the project and copy the new files to the `/StockWatcher` directory in your local web server.  Now launch the updated StockWatcher in your browser.

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherSopError.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherSopError.png)

As you may have noticed, StockWatcher wasn't able to retrieve the JSON stock feed data. The question is, why didn't this work?  We know our Python JSON server is working correctly: we accessed it directly from the browser, after all.  So what's stopping StockWatcher?

### Same origin policy ###

The obstacle we just ran into is the [same origin policy (SOP)](http://en.wikipedia.org/wiki/Same_origin_policy).  SOP is a browser security measure that restricts client-side JavaScript code from interacting with resources originating from "other" websites; that is, resources loaded from any other domain and/or port.  So, for example, JavaScript running in a web page on `http://abc.com:80` _cannot_ interact with data loaded from `http://xyz.com` or even `http://abc.com:81`.

As a result of SOP, the browser's internal [XMLHTTPRequest](http://www.w3.org/TR/XMLHttpRequest/) object that is making our HTTP calls is prevented from sending requests to an outside web server (read this [FAQ article](FAQ_SOP.md) for a more detailed description of SOP and its effect on GWT).

Now that we know what SOP is and how it affects web applications running in the browser, we can diagnose the problem with StockWatcher.  StockWatcher is being served up via our local web server on `http://localhost:8080`, but the Python server is running on `http://localhost:8000`.  Since the port numbers don't match, the web browser blocks our HTTP call to retrieve the JSON.

### Working around SOP ###

Now that we've figured out what's wrong, the next question is: how can we fix it?  Fortunately, it is possible to work around same origin policy.  We have two options.

#### 1. Proxy on your own server ####

Our first option is to play by the rules of SOP and simply create a proxy on our own server.  We can then make HTTP calls to our server (which is of course allowed) and have _it_ go fetch the data from the remote server.  Since the code running on our web server is not subject to SOP restrictions, this works just fine.  For our StockWatcher application, we could write server-side code to download (and maybe cache) the JSON-encoded stock quotes from a remote server.  We can then use any mechanism we want for talking to _our_ server, for example, [GWT RPC](GettingStartedRPC.md) or direct HTTP using [RequestBuilder](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html).

One downside to this approach is that it requires server-side code.  Another is that the extra HTTP call increases the latency of remote calls and adds to the workload on our web server.

#### 2. Load the JSON response into a 

&lt;script&gt;

 tag ####

Another option is to dynamically load JavaScript into a `<script>` tag.  Client-side JavaScript can manipulate `<script>` tags, just like any other element in the [HTML Document Object Model (DOM)](http://www.w3.org/TR/DOM-Level-2-HTML/).  Client-side code can set the `src` attribute of a `<script>` tag to automatically download and execute new JavaScript into the page.  As it turns out, this method _is not_ subject to SOP restrictions, so we can effectively load JavaScript (and therefore JSON) from remote servers without a hitch.

Well, maybe _one_ small hitch.  When we load JavaScript this way, the browser retrieves the code asynchronously, but it doesn't notify us when it's finished.  Instead, it simply executes the new JavaScript.  However, by definition, JSON cannot contain executable code.  Put the two together and you'll realize that we can't load plain JSON data using a `<script>` tag.

What we _can_ do however, is wrap the JSON data in a function call, which will then execute when the browser finishes downloading the new contents of the `<script>` tag.  This technique has been called [JSON with Padding (JSONP)](http://bob.pythonmac.org/archives/2005/12/05/remote-json-jsonp/).  The additional client-side requirement is that we need to include in our HTTP request the name of the JavaScript function we're using as a callback.  The web server will then wrap the JSON response in a call to that function.  [Google Data APIs](http://code.google.com/apis/gdata/), for example, [supports this](http://code.google.com/apis/gdata/json.html).  Eagle-eyed readers may have already noticed that our tiny Python web server also supports this through the `callback` query string parameter:

```
if 'callback' in form:
      body = ('%s(%s);' % (form['callback'], body))
```

## Fixing StockWatcher ##

Let's go with option 2 and pass our Python server a JavaScript callback function.  However, we can't just create another function in `StockWatcher.java` and pass its name as the callback.  The reason is that when the GWT compiler translates our Java source code to JavaScript, one of the optimizations it performs is shortening our function names to minimize the size of the `.js` file.  For example, a `myCallback()` Java function may end up as `qd()` in web mode.  This means our callback will need to be a "real" JavaScript function (that is, one that's _not_ translated from Java).

Before we tackle that problem, let's take a step back and figure out what raw JavaScript we would use to download the JSON data.  Here's a first pass that includes no error checking:

```
var url = "http://localhost:8000/?q=ABC+DEF&callback=jsonCallback";

var script = document.createElement("script");
script.setAttribute("src", url);
script.setAttribute("type", "text/javascript");

window.jsonCallback = function(jsonObj) {
  // use JSON object here

  // cleanup
  document.body.removeChild(script);
  delete window[callback];
}

document.body.appendChild(script);
```

The script starts by setting up a `<script>` element pointing to the URL that will retrieve the JSON wrapped in a call to the function `jsonCallback()`.  That function is defined on the browser's `window` object.  It receives the JSON object as the `jsonObj` argument.  Before it returns, it removes the new `<script>` element and itself from `window`.  With the callback defined, the final step is to call `appendChild()` to attach the dynamically-loaded `<script>` element to the HTML document body.  This causes the web browser to download the JavaScript referenced by the `src` attribute.

That wasn't too difficult, was it?  Now if only we could just inject that raw JavaScript directly into our compiled GWT application.  Well guess what? We can!

### Creating a bridge method with JSNI ###

GWT allows you to easily mix handwritten JavaScript with your Java source code using something called [JavaScript Native Interface (JSNI)](DevGuideJavaScriptNativeInterface.md).  JSNI is sort of the GWT equivalent of inline assembly code.  We can use it to implement methods directly in JavaScript.  We can even call our Java methods and access Java fields from within native JavaScript.  It's just what we need for adding a JSON callback method to StockWatcher.

To [create a JSNI method](DevGuideJavaScriptFromJava.md), we declare the method as `native` and define the JavaScript within a specially- formatted comment block.  Within that block we use regular JavaScript code just as we would if we were not using GWT.  One special case is when we need to [use Java methods and fields](DevGuideJavaFromJavaScript.md).  Calls to Java methods in JSNI have the following form:

```
$PP_OFF
[instance-expr.]@class-name::method-name(param-signature)(arguments)
```

Accessing fields requires a very similar syntax:

```
$PP_OFF
[instance-expr.]@class-name::field-name
```

We'll use this mechanism in our JSNI method to pass the retrieved JSON object to a Java method in the StockWatcher class with the following signature:

```
public void handleJsonResponse(JavaScriptObject jso);
```

Notice the type of the method parameter: [JavaScriptObject](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/JavaScriptObject.html).  A JavaScriptObject in GWT is a handle to a native JavaScript object.  It retains its identity when passed back and forth between JavaScript and Java code, but it cannot be inspected or manipulated from Java code.  In StockWatcher, we'll pass the JSON response as a JavaScriptObject to a Java method that will update the watch list.

Before we forget, let's go ahead and add the required `import` for JavaScriptObject now:

```
import com.google.gwt.core.client.JavaScriptObject;
```

Now then, here's the JSNI method we need to add to `StockWatcher.java`:

```
public native static void getJson(int requestId, String url, StockWatcher handler) /*-{
    var callback = "callback" + requestId;
    
    var script = document.createElement("script");
    script.setAttribute("src", url+callback);
    script.setAttribute("type", "text/javascript");

    window[callback] = function(jsonObj) {
      handler.@com.google.gwt.sample.stockwatcher.client.StockWatcher::handleJsonResponse(Lcom/google/gwt/core/client/JavaScriptObject;)(jsonObj);
      window[callback + "done"] = true;
    }
    
    // JSON download has 1-second timeout
    setTimeout(function() {
      if (!window[callback + "done"]) {
        handler.@com.google.gwt.sample.stockwatcher.client.StockWatcher::handleJsonResponse(Lcom/google/gwt/core/client/JavaScriptObject;)(null);
      } 

      // cleanup
      document.body.removeChild(script);
      delete window[callback];
      delete window[callback + "done"];
    }, 1000);
    
    document.body.appendChild(script);
}-*/;
```

It looks very similar to the raw JavaScript snippet you've already seen, except it adds a bit of error checking to handle an unresponsive server or network problem (basically, we use a timer function to check a flag to see if the JSON callback was ever called).  We're also now generating callback function names sequentially in case we have multiple pending requests.  In particular, notice the syntax we're using to call our `handleJsonResponse(JavaScriptObject)` method:

```
$PP_OFF
handler.@com.google.gwt.sample.stockwatcher.client.StockWatcher::handleJsonResponse(Lcom/google/gwt/core/client/JavaScriptObject;)(jsonObj);
```

It's long, but it should make sense if you break it down:

  * **[instance-expr.]** : Our own StockWatcher object instance
  * **class-name** : The fully-qualified name of the StockWatcher class
  * **method-name** : The name of the method we're calling: `handleJsonResponse`
  * **param-signature** : The `handleJsonResponse` method signature, defined with the [JNI syntax](http://java.sun.com/j2se/1.5.0/docs/guide/jni/spec/types.html#wp16432)
  * **arguments** : The `jsonObj` containing the JSON data

You can see that once the JSON object is downloaded, the callback in our JSNI method is really just delegating to the Java method `handleJsonResponse(JavaScriptObject)`.  Here's what it does:

```
public void handleJsonResponse(JavaScriptObject jso) {
  if (jso == null) {
    displayError("Couldn't retrieve JSON");
    return;
  }
  
  updateTable(new JSONArray(jso));    
}
```

Were you expecting something more?  As it turns out, once we have the JavaScriptObject containing the JSON response there's really not much left to do.  We first check that we actually got a response back, and if not we display an error message.  If all went well, we transform the JavaScriptObject directly into a [JSONArray](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONArray.html) that can be forwarded on to our existing `updateTable(JSONArray)` method.  Since we're receiving the JSON data in object form and not as a `String`, we no longer need [JSONParser](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONParser.html).  Instead, we can create a [JSONValue](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONValue.html) simply by passing a [JavaScriptObject](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/JavaScriptObject.html) to the [JSONArray(JavaScriptObject)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONArray.html#JSONArray(com.google.gwt.core.client.JavaScriptObject)) or [JSONObject(JavaScriptObject)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONObject.html#JSONObject(com.google.gwt.core.client.JavaScriptObject)) constructor.

And now, the final piece of the puzzle: we need to modify `refreshWatchList()` to call our JSNI method instead of explictly downloading the JSON.  Here's the new version:

```
private void refreshWatchList() {
  if (stocks.size() == 0) {
    return;
  }   
  
  // add watch list stock symbols to URL
  String url = JSON_URL;
  
  Iterator<String> iter = stocks.iterator();
  while (iter.hasNext()) {
      url += iter.next();
      if (iter.hasNext()) {
          url += "+";
      }
  }
  
  url = URL.encode(url) + "&callback=";
  getJson(jsonRequestId++, url, this);
}
```

All of the [RequestBuilder](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html) code we used before has now been replaced with a call to `getJson(int, String, StockWatcher)`.  The first parameter is an ID number that uniquely identifies each HTTP request.  We'll need a place to store that, so add the following instance field to the StockWatcher class:

```
private int jsonRequestId = 0;
```

### Testing the change ###

Well, that should be everything.  Go ahead and compile to web mode again and launch StockWatcher (make sure the Python server is running first).

![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherJson.png](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/StockWatcherJson.png)

StockWatcher is back in action!  Now, our stock price data is coming from a _different server_ than the one hosting StockWatcher.  This ability to communicate across websites opens up a whole world of new possibilities for your GWT applications.

## Security ##

Before you run off and implement mashups of your own, remember that downloading cross-site JSON is powerful, but can also be a security risk.  Make sure the servers you interact with are _absolutely trustworthy_, since they will have the ability to execute arbitrary JavaScript code within your application.  Take a minute to read the [Security for GWT Applications](http://groups.google.com/group/Google-Web-Toolkit/web/security-for-gwt-applications) article, which describes the potential threats to GWT applications and how you can combat them.