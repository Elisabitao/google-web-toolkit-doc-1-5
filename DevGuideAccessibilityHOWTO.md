# Adding Accessibility Support to GWT Widgets: A Guide for Widget Developers #

This document describes how to add accessibility support to your GWT application and widgets. A variety of different techniques are discussed; there is not yet a standard and generally-applicable approach for making AJAX applications accessible, but we provide some suggestions.

## Class com.google.gwt.user.client.ui.Accessibility ##

The Accessibility class implements the needed accessor and modifier methods for manipulating DOM attributes defined by the ARIA specifications. It also defines constants for the ARIA role and state names which are used by GWT widgets.

## Adding ARIA Roles ##

The ARIA attribute `role` is an indication of the widget type; it describes the way the widget should behave. Roles are static and should not change during the lifetime of a widget. Widget authors should:

  * Pick the right role for the widget from the list of supported ARIA roles.
  * Set this attribute at construction time.

The following is an example taken from the CustomButton widget. Adding the `button` role indicates to an assistive technology that the widget will behave like a button.

```
  protected CustomButton() {
    ...
    // Add a11y role "button". Accessibility.ROLE_BUTTON is the String constant
    // "button"
    Accessibility.setRole(getElement(), "role", Accessibility.ROLE_BUTTON);
    ...
  }
```

Note that some of the role names have already been defined as constants in the Accessibility class.

## Adding ARIA States ##

An ARIA state is an additional attribute that reflects the current state of a widget, for example, whether a checkbox is checked or unchecked. A state should be initialized at the time a widget is constructed and updated during user interaction.

Note that:
  * A widget can have numerous state attributes, whereas a widget can only ever have one role attribute.
  * State attributes are dynamic and change during the widget's lifetime. A role once set does not change.

Also, some of the relevant state names are defined as constants in the Accessibility class.

### Initializing States ###

Once a specific ARIA role has been associated with a widget, it is important to check which states are associated with that role. For example, the `button` role has two state attributes:

`aria-disabled`
> Indicates that the widget is present, but is not allowed for user actions.
`aria-pressed`
> Used for buttons that are toggleable to indicate their current pressed state.

The `aria-pressed` state is not used in the CustomButton widget, as it is not supported by most screen readers yet. However, the code in the example below is the code that we would write when the `aria-pressed` state is better supported.

In the CustomButton widget, the `aria-pressed` ARIA state is initialized as follows:

```
  protected CustomButton() {
    ...
    // Add a11y state "aria-pressed"
    Accessibility.setState(getElement(), "aria-pressed", "false");
  }
```

### Updating States During User Interaction ###

The CustomButton widget has support for multiple button faces, giving developers more stylistic control. Also, unlike the Button widget, a CustomButton can be toggleable, as is the case with the CustomButton subclass ToggleButton. Event handlers attached to the underlying DOM element update the button faces when the button is pressed. At the same time, the Accessibility.setState(Element, String, String) method should be used to manipulate the DOM and update the value of the ARIA state aria-pressed:

```
  void toggleDown() {
    ...
    // Update a11y state "aria-pressed"
    Accessibility.setState(getElement(), "aria-pressed", isDown() ? "true" : "false");
  }
```

It is important to make sure that all event handlers that change the state of the widget also change the ARIA state.

## Adding Keyboard Accessibility ##

Keyboard accessibility is a key requirement when access-enabling GWT widgets. When developing a new widget, ensure that it is keyboard accessible from the outset; adding keyboard accessibility later can be difficult. Screenreaders will speak the element that has keyboard focus, so keyboard accessibility can be accomplished by moving focus to different elements in response to keyboard commands.


Proper keyboard accessibility affords the following end-user behavior:
  * Users can tab to move focus to the widget.
  * When the widget receives focus, the screenreader will interpret the ARIA roles and states that are set on the widget.
  * The screenreader will speak a description of the widget, and its textual content.

By default, the only elements in the HTML DOM that can receive keyboard focus are anchor and form fields. However, setting the DOM property `tabIndex` (note that this corresponds to HTML attribute `tabindex`) to 0 will place such elements in the default tab sequence and thereby enable them to receive keyboard focus. Setting `tabIndex = -1`, while removing the element from the tab sequence, still allows the element to receive keyboard focus programmatically.

