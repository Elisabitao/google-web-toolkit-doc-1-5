# Deferred Binding #

Deferred binding is a feature of the GWT compiler that works by generating many versions of code at compile time, only one of which needs to be loaded by a particular client during bootstrapping at runtime. Each version is generated on a per browser basis, along with any other axis that your application defines or uses. For example, if you were to internationalize your application using [GWT's Internationalization module](DevGuideInternationalization.md), the GWT compiler would generate various versions of your application per browser environment, such as "Firefox in English", "Firefox in French", "Internet Explorer in English", etc...  As a result, the deployed JavaScript code is compact and quicker to download than hand coded JavaScript, containing only the code and resources it needs for a particular browser environment.

## Topics ##

[Deferred Binding Benefits](DevGuideDeferredBindingConcepts.md)
Introduction to deferrred binding concepts and its benefits over traditional JavaScript cross-browser support.

[Defining Deferred Binding Rules](DevGuideDeferredBindingUsing.md)
Defining rules to create your own classes that use Deferred Binding

[Deferred Binding with Replacement](DevGuideDeferredBindingReplacement.md)
Using the Deferred Binding Replacement mechanism.

[Deferred Binding using Generators](DevGuideDeferredBindingGenerators.md)
Using Deferred Binding by defining Generator subclasses.
