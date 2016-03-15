# How can I coerce a JSON JavaScriptObject into a GWT JSONObject? #
## GWT 1.4 and later ##
GWT 1.4 makes this process a lot easier by providing constructors for  [JSONObject](http://google-web-toolkit.googlecode.com/svn/javadoc/1.4/com/google/gwt/json/client/JSONObject.html) and [JSONArray](http://google-web-toolkit.googlecode.com/svn/javadoc/1.4/com/google/gwt/json/client/JSONArray.html) that take JavaScriptObject values as constructor arguments.

So if you have some JavaScriptObject `value` object that you want to coerce into a JSONObject, all you need to do is create the JSONObject by passing in the `value` as a constructor argument, as shown below.

```
JSONObject jsonValue = new JSONObject(value);
```

## Previous to GWT 1.4 ##
Previous to GWT 1.4, the only way to do this was to convert the JavaScriptObject back into text representation, and then invoke JSONParser.parse(). You would have to incorporate a JSON encoder/stringifier, such as the open source implementation available at http://www.json.org/json.js, and use it to convert the JavaScriptObject back into a text representation. From that point, you would write a JSNI method to invoke the toJSONString() method on the JavaScriptObject, and then pass it to JSONParser.parse().
