<a href='Hidden comment: 
2008-12-01. Replaced by FAQ_JSONJSOIntroGWTJSONObject.wiki which appended technique for GWT 1.4 and later.
'></a>
# How can I coerce a JSON JavaScriptObject into a GWT JSONObject? #
At the moment, the only way to do this is to convert the JavaScriptObject back into text representation, and then invoke JSONParser.parse(). You can do that by incorporating a JSON encoder/stringifier, such as the open source implementation available at http://www.json.org/json.js. Simply write a JSNI method to invoke toJSONString() on the JavaScriptObject, and then pass it to JSONParser.parse().
