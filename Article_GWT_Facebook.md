# Put Your GWT App on Facebook #

_Jared Jacobs is the frontend lead at [kaChing](http://www.kaching.com), a marketplace for investing talent. He had a chance to stop by and give us the lowdown on his team's successful port of their GWT application onto the Facebook platform. If you're looking to socialize your application on Facebook, read on for the steps to make it happen._

As you may know, social networks like Facebook, LinkedIn, and MySpace can be an excellent place to grow a new business. Most of the kaChing community first discovered us through friends using our [Fantasy Stock Exchange](http://www.facebook.com/apps/application.php?id=2339204748) Facebook app.

In this article, I'll help you get your GWT app running as a Facebook app. Believe it or not, it can be done in just two easy steps.

<ol><li>Create the Facebook App.<br>
Follow the instructions on Facebook's <a href='http://developers.facebook.com/get_started.php'>Getting Started</a> page to create and name your Facebook app. Accept the default settings for now.</li>

<li>Point your Facebook App at your GWT App.</li></ol>
> Adjust these Facebook App settings:
> <table border='1'><tr><td>Callback URL</td><td>Enter the URL of your GWT app's main HTML page. Tip: For a quick development cycle, run your server locally and use a <i>localhost</i> URL.</td></tr><tr><td>Canvas Page URL</td><td>Choose a path beneath apps.facebook.com for your Facebook app. Also select the <b>Use iframe</b> radio button.</td></tr></table>


And you're done! Visit the Canvas Page URL that you chose, and you'll see your GWT app running in Facebook. The rest of this post will suggest a couple of ways to integrate more fully, to better leverage the Facebook platform.

## Getting rid of unwanted scroll bars ##

If your GWT app is too wide for the containing Facebook iframe and you can't stomach a horizontal scrollbar, then you need to make your app slimmer - at least when it's running inside Facebook. Facebook's width limit (currently 760px) is a hard limit.

If your GWT app is too tall, it'll be clipped and you'll see a vertical scrollbar along the right side. To fix this, you can specify a large fixed canvas height for your app using the `CanvasUtil` feature of Facebook's [JavaScript Client Library](http://wiki.developers.facebook.com/index.php/JavaScript_Client_Library). You can read the docs for more detail, but in practice it boils down to adding the following snippet to the body of your app's main HTML page:
```
<script type="text/javascript" 
  src="http://static.ak.facebook.com/js/api_lib/v0.4/FeatureLoader.js.php">
</script>
<div id='FB_HiddenContainer' 
  style='display:none;position:absolute;left:-100px;top:-100px;width:0;height:0'>
</div>
<script type="text/javascript">
  window.onload = function(){
    FB_RequireFeatures(['CanvasUtil'], function(){
      FB.XdComm.Server.init('some/path/to/xd_receiver.html');
      FB.CanvasClient.setCanvasHeight('2000px');
    });
  };
</script>
```

You should customize the arguments to `init()` and `setCanvasHeight()` based on your needs. Here's a description of what's going on in the snippet above:

<ol><li>The first script tag loads <code>FB_RequireFeatures</code>, the entry point to the Facebook JS Client Library.</li>
<li>The <code>FB_RequireFeatures</code> call loads the <code>CanvasUtil</code> feature (the <code>FB.CanvasClient</code> object).</li>
<li>Before using <code>FB.CanvasClient</code>, we must set up a <a href='http://wiki.developers.facebook.com/index.php/Cross_Domain_Communication_Channel'>Cross Domain Communication Channel</a> between your app's canvas window and the containing Facebook window. This means a) hosting Facebook's <code>xd_receiver.html</code> file somewhere on your server, b) telling the Facebook JS Client Library where to find it (hence the <code>FB.XdComm.Server.init</code> call), and c) adding an <code>FB_HiddenContainer</code> div to the body of your main HTML page to serve as the container for channel iframes.</li></ol>

If your app changes its height from time to time, you can ask the Facebook's JS library to check your canvas window's height at regular intervals and adjust the containing iframe's height to match. To do so, replace the `FB.CanvasClient.setCanvasHeight` call in the snippet above with:
```
FB.CanvasClient.setCanvasHeight('2000px',
function(){FB.CanvasClient.startTimerToSizeToContent()});
```

For more info on the `CanvasUtil` feature, including how to make the containing Facebook window scroll to a desired location, see [this demo iframe app](http://apps.facebook.com/wzhu_public_resize/).

## Accessing Facebook User Data ##

If a Facebook user is logged into Facebook when accessing your Facebook app, the app URL will include the user's Facebook ID in the query string as the `fb_sig_user` or `fb_sig_canvas_user` parameter, depending on whether the user has authorized your application. (See [Authorizing Applications](http://wiki.developers.facebook.com/index.php/Authorizing_Applications) for more on the various request parameters.) You can use this Facebook ID, in conjunction with your app's ID and secret key, to request information about the user from Facebook using any of Facebook's various APIs. As a GWT app author, you are likely to be using a Java server, so consider using one of [these Java Facebook clients](http://wiki.developers.facebook.com/index.php/Java#Alternative_Java_clients). We use [this open-source Facebook Java API](http://code.google.com/p/facebook-java-api/).

For a high-level overview of Facebook's various APIs and their relative merits, I'd recommend a [useful blog post](http://www.ccheever.com/blog/?p=10) by Facebook Platform engineer Charlie Cheever.

## Get some Face time for your app ##

I hope this article helps you get your snazzy, snappy GWT apps to larger audiences. If you can afford the time investment, be sure to utilize social network integration points that can spur viral growth, such as invitations, profile boxes, and activity streams.