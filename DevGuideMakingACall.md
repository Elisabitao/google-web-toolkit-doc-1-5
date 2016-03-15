# Actually Making a Call #

The process of making an RPC from the client always involves the same steps:

  1. Instantiate the service interface using [GWT.create()](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/GWT.html#create(java.lang.Class)).
  1. Create an asynchronous callback object to be notified when the RPC has completed.
  1. Make the call.

## Example ##

Suppose you want to call a method on a service interface defined as follows:

```
// The RemoteServiceRelativePath annotation automatically calls setServiceEntryPoint()
@RemoteServiceRelativePath("email")
public interface MyEmailService extends RemoteService {
  void emptyMyInbox(String username, String password);
}
```

Its corresponding asynchronous interface will look like this:

```
public interface MyEmailServiceAsync {
  void emptyMyInbox(String username, String password,
      AsyncCallback<Void> callback);
}
```

The client-side call will look like this:

```
public void menuCommandEmptyInbox() {
  // (1) Create the client proxy. Note that although you are creating the
  // service interface proper, you cast the result to the asynchronous
  // version of the interface. The cast is always safe because the 
  // generated proxy implements the asynchronous interface automatically.
  //
  MyEmailServiceAsync emailService = (MyEmailServiceAsync) GWT.create(MyEmailService.class);

  // (2) Create an asynchronous callback to handle the result.
  //
  AsyncCallback callback = new AsyncCallback() {
    public void onSuccess(Void result) {
      // do some UI stuff to show success
    }

    public void onFailure(Throwable caught) {
      // do some UI stuff to show failure
    }
  };

  // (3) Make the call. Control flow will continue immediately and later
  // 'callback' will be invoked when the RPC completes.
  //
  emailService.emptyMyInbox(fUsername, fPassword, callback);
}
```

It is safe to cache the instantiated service proxy to avoid creating it for subsequent calls.  For example, you can instantiate the service proxy in the module's `onModuleLoad()` method and save the resulting instance as a class member.

```
    public class Foo implements EntryPoint {
      private MyEmailServiceAsync myEmailService = (MyEmailServiceAsync) GWT.create(MyEmailService.class);

      public void onModuleLoad() {
        // ... other initialization
      }

      /**
       * Make a GWT-RPC call to the server.  The myEmailService class member 
       * was initalized when the module started up.
       */
      void sendEmail (String message) {
          myEmailService.sendEmail(message, new AsyncCallback<String>() {

            public void onFailure(Throwable caught) {
              Window.alert("RPC to sendEmail() failed.");
            }

            public void onSuccess(String result) {
              label.setText(result);
            }
          });
      }
    }
```

### See Also ###

  * [RemoteService](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/RemoteService.html)

  * [ServiceDefTarget](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/user/client/rpc/ServiceDefTarget.html)

  * [GWT.create(Class)](http://google-web-toolkit.googlecode.com/svn/javadoc/1.5/com/google/gwt/core/client/GWT.html#create(java.lang.Class))