In GWT, any widget that extends the FocusWidget abstract class will be keyboard focusable by default. The FocusWidget abstract class includes a setFocus(boolean) method that can be used to programmatically set the focus or remove focus on the widget. FocusWidget also includes a setTabIndex(int) method that allows the user to set the DOM property `tabIndex` for the widget.

Keep in mind that extending FocusWidget does not guarantee focusability for your widget. The base element of the the FocusWidget (passed to the superclass constructor) must be a naturally focusable HTML element. In the case where the new widget does not have an obviously focusable component, a focusable element can be created via the FocusPanel.impl.createFocusable() static method. Look at CustomButton's implementation for an example of this.

For widgets that don't extend the FocusWidget abstract class, ensuring keyboard accessibility can be more difficult. Different browsers set focus in different ways, and focus on arbitrary elements is not supported everywhere. You can use FocusPanel to enclose elements that need to receive keyboard focus; just be sure to test your widget on different browsers.

For an example of using the `tabIndex` property, see the MenuBar widget. The root menu is the only one that should be in the tab sequence; its sub-menus are not. To achieve this, the tab index is set to 0 in the MenuBar's constructor, and as new MenuBars are added as sub-menus, their tab indexes are reset to -1.

## Indicating Selection Changes to Screen Readers for Items That Are Not Naturally Focusable ##

Some widgets, such as GWT's Tree and MenuBar widgets, consist of a container with a set of items. The container has a naturally focusable DOM element, but the items themselves do not. The focusable element receives all keyboard input, and causes visual changes in the contained items to indicate a change in item selection. For example, GWT's Tree widget contains TreeItems; both of these elements are made up of divs. However, the Tree also has a naturally focusable hidden element which receives keyboard events. Whenever the user hits the arrow keys, this element handles the event and causes the appropriate TreeItem to be highlighted.

While this technique holds up for sighted users, it plays havoc with screen readers. Since the TreeItems themselves never get natural focus when selected, there is no way for the screen reader to know that the item selection has changed. One possible way to remedy this would be to have each TreeItem be naturally focusable. Unfortunately, TreeItems can contain more than just text -- they can contain other widgets, which themselves can be focusable. Here, delegating focus properly would be fairly complex -- each TreeItem would have to handle all of the key events for its child widget, and decide whether or not to delegate key events to its child (for user interaction with the child widget), or to handle the key events itself (for Tree navigation). Keep in mind that hooking up keyboard event listeners for each item would become unwieldy, as Trees may become very large. One can avoid doing this by relying on the natural event bubbling of key events, and having an element at the root of the Tree widget be responsible for receiving events.

Another way to remedy the situation is to use the ARIA `aria-activedescendant` state. This state is set on an element that is naturally focusable, and its value is the HTML id of the currently-selected item. Whenever the item changes, the `aria-activedescendant` value should be updated to the id of the newly-selected item. The screen reader will notice the change in the value and read the element corresponding to the id. Below is an example of how this technique is used on the GWT Tree and TreeItem widgets.

First, we set roles on the Tree's root element and its focusable element:

```
  // Called from Tree(...) constructor
  private void init(TreeImages images, boolean useLeafImages) {
    
    ...
    
    // Root element of Tree is a div
    setElement(DOM.createDiv());

    ...
    
    // Create naturally-focusable element
    focusable = FocusPanel.impl.createFocusable();

    ...
    
    // Hide element and append it to root div
    DOM.setIntStyleAttribute(focusable, "zIndex", -1);
    DOM.appendChild(getElement(), focusable);

    // Listen for key events on the root element
    sinkEvents(Event.MOUSEEVENTS | Event.ONCLICK | Event.KEYEVENTS);
    
    ...
    
    // Add a11y role "tree" to root element
    Accessibility.setRole(getElement(), Accessibility.ROLE_TREE);
    
    // Add a11y role "treeitem" to focusable element. This is necessary for some screen 
    // readers to interpret the aria-activedescendant state of this element.     
    Accessibility.setRole(focusable, Accessibility.ROLE_TREEITEM);       
 }
```

