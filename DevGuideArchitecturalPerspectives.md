# Architectural Perspectives #

There are various ways to approach services within your application architecture. Understand first of all that [GWT RPC services](DevGuideRemoteProcedureCalls.md) are not intended to replace J2EE servers, nor are they intended to provide a public web service (e.g. SOAP) layer for your application. GWT RPCs, fundamentally, are simply a method of "getting from the client to the server." In other words, you use RPCs to accomplish tasks that are part of your application but that cannot be done on the client computer.

Architecturally, you can make use of RPC two alternative ways. The difference is a matter of taste and of the architectural needs of your application.

## Simple Client/Server Deployment ##

The first and most straightforward way to think of service definitions is to treat them as your application's entire back end. From this perspective, [client-side code](DevGuideClientSide.md) is your "front end" and all service code that runs on the server is "back end." If you take this approach, your service implementations would tend to be more general-purpose APIs that are not tightly coupled to one specific application. Your service definitions would likely directly access databases through JDBC or Hibernate or even files in the server's file system. For many applications, this view is appropriate, and it can be very efficient because it reduces the number of tiers.

## Multi-Tier Deployment ##

In more complex, multi-tiered architectures, your GWT service definitions could simply be lightweight gateways that call through to back-end server environments such as J2EE servers. From this perspective, your services can be viewed as the "server half" of your application's user interface. Instead of being general-purpose, services are created for the specific needs of your user interface. Your services become the "front end" to the "back end" classes that are written by stitching together calls to a more general-purpose back-end layer of services, implemented, for example, as a cluster of J2EE servers. This kind of architecture is appropriate if you require your back-end services to run on a physically separate computer from your HTTP server.
