# Working with JSON #

Many AJAX application developers have adopted [JSON](http://www.json.org/) as the data format of choice for server communication.  It is a relatively simple format based on the object-literal notation of JavaScript.  If you choose to use JSON-encoded data within your application, GWT contains classes you can use to parse and manipulate JSON objects.

## JSON encoding ##

The JSON format is based on the syntax and data types of the JavaScript language.  It supports strings, numbers, booleans, and null values.  You can also combine multiple values into arrays and objects.  JSON objects are simply unordered sets of name/value pairs, where the name is always a string and the value is any other valid JSON type (even another object).  Here's an example of encoding product data in JSON:

```
{
  "product": {
    "name": "Widget",
    "company": "ACME, Inc",
    "partNumber": "7402-129",
    "prices": [
      { "minQty": 1, "price": 12.49 },
      { "minQty": 10, "price": 9.99 },
      { "minQty": 50, "price": 7.99 }
    ]
  }
}
```

See [json.org/example.html](http://www.json.org/example.html) for more JSON examples.

## Parsing JSON ##

GWT's JSON types are contained in a separate module, so you'll need to add the necessary `<inherits>` tag to your [module XML file](http://code.google.com/p/google-web-toolkit-doc-1-5/wiki/DevGuideModuleXml):

```
<inherits name="com.google.gwt.json.JSON" />
```

Typically, you will receive JSON data as the response text of an [HTTP request](DevGuideHttpRequests.md).  Thus, you'll first have to convert that `String` into a data structure you can navigate and manipulate.  GWT includes the [JSONParser](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONParser.html) class to handle that transformation.  Its lone static method [parse(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONParser.html#parse(java.lang.String)) will accept the JSON text and parses is into a set of [JSONValue](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONValue.html)-derived objects.

[JSONValue](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONValue.html) is the superclass of all JSON types.  Each of the basic JSON types is represented by a class in the [com.google.gwt.json.client](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/package-summary.html) package.  JSONValue contains a series of methods you can use to determine which specific subtype the value represents, and if possible perform the conversion.  For example, the [isBoolean()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONValue.html#isBoolean()) method will return a [JSONBoolean](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONBoolean.html) if the JSONValue is really a JSONBoolean, or `null` if it is not.

Composite JSON types contain specialized methods for accessing their members.  [JSONObject](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONObject.html) has [get(String)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONObject.html#get(java.lang.String)) and [put(String, JSONValue)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONObject.html#put(java.lang.String,%20com.google.gwt.json.client.JSONValue)) methods for getting and setting the object's properties.  [JSONArray](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONArray.html) has corresponding methods named [get(int)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONArray.html#get(int)) and [set(int, JSONValue)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/json/client/JSONArray.html#set(int,%20com.google.gwt.json.client.JSONValue)).

For a complete example of parsing JSON, see the [JSON article](GettingStartedJSON.md) in the [Getting Started guide](GettingStarted.md).

## Mashups with JSON and JSNI ##

If you're loading JSON-encoded data from your own server, you'll typically use the [RequestBuilder](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/http/client/RequestBuilder.html) and related classes to [make HTTP requests](DevGuideHttpRequests.md).  However, you can also retrieve JSON from remote servers in true mashup fashion using GWT's [JavaScript Native Interface (JSNI)](http://code.google.com/p/google-web-toolkit-doc-1-5/wiki/DevGuideJavaScriptNativeInterface) functionality.  The techniques for cross-site JSON is explained more fully in [this FAQ article](FAQ_JSONFeedsFromOtherDomain.md).  To see a working example, check out the [JSON/HTTP](GettingStartedJSON.md) chapter in the [Getting Started guide](GettingStarted.md).