Whenever an item selection changes, the value of the aria-activedescendant state is set on the focusable element, and the ARIA states of the currently-selected item are set:

```
  // Called after a new item has been selected
  private void updateAriaAttributes() {

    // Get the element which contains the text (or widget) content within
    // the currently-selected TreeItem
    Element curSelectionContentElem = curSelection.getContentElem();

    ...
    
    // Set the 'aria-level' state. To do this, we need to compute the level of
    // the currently selected item.    
    Accessibility.setState(curSelectionContentElem, Accessibility.STATE_LEVEL,
        String.valueOf(curSelectionLevel + 1));

    // Set other ARIA states
    ...
    
    // Update the 'aria-activedescendant' state for the focusable element to
    // match the id of the currently selected item.
    
    Accessibility.setState(focusable, Accessibility.STATE_ACTIVEDESCENDANT,
        DOM.getElementAttribute(curSelectionContentElem, "id"));
  }
```

Though it is not shown in this code snippet, when TreeItems are created, they are constructed out of several divs, only one of which contains the content that we wish to be interpreted by the screen reader. This div is assigned a unique DOM id (which is generated using the DOM.createUniqueId() method), and a role of `treeitem`. These attributes are not set on the root TreeItem div because it contains a child image, which we do not want to be read.

### Caveats with this Approach ###

The obvious problem with this approach is that unique DOM ids need to be assigned to all of the possible items that could be selected. While this is easy enough to implement, it seems unwieldy to assign a DOM id to each item.

Also, there is a subtle problem with using the `aria-activedescendant` state. Originally, the intended use-case for this state was the implementation of a listbox with divs. Whenever the `aria-activedescendant` value of the parent div (which was the one with natural focus) would change, the screen reader would read out the text of the list item with the corresponding id, ignoring any roles or states set on the selected item. This is fine for widgets as simple as a listbox; the selected item has enough text for the user to understand what is selected. However, in the case of a Tree, the selected item's text may not be enough. For example, which level of the tree is the selected item on?

Some screen readers have started to do more than just read the text of items selected with `aria-activedescendant`, interpreting the item just as they would any other element that received keyboard focus. However, not all screen readers do this yet.

## Associating Meaningful Labels ##

A web page will often include human-readable descriptive elements (such as Label or HTML widgets) that explain the purpose of a particular widget. However, the association between a widget and its description may not be obvious to a browser or a screen reader. ARIA defines the `aria-labelledby` state which can be used to explicitly associate a widget with one or more descriptive elements.

In order to associate a label with a widget, ensure that descriptive elements all have a unique id. The assigned id can later be used with to set the `aria-labelledby` state of a widget to refer to the id values of any descriptive elements, thereby associating those descriptive elements with the widget.

## Automatically Speaking Highlighted Content ##

AJAX components often highlight an item of interest without moving keyboard focus to that item. This creates a good end-user experience when using components such as autocomplete widgets; the user can continue to type and obtain further refinements to the available set of choices. Because screenreaders traditionally attempt to speak the item that has keyboard focus, they will not read highlighted items. ARIA live regions help make widgets such as autocomplete boxes usable for visually impaired users.

### How It Works ###

The ARIA role `region` is used to declare areas that hold such live content, i.e., content that updates dynamically without having keyboard focus. The ARIA state `aria-live` on such regions specifies the priority of such updates; think of this as a politeness setting. Here is a code example that should provide the general idea of how to implement this technique for an auto-complete widget:

### Initialize Live Region ###

The ARIA role `region` is added when instantiating the relevant DOM nodes in the AutoCompleteWidget constructor:

```
  public AutoCompleteWidget() {
    ...
    // Create a hidden div where we store the current item text for a
    // screen reader to speak
    ariaElement = DOM.createDiv();
    DOM.setStyleAttribute(ariaElement, "display", "none");
    Accessibility.setRole(ariaElement, "role", "region");
    Accessibility.setState(ariaElement, "aria-live", "rude");
    DOM.appendChild(getElement(), ariaElement);
  }
```

