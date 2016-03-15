# Serializable Types #

GWT supports the concept of _serialization_, which means allowing the contents of a data object to be moved out of one piece of running code and either transmitted to another application or stored outside the appliation for later use.  GWT RPC method parameters and return types must be transmitted across a network between client and server applications and therefore they must be _serializable_.

Serializable types must conform to certain restrictions.  GWT tries really hard to make serialization as painless as possible.  While the rules regarding serialization are subtle, in practice the behavior becomes intuitive very quickly.

> _Tip: Although the terminology is very similar, GWT's concept of "serializable" is different than serialization based on the standard Java interface [Serializable](http://java.sun.com/j2se/1.5.0/docs/api/java/io/Serializable.html). All references to serialization are referring to the GWT concept.  For more information see the FAQ topic, "[Does the GWT RPC system support the use of java.io.Serializable?](FAQ_RPCSerializationSupport.md)"
>_

A type is serializable and can be used in a service interface if one of the following is true:

  * The type is primitive, such as `char`, `byte`, `short`, `int`, `long`, `boolean`, `float`, or `double`.
  * The type an instance of the `String`, `Date`, or a primitive wrapper such as `Character`, `Byte`, `Short`, `Integer`, `Long`, `Boolean`, `Float`, or `Double`.
  * The type is an enumeration.  Enumeration constants are serialized as a name only; none of the field values are serialized.
  * The type is an array of serializable types (including other serializable arrays).
  * The type is a serializable user-defined class.
  * The type has at least one serializable subclass.


> _The Object class is NOT serializable, therefore you cannot expect that a collection of Object types will be serialized across the wire. However, you can pull in all serializable subtypes of Object into a collection by omitting the @gwt.typeArgs in GWT 1.4 and prior.  As of GWT 1.5, most use cases can utilize Java generics to replace the use of Object instances._

## Serializable User-defined Classes ##

A user-defined class is serializable if all of the following apply:

  1. It is assignable to [IsSerializable](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/IsSerializable.html) or [Serializable](http://java.sun.com/j2se/1.5.0/docs/api/java/io/Serializable.html), either because it directly implements one of these interfaces or because it derives from a superclass that does
  1. All non-`final`, non-`transient` instance fields are themselves serializable, and
  1. Prior to GWT 1.5, it must have a _public_ default (zero argument) constructor or no constructor at all.
  1. As of GWT 1.5, it must have a default (zero argument) constructor (with any access modifier) or no constructor at all.

The `transient` keyword is honored, so values in transient fields are not exchanged during RPCs. Fields that are declared `final` are also not exchanged during RPCs, so they should generally be marked `transient` as well.

## Polymorphism ##

GWT RPC supports polymorphic parameters and return types. To make the best use of polymorphism, however, you should still try to be as specific as your design allows when defining service interfaces. Increased specificity allows the [compiler](DevGuideJavaToJavaScriptCompiler.md) to do a better job of removing unnecessary code when it optimizes your application for size reduction.

## Type Arguments ##

Collection classes such as `java.util.Set` and `java.util.List` are tricky because they operate in terms of `Object` instances. To make collections serializable, you should specify the particular type of objects they are expected to contain. In GWT 1.4 and prior, you would specify this type information using a special `@gwt.typeArgs` Javadoc annotation. In GWT 1.5, `@gwt.typeArgs` annotations have been deprecated in favor of parameterized types.  If you do not use gwt.typeArgs or particular parameterized type information on raw collections or maps you will get bloated code. Adding an object to a collection that violates its asserted item type using `@gwt.typeArgs` annotations will lead to undefined behavior. Adding a violating object to a parameterized collection in Java 1.5 will, of course, lead to compile-time errors.

When using `Collection` classes, with RPC service interfaces, you must annotate with  `@gwt.typeArgs` as follows:
  1. For RPC service interfaces, you must annotate the method parameters.
  1. For data objects, you must annotate the fields.

The following example demonstrates how to annotate fields of collection type in a serializable user-defined class:

```
public class MyClass implements IsSerializable {
  /**
   * This field is a Set that must always contain Strings.
   * 
   * @gwt.typeArgs <java.lang.String>
   */
  public Set setOfStrings;

  /**
   * This field is a Map that must always contain Strings as its keys and
   * values.
   * 
   * @gwt.typeArgs <java.lang.String,java.lang.String>
   */
  public Map mapOfStringToString;

  /**
   * Default Constructor. The Default Constructor's explicit declaration
   * is required for a serializable class.
   */
  public MyClass() {
  }
}
```

Note that there is no need to specify the name of the field in the `@gwt.typeArgs` declaration since it can be inferred.


Similarly, to annotate parameters and return types add the following interface:

```
public interface MyService extends RemoteService {
  /**
   * The first annotation indicates that the parameter named 'c' is a List
   * that will only contain Integer objects. The second annotation
   * indicates that the returned List will only contain String objects
   * (notice there is no need for a name, since it is a return value).
   * 
   * @gwt.typeArgs c <java.lang.Integer>
   * @gwt.typeArgs <java.lang.String>
   */
  List reverseListAndConvertToStrings(List c);
}
```

Note that parameter annotations must include the name of the parameter they are annotating in addition to the collection item type, while return type annotations do not.