# RPC Plumbing Diagram #

This section outlines the moving parts required to invoke a service. Each service has a small family of helper interfaces and classes. Some of these classes, such as the service proxy, are automatically generated behind the scenes and you generally will never realize they exist. The pattern for helper classes is identical for every service that you implement, so it is a good idea to spend a few moments to familiarize yourself with the terminology and purpose of each layer in server call processing. If you are familiar with traditional remote procedure call (RPC) mechanisms, you will recognize most of this terminology already.
![http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/AnatomyOfServices.gif](http://google-web-toolkit-doc-1-5.googlecode.com/svn/wiki/AnatomyOfServices.gif)