Here, we have created a hidden div element that holds the content to be spoken. We've declared it to have `role = 'region'` and `live = 'rude'`; the latter setting specifies that updates to this content have the highest priority. Next, we set up the needed associations so that the set of suggestions returned as the user types into the AutoCompleteWidget's text box are put in hidden div:

```
    // This method is called via a keyboard event handler
    private void showSuggestions(Collection suggestions) {
      if (suggestions.size() > 0) {
    
        // Popupulate the visible suggestion pop-up widget with the new selections
        // and show them
        ....
    
        // Generate the hidden div content based on the suggestions
    
        String hiddenDivText = "Suggestions ";

        for (Suggestion curSuggestion : suggestions) {
          hiddenDivText += " " + curSuggestion.getDisplayString();
        }
    
        DOM.setInnerText(ariaElement, hiddenDivText);
      }
    }
```

### Problems with this Approach ###

With this technique, the developer has complete control over the text that is spoken by the screen reader. While this seems like a good thing for the developer, it's not great for users of screen readers. Taking this approach, developers of AutoComplete widgets may decide on different text that the screen reader should read. For example, another screen reader might prefix each suggestion with the "Suggestion x", where x is the index of the suggestion in the list. This leads to an inconsistent experience across applications. If both developers were able to make use of ARIA roles and states, then a more consistent experience would result, in accordance with the ARIA specification.

A more direct problem with this approach is internationalization. Most developers would realize that the list of suggestions needs to be translated into different languages; this list is directly displayed on the screen. However, the word 'Suggestions', which is the first word on the live region, could easily be missed, since it is never visually displayed to the user. These sorts of descriptive terms must also be translated. If ARIA roles and states could be used, then translation of the spoken terms associated with the roles and states would be the screenreader's job; developers would only need to be responsible for translating their content.

## General Advice For Widget Developers ##

First and foremost, use native HTML controls whenever possible. Native HTML controls are well understood by screen readers. They do not require ARIA roles and states, which has two main benefits:

  * ARIA is not yet supported by all major browsers. Screen reader and browser developers have already done the work to make HTML controls accessible.
  * Reimplementing native HTML controls using divs (for example) can cause poor performance. For example, suppose a developer were to re-implement a listbox using divs. One of the ARIA states that applies to the `listitem` role is `aria-posinset`. This value indicates the position of the item within its parent container, which corresponds to the item's index in the listbox. The problem is that every time an item is added or removed from the listbox, one has to iterate through all of the items in the list, adjusting their `aria-posinset` values. Though there are some optimizations that can be done, this is still much slower than native HTML select elements.

If native HTML controls cannot be used and a custom widget has to be built, keep in mind that it is much easier to develop an accessible widget from the beginning than to go back and add accessibility support to an existing widget. While adding ARIA roles and states is relatively easy, ensuring that the appropriate elements receive keyboard focus during user interaction can be more complicated.

Also, widgets that subclass other widgets should end up with the appropriate ARIA roles and states. Your superclass widget might already specify a certain ARIA role, and while the Accessibility.setRole(Element, String) method will overwrite a previous ARIA role in the same element, a complicated DOM configuration may result in different ARIA roles being placed in different elements.

Make sure to test that a new widget is accessible! There are three basic steps in the translation between the DOM and the screen reader:

  * DOM: since ARIA attributes are being added directly to the DOM, an easy way to check that the attributes are in the right location is to use a DOM inspector like Firebug
  * Events: it is important to make sure that Firefox raises the appropriate events in response to ARIA attributes, changes in focus, and changes in the widget itself. A Microsoft tool called the Accessible Event Watcher, or AccEvent, can allow you to check which events are being raised.
  * The screen reader: in the end, you can be most sure that your GWT widgets are accessible by using a screen reader. Some screen readers may not be listening for all of the events raised by Firefox, or they might expect the ARIA attributes to be added to the DOM in certain locations. The most widely used screen readers with some support for ARIA are JAWS and Window-Eyes. FireVox, a free text-to-speech add-on for Firefox, also includes support for ARIA.
