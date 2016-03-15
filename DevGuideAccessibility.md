# Accessibility Support for GWT #

## Screen Readers ##
A screen reader is an assistive application that interprets what is displayed on the screen for a blind or visually impaired user. Screen readers can interact with the user in a variety of ways, including speaking out loud or even producing a braille output.

### Who uses screen readers? ###

Many people find it helpful to be able to interact with their computers in multiple ways. Though Google does not keep statistics on how many of our users are using screen readers, as the population ages, more people will require assistive technology. It is important to make that our applications are accessible for everybody.

### How do screen readers work? ###

Screen readers listen for a standard set of events that are raised by platform-specific APIs. For example, when an alert window pops up on your screen, Microsoft Windows would expose this event using the Microsoft Active Accessibility API, while a Linux machine would use the Linux Access Toolkit. A screen reader will then communicate this change to the user.

## GWT and Screen Readers ##
Ajax applications are often written in ways that screen readers have difficulty interpreting correctly. A GWT developer writing a tree widget, for example, might use a list element that has been altered to behave like a tree control. But a screen reader would present the control as a list - an incorrect description that renders the tree unusable. Screen readers will also treat HTML span or div elements as regular static text elements, regardless of the presence of JavaScript event handlers for user interaction; you can easily imagine how this causes problems.

## ARIA ##
ARIA is a specification for making rich Internet applications accessible via a standard set of DOM properties. It is currently [a work in progress at the W3C](http://www.w3.org/WAI/intro/aria). More information can be found at the [ARIA page at the Mozilla Developer Center](http://developer.mozilla.org/en/docs/Accessible_DHTML).

Adding accessibility support to GWT widgets involves adding the relevant properties to DOM elements that can be used by browsers to raise events during user interaction. Screen readers can react to these events to represent the function of GWT widgets. The DOM properties specified by ARIA are classified into Roles and States.

The ARIA property `role` is an indication of the widget type, and therefore describes the way the widget should behave. A role is static and does not change over the lifetime of widget. Some examples of ARIA roles: `tree`, `menubar`, `menuitem`, `tab`. The ARIA role of a widget is expressed as a DOM attribute named `role` with its value set to one of the ARIA role strings.

There are also ARIA properties that describe the state of a widget. For example, a checkbox widget could be in the states "checked" or "unchecked". A state is dynamic and should be updated during user interaction. Some examples of ARIA states: `aria-disabled`, `aria-pressed`, `aria-expanded`, `aria-haspopup`. Note that an ARIA state itself a DOM attribute -- for example, a toggle button widget that wants to express that it has been pressed will have an attribute `aria-pressed` set to true.

The role of a widget determines the set of states that it supports. A widget with the role of `list`, for example, would not expose the `aria-pressed` state.

Also, accessible widgets require keyboard support. Screenreaders will speak the element that has keyboard focus, so keyboard accessibility can be accomplished by moving focus to different elements in response to keyboard commands.

## GWT and ARIA ##
Once ARIA roles and states are added to the DOM of a GWT widget, browsers will raise the appropriate events to the screen reader. As ARIA is still a work in progress, browsers may not raise an event for every ARIA property, and screenreaders may not recognize all of the events being raised.

Many GWT widgets now have keyboard accessibility and ARIA properties. These include CustomButton, Tree, TreeItem, MenuBar, MenuItem, TabBar, and TabPanel. Also, all widgets that inherit from FocusWidget now have a tabindex by default, allowing for better keyboard navigation.

## Making your GWT application accessible ##
The next document, [Adding accessibility support to widgets](DevGuideAccessibilityHOWTO.md), explains how to make your application and widgets work with screen readers.