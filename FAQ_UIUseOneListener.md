# As the application grows, event listeners seem to fire more slowly #

If you are creating an interface with many widgets that need to respond to events, such as `click` events, you may find that the application handles events more and more slowly.  One example of this might be having a list of widgets in a table.

One contributor to poor performance can be the overhead of managing many click listener instances.  Consider a common way of handling click events:

```
  Button newButton = new Button(titleText);
  newButton.addClickListener(new ClickListener() {
    public void onClick(Widget sender) {
      dialogBox.hide();
    }
  });
```

Each widget has its own click listener instance created by adding an anonymous inner class when the widget is created.  This construct is convenient and causes no performance issues for most cases, but when creating many widgets that need event handling, consider creating a single click listener and share it between multiple widgets:

```

 class MyApp implements EntryPoint, ClickListener {

   /* Be careful with saving UI components in datastructures like this:
    * if you remove a button from the app, make sure you also remove 
    * its reference from buttonMap HashMap to avoid memory leaks.
    */
   Map buttonMap<Button, Integer> = new HashMap<Button,Integer>();
 
   public void onModuleLoad() {
     FlowPanel panel = new FlowPanel();
     for (int i = 1; i < 100; ++i) {
        Button newButton = new Button("Option " + i);
        newButton.addClickListener(this);
        panel.add(newButton);
        buttonmap.add(newButton, Integer.valueOf(i));
     }
     RootPanel.get().add(panel);
   }

   // The shared ClickListener code.
   public void onClick(Widget sender) {
     if (sender instanceof Button) {
       Button b = (Button) sender;
       Integer context = buttonMap.get(b);
       if (context != null) {
         // ... Handle the button click for this button.
       }
     }
   }
 }
```
