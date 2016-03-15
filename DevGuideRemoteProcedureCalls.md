# Remote Procedure Calls #

A fundamental difference between AJAX applications and traditional HTML web applications is that AJAX applications do not need to fetch new HTML pages  while they execute. Because AJAX pages actually run more like applications within the browser, there is no need to request new HTML from the server to make user interface updates. However, like all client/server applications, AJAX applications usually _do_ need to fetch data from the server as they execute. The mechanism for interacting with a server across a network is called making a remote procedure call (RPC), also sometimes referred to as a _server call_. GWT RPC makes it easy for the client and server to pass Java objects back and forth over HTTP.
When used properly, RPCs give you the opportunity to move all of your UI logic to the client, resulting in greatly improved performance, reduced bandwidth, reduced web server load, and a pleasantly fluid user experience.

The [server-side](DevGuideServerSide.md) code that gets invoked from the client is often referred to as a _service_, so the act of making a remote procedure call is sometimes referred to as invoking a service. To be clear, though, the term _service_ in this context is not the same as the more general "web service" concept. In particular, GWT services are not related to the Simple Object Access Protocol (SOAP).


## Specifics ##
[RPC Plumbing Diagram](DevGuidePlumbingDiagram.md)
Diagram of the RPC plumbing.

[Creating Services](DevGuideCreatingServices.md)
How to build a service interface from scratch.

[Implementing Services](DevGuideImplementingServices.md)
Implement your service interface as a servlet.

[Actually Making a Call](DevGuideMakingACall.md)
How to actually make a remote procedure call from the client

[Serializable Types](DevGuideSerializableTypes.md)
Using GWT's automatic serialization well.

[Handling Exceptions](DevGuideHandlingExceptions.md)
Handle exceptions due to failed calls or thrown from the server.

[Getting Used to Asynchronous Calls](DevGuideGettingUsedToAsyncCalls.md)
Asynchronous calls are tricky at first, but ultimately your users will thank you.

[Architectural Perspectives](DevGuideArchitecturalPerspectives.md)
Contrasting a couple of approaches to implementing services.

[Example Deployment with Tomcat](DevGuideRPCDeployment.md)
An example of how to deploy your finished application to a Web Engine.