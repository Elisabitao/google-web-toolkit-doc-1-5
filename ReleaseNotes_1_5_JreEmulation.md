# JRE emulation library enhancements #

### New JRE Classes ###
  * StringBuilder
  * java.sql.Date
  * java.sql.Time
  * java.sql.Timestamp
  * Enum
  * EnumSet
  * EnumMap
  * LinkedHashMap
  * LinkedList
  * PriorityQueue
  * TreeMap
  * TreeSet

### New methods on existing JRE classes ###
  * Object.getClass()
  * Class.getName()
  * Integer.bitCount()
  * Several additional methods on Class, String and Character

### GWT.getTypeName() is now deprecated ###
Use `Object.getClass().getName()` instead.
