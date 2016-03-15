# JRE Emulation #
Google Web Toolkit includes a library that emulates a subset of the Java runtime library. The list below shows the set of JRE types and methods that GWT can translate automatically. Note that in some cases, only a subset of methods is supported for a given type.
## Package java.lang ##

[ArithmeticException](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/ArithmeticException.html)
> ArithmeticException(String), ArithmeticException()

[ArrayIndexOutOfBoundsException](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/ArrayIndexOutOfBoundsException.html)
> ArrayIndexOutOfBoundsException(), ArrayIndexOutOfBoundsException(int), ArrayIndexOutOfBoundsException(String)

[ArrayStoreException](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/ArrayStoreException.html)
> ArrayStoreException(), ArrayStoreException(String)

[AssertionError](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/AssertionError.html)
> AssertionError(), AssertionError(boolean), AssertionError(char), AssertionError(double), AssertionError(float), AssertionError(int), AssertionError(long), AssertionError(Object)

[Boolean](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Boolean.html)
> Boolean(boolean), Boolean(String), parseBoolean(String), toString(boolean), valueOf(boolean), valueOf(String), booleanValue(), compareTo(Boolean), equals(Object), hashCode(), toString()

[Byte](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Byte.html)
> Byte(byte), Byte(String), decode(String), hashCode(byte), parseByte(String), parseByte(String, int), toString(byte), valueOf(byte), valueOf(String), valueOf(String, int), byteValue(), compareTo(Byte), doubleValue(), equals(Object), floatValue(), hashCode(), intValue(), longValue(), shortValue(), toString()

[CharSequence](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/CharSequence.html)
> charAt(int), length(), subSequence(int, int), toString()

[Character](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Character.html)
> Character(char), charCount(int), codePointAt(char[.md](.md), int), codePointAt(char[.md](.md), int, int), codePointAt(CharSequence, int), codePointBefore(char[.md](.md), int), codePointBefore(char[.md](.md), int, int), codePointBefore(CharSequence, int), codePointCount(char[.md](.md), int, int), codePointCount(CharSequence, int, int), digit(char, int), forDigit(int, int), hashCode(char), isDigit(char), isHighSurrogate(char), isLetter(char), isLetterOrDigit(char), isLowerCase(char), isLowSurrogate(char), isSpace(char), isSupplementaryCodePoint(int), isSurrogatePair(char, char), isUpperCase(char), isValidCodePoint(int), offsetByCodePoints(char[.md](.md), int, int, int, int), offsetByCodePoints(CharSequence, int, int), toChars(int), toChars(int, char[.md](.md), int), toCodePoint(char, char), toLowerCase(char), toString(char), toUpperCase(char), valueOf(char), charValue(), compareTo(Character), equals(Object), hashCode(), toString()

[Class](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Class.html)
> desiredAssertionStatus(), getEnumConstants(), getName(), getSuperclass(), isArray(), isEnum(), isInterface(), isPrimitive(), toString()

[ClassCastException](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/ClassCastException.html)
> ClassCastException(), ClassCastException(String)

[Cloneable](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Cloneable.html)


[Comparable](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Comparable.html)
> compareTo(T)

[Deprecated](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Deprecated.html)


[Double](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Double.html)
> Double(double), Double(String), compare(double, double), hashCode(double), isInfinite(double), isNaN(double), parseDouble(String), toString(double), valueOf(double), valueOf(String), byteValue(), compareTo(Double), doubleValue(), equals(Object), floatValue(), hashCode(), intValue(), isInfinite(), isNaN(), longValue(), shortValue(), toString()

[Enum](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Enum.html)
> valueOf(Class, String), compareTo(E), equals(Object), getDeclaringClass(), hashCode(), name(), ordinal(), toString()

[Error](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Error.html)
> Error(), Error(String, Throwable), Error(String), Error(Throwable)

[Exception](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Exception.html)
> Exception(), Exception(String), Exception(String, Throwable), Exception(Throwable)

[Float](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Float.html)
> Float(double), Float(float), Float(String), compare(float, float), hashCode(float), isInfinite(float), isNaN(float), parseFloat(String), toString(float), valueOf(float), valueOf(String), byteValue(), compareTo(Float), doubleValue(), equals(Object), floatValue(), hashCode(), intValue(), isInfinite(), isNaN(), longValue(), shortValue(), toString()

[IllegalArgumentException](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/IllegalArgumentException.html)
> IllegalArgumentException(), IllegalArgumentException(String), IllegalArgumentException(String, Throwable), IllegalArgumentException(Throwable)

[IllegalStateException](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/IllegalStateException.html)
> IllegalStateException(), IllegalStateException(String), IllegalStateException(String, Throwable), IllegalStateException(Throwable)

[IndexOutOfBoundsException](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/IndexOutOfBoundsException.html)
> IndexOutOfBoundsException(), IndexOutOfBoundsException(String)

[Integer](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Integer.html)
> Integer(int), Integer(String), bitCount(int), decode(String), hashCode(int), highestOneBit(int), lowestOneBit(int), numberOfLeadingZeros(int), numberOfTrailingZeros(int), parseInt(String), parseInt(String, int), reverse(int), reverseBytes(int), rotateLeft(int, int), rotateRight(int, int), signum(int), toBinaryString(int), toHexString(int), toOctalString(int), toString(int), toString(int, int), valueOf(int), valueOf(String), valueOf(String, int), byteValue(), compareTo(Integer), doubleValue(), equals(Object), floatValue(), hashCode(), intValue(), longValue(), shortValue(), toString()

[Iterable](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Iterable.html)
> iterator()

[Long](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Long.html)
> Long(long), Long(String), bitCount(long), decode(String), hashCode(long), highestOneBit(long), lowestOneBit(long), numberOfLeadingZeros(long), numberOfTrailingZeros(long), parseLong(String), parseLong(String, int), reverse(long), reverseBytes(long), rotateLeft(long, int), rotateRight(long, int), signum(long), toBinaryString(long), toHexString(long), toOctalString(long), toString(long), toString(long, int), valueOf(long), valueOf(String), valueOf(String, int), byteValue(), compareTo(Long), doubleValue(), equals(Object), floatValue(), hashCode(), intValue(), longValue(), shortValue(), toString()

[Math](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Math.html)
> Math(), abs(double), abs(float), abs(int), abs(long), acos(double), asin(double), atan(double), atan2(double, double), ceil(double), copySign(double, double), copySign(float, float), cos(double), cosh(double), exp(double), expm1(double), floor(double), hypot(double, double), log(double), log10(double), log1p(double), max(double, double), max(float, float), max(int, int), max(long, long), min(double, double), min(float, float), min(int, int), min(long, long), pow(double, double), random(), rint(double), round(double), round(float), scalb(double, int), scalb(float, int), signum(double), signum(float), sin(double), sinh(double), sqrt(double), tan(double), tanh(double), toDegrees(double), toRadians(double)

[NegativeArraySizeException](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/NegativeArraySizeException.html)
> NegativeArraySizeException(), NegativeArraySizeException(String)

[NullPointerException](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/NullPointerException.html)
> NullPointerException(), NullPointerException(String)

[Number](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Number.html)
> Number(), byteValue(), doubleValue(), floatValue(), intValue(), longValue(), shortValue()

[NumberFormatException](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/NumberFormatException.html)
> NumberFormatException(), NumberFormatException(String)

[Object](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Object.html)
> Object(), equals(Object), getClass(), hashCode(), toString()

[Override](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Override.html)


[Runnable](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Runnable.html)
> run()

[RuntimeException](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/RuntimeException.html)
> RuntimeException(), RuntimeException(String), RuntimeException(String, Throwable), RuntimeException(Throwable)

[Short](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Short.html)
> Short(short), Short(String), decode(String), hashCode(short), parseShort(String), parseShort(String, int), reverseBytes(short), toString(short), valueOf(short), valueOf(String), valueOf(String, int), byteValue(), compareTo(Short), doubleValue(), equals(Object), floatValue(), hashCode(), intValue(), longValue(), shortValue(), toString()

[StackTraceElement](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/StackTraceElement.html)
> StackTraceElement(), getClassName(), getFileName(), getLineNumber(), getMethodName()

[String](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/String.html)
> String(), String(char[.md](.md)), String(char[.md](.md), int, int), String(int[.md](.md), int, int), String(String), String(StringBuffer), String(StringBuilder), copyValueOf(char[.md](.md)), copyValueOf(char[.md](.md), int, int), valueOf(boolean), valueOf(char), valueOf(char[.md](.md), int, int), valueOf(char[.md](.md)), valueOf(double), valueOf(float), valueOf(int), valueOf(long), valueOf(Object), charAt(int), codePointAt(int), codePointBefore(int), codePointCount(int, int), compareTo(String), compareToIgnoreCase(String), concat(String), contains(CharSequence), contentEquals(CharSequence), contentEquals(StringBuffer), endsWith(String), equals(Object), equalsIgnoreCase(String), getChars(int, int, char[.md](.md), int), hashCode(), indexOf(int), indexOf(int, int), indexOf(String), indexOf(String, int), intern(), isEmpty(), lastIndexOf(int), lastIndexOf(int, int), lastIndexOf(String), lastIndexOf(String, int), length(), matches(String), offsetByCodePoints(int, int), regionMatches(boolean, int, String, int, int), regionMatches(int, String, int, int), replace(char, char), replace(CharSequence, CharSequence), replaceAll(String, String), replaceFirst(String, String), split(String), split(String, int), startsWith(String), startsWith(String, int), subSequence(int, int), substring(int), substring(int, int), toCharArray(), toLowerCase(), toString(), toUpperCase(), trim()

[StringBuffer](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/StringBuffer.html)
> StringBuffer(), StringBuffer(CharSequence), StringBuffer(int), StringBuffer(String), append(boolean), append(char), append(char[.md](.md)), append(char[.md](.md), int, int), append(CharSequence), append(CharSequence, int, int), append(double), append(float), append(int), append(long), append(Object), append(String), append(StringBuffer), capacity(), charAt(int), delete(int, int), deleteCharAt(int), ensureCapacity(int), getChars(int, int, char[.md](.md), int), indexOf(String), indexOf(String, int), insert(int, boolean), insert(int, char), insert(int, char[.md](.md)), insert(int, char[.md](.md), int, int), insert(int, CharSequence), insert(int, CharSequence, int, int), insert(int, double), insert(int, float), insert(int, int), insert(int, long), insert(int, Object), insert(int, String), lastIndexOf(String), lastIndexOf(String, int), length(), replace(int, int, String), setCharAt(int, char), setLength(int), subSequence(int, int), substring(int), substring(int, int), toString(), trimToSize()

[StringBuilder](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/StringBuilder.html)
> StringBuilder(), StringBuilder(CharSequence), StringBuilder(int), StringBuilder(String), append(boolean), append(char), append(char[.md](.md)), append(char[.md](.md), int, int), append(CharSequence), append(CharSequence, int, int), append(double), append(float), append(int), append(long), append(Object), append(String), append(StringBuilder), capacity(), charAt(int), delete(int, int), deleteCharAt(int), ensureCapacity(int), getChars(int, int, char[.md](.md), int), indexOf(String), indexOf(String, int), insert(int, boolean), insert(int, char), insert(int, char[.md](.md)), insert(int, char[.md](.md), int, int), insert(int, CharSequence), insert(int, CharSequence, int, int), insert(int, double), insert(int, float), insert(int, int), insert(int, long), insert(int, Object), insert(int, String), lastIndexOf(String), lastIndexOf(String, int), length(), replace(int, int, String), setCharAt(int, char), setLength(int), subSequence(int, int), substring(int), substring(int, int), toString(), trimToSize()

[StringIndexOutOfBoundsException](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/StringIndexOutOfBoundsException.html)
> StringIndexOutOfBoundsException(), StringIndexOutOfBoundsException(String), StringIndexOutOfBoundsException(int)

[SuppressWarnings](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/SuppressWarnings.html)
> value()

[System](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/System.html)
> System(), arraycopy(Object, int, Object, int, int), currentTimeMillis(), gc(), identityHashCode(Object), setErr(PrintStream), setOut(PrintStream)

[Throwable](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Throwable.html)
> Throwable(), Throwable(String), Throwable(String, Throwable), Throwable(Throwable), fillInStackTrace(), getCause(), getLocalizedMessage(), getMessage(), getStackTrace(), initCause(Throwable), printStackTrace(), printStackTrace(PrintStream), setStackTrace(StackTraceElement[.md](.md)), toString()

[UnsupportedOperationException](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/UnsupportedOperationException.html)
> UnsupportedOperationException(), UnsupportedOperationException(String), UnsupportedOperationException(String, Throwable), UnsupportedOperationException(Throwable)

[Void](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Void.html)


## Package java.lang.annotation ##

[Annotation](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/annotation/Annotation.html)
> annotationType(), equals(Object), hashCode(), toString()

[AnnotationFormatError](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/annotation/AnnotationFormatError.html)
> AnnotationFormatError()

[AnnotationTypeMismatchException](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/annotation/AnnotationTypeMismatchException.html)
> AnnotationTypeMismatchException()

[Documented](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/annotation/Documented.html)


[ElementType](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/annotation/ElementType.html)
> values(), valueOf(String)

[IncompleteAnnotationException](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/annotation/IncompleteAnnotationException.html)
> IncompleteAnnotationException(Class, String), annotationType(), elementName()

[Inherited](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/annotation/Inherited.html)


[Retention](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/annotation/Retention.html)
> value()

[RetentionPolicy](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/annotation/RetentionPolicy.html)
> values(), valueOf(String)

[Target](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/annotation/Target.html)
> value()

## Package java.util ##

[AbstractCollection](http://java.sun.com/j2se/1.5.0/docs/api/java/util/AbstractCollection.html)
> add(E), addAll(Collection), clear(), contains(Object), containsAll(Collection), isEmpty(), iterator(), remove(Object), removeAll(Collection), retainAll(Collection), size(), toArray(), toArray(T[.md](.md)), toString()

[AbstractList](http://java.sun.com/j2se/1.5.0/docs/api/java/util/AbstractList.html)
> add(E), add(int, E), addAll(int, Collection), clear(), equals(Object), get(int), hashCode(), indexOf(Object), iterator(), lastIndexOf(Object), listIterator(), listIterator(int), remove(int), set(int, E)

[AbstractMap](http://java.sun.com/j2se/1.5.0/docs/api/java/util/AbstractMap.html)
> clear(), containsKey(Object), containsValue(Object), entrySet(), equals(Object), get(Object), hashCode(), isEmpty(), keySet(), put(K, V), putAll(Map), remove(Object), size(), toString(), values()

[AbstractQueue](http://java.sun.com/j2se/1.5.0/docs/api/java/util/AbstractQueue.html)
> add(E), addAll(Collection), clear(), element(), offer(E), peek(), poll(), remove()

[AbstractSequentialList](http://java.sun.com/j2se/1.5.0/docs/api/java/util/AbstractSequentialList.html)
> add(int, E), addAll(int, Collection), get(int), iterator(), listIterator(int), remove(int), set(int, E), size()

[AbstractSet](http://java.sun.com/j2se/1.5.0/docs/api/java/util/AbstractSet.html)
> AbstractSet(), equals(Object), hashCode(), removeAll(Collection)

[ArrayList](http://java.sun.com/j2se/1.5.0/docs/api/java/util/ArrayList.html)
> ArrayList(), ArrayList(Collection), ArrayList(int), add(E), add(int, E), addAll(Collection), addAll(int, Collection), clear(), clone(), contains(Object), ensureCapacity(int), get(int), indexOf(Object), isEmpty(), lastIndexOf(Object), remove(int), remove(Object), set(int, E), size(), toArray(), toArray(T[.md](.md)), trimToSize()

[Arrays](http://java.sun.com/j2se/1.5.0/docs/api/java/util/Arrays.html)
> Arrays(), asList(T[.md](.md)), binarySearch(byte[.md](.md), byte), binarySearch(char[.md](.md), char), binarySearch(double[.md](.md), double), binarySearch(float[.md](.md), float), binarySearch(int[.md](.md), int), binarySearch(long[.md](.md), long), binarySearch(Object[.md](.md), Object), binarySearch(short[.md](.md), short), binarySearch(T[.md](.md), T, Comparator), deepEquals(Object[.md](.md), Object[.md](.md)), deepHashCode(Object[.md](.md)), deepToString(Object[.md](.md)), equals(boolean[.md](.md), boolean[.md](.md)), equals(byte[.md](.md), byte[.md](.md)), equals(char[.md](.md), char[.md](.md)), equals(double[.md](.md), double[.md](.md)), equals(float[.md](.md), float[.md](.md)), equals(int[.md](.md), int[.md](.md)), equals(long[.md](.md), long[.md](.md)), equals(Object[.md](.md), Object[.md](.md)), equals(short[.md](.md), short[.md](.md)), fill(boolean[.md](.md), boolean), fill(boolean[.md](.md), int, int, boolean), fill(byte[.md](.md), byte), fill(byte[.md](.md), int, int, byte), fill(char[.md](.md), char), fill(char[.md](.md), int, int, char), fill(double[.md](.md), double), fill(double[.md](.md), int, int, double), fill(float[.md](.md), float), fill(float[.md](.md), int, int, float), fill(int[.md](.md), int), fill(int[.md](.md), int, int, int), fill(long[.md](.md), int, int, long), fill(long[.md](.md), long), fill(Object[.md](.md), int, int, Object), fill(Object[.md](.md), Object), fill(short[.md](.md), int, int, short), fill(short[.md](.md), short), hashCode(boolean[.md](.md)), hashCode(byte[.md](.md)), hashCode(char[.md](.md)), hashCode(double[.md](.md)), hashCode(float[.md](.md)), hashCode(int[.md](.md)), hashCode(long[.md](.md)), hashCode(Object[.md](.md)), hashCode(short[.md](.md)), sort(byte[.md](.md)), sort(byte[.md](.md), int, int), sort(char[.md](.md)), sort(char[.md](.md), int, int), sort(double[.md](.md)), sort(double[.md](.md), int, int), sort(float[.md](.md)), sort(float[.md](.md), int, int), sort(int[.md](.md)), sort(int[.md](.md), int, int), sort(long[.md](.md)), sort(long[.md](.md), int, int), sort(Object[.md](.md)), sort(Object[.md](.md), int, int), sort(short[.md](.md)), sort(short[.md](.md), int, int), sort(T[.md](.md), Comparator), sort(T[.md](.md), int, int, Comparator), toString(boolean[.md](.md)), toString(byte[.md](.md)), toString(char[.md](.md)), toString(double[.md](.md)), toString(float[.md](.md)), toString(int[.md](.md)), toString(long[.md](.md)), toString(Object[.md](.md)), toString(short[.md](.md))

[Collection](http://java.sun.com/j2se/1.5.0/docs/api/java/util/Collection.html)
> add(E), addAll(Collection), clear(), contains(Object), containsAll(Collection), equals(Object), hashCode(), isEmpty(), iterator(), remove(Object), removeAll(Collection), retainAll(Collection), size(), toArray(), toArray(T[.md](.md))

[Collections](http://java.sun.com/j2se/1.5.0/docs/api/java/util/Collections.html)
> Collections(), addAll(Collection, T[.md](.md)), binarySearch(List, T), binarySearch(List, T, Comparator), copy(List, List), disjoint(Collection, Collection), emptyList(), emptyMap(), emptySet(), enumeration(Collection), fill(List, T), frequency(Collection, Object), list(Enumeration), max(Collection), max(Collection, Comparator), min(Collection), min(Collection, Comparator), nCopies(int, T), replaceAll(List, T, T), reverse(List), reverseOrder(), reverseOrder(Comparator), singleton(T), singletonList(T), singletonMap(K, V), sort(List), sort(List, Comparator), swap(List, int, int), unmodifiableCollection(Collection), unmodifiableList(List), unmodifiableMap(Map), unmodifiableSet(Set), unmodifiableSortedMap(SortedMap), unmodifiableSortedSet(SortedSet)

[Comparator](http://java.sun.com/j2se/1.5.0/docs/api/java/util/Comparator.html)
> compare(T, T), equals(Object)

[ConcurrentModificationException](http://java.sun.com/j2se/1.5.0/docs/api/java/util/ConcurrentModificationException.html)
> ConcurrentModificationException(), ConcurrentModificationException(String)

[Date](http://java.sun.com/j2se/1.5.0/docs/api/java/util/Date.html)
> Date(), Date(int, int, int), Date(int, int, int, int, int), Date(int, int, int, int, int, int), Date(long), Date(String), parse(String), !UTC(int, int, int, int, int, int), after(Date), before(Date), clone(), compareTo(Date), equals(Object), getDate(), getDay(), getHours(), getMinutes(), getMonth(), getSeconds(), getTime(), getTimezoneOffset(), getYear(), hashCode(), setDate(int), setHours(int), setMinutes(int), setMonth(int), setSeconds(int), setTime(long), setYear(int), toGMTString(), toLocaleString(), toString()

[EmptyStackException](http://java.sun.com/j2se/1.5.0/docs/api/java/util/EmptyStackException.html)
> EmptyStackException()

[EnumMap](http://java.sun.com/j2se/1.5.0/docs/api/java/util/EnumMap.html)
> EnumMap(Class), EnumMap(EnumMap), EnumMap(Map), clear(), clone(), containsKey(Object), containsValue(Object), entrySet(), get(Object), put(K, V), remove(Object), size()

[EnumSet](http://java.sun.com/j2se/1.5.0/docs/api/java/util/EnumSet.html)
> allOf(Class), complementOf(EnumSet), copyOf(Collection), copyOf(EnumSet), noneOf(Class), of(E), of(E, E[.md](.md)), range(E, E), clone()

[Enumeration](http://java.sun.com/j2se/1.5.0/docs/api/java/util/Enumeration.html)
> hasMoreElements(), nextElement()

[EventListener](http://java.sun.com/j2se/1.5.0/docs/api/java/util/EventListener.html)


[EventObject](http://java.sun.com/j2se/1.5.0/docs/api/java/util/EventObject.html)
> EventObject(Object), getSource()

[HashMap](http://java.sun.com/j2se/1.5.0/docs/api/java/util/HashMap.html)
> HashMap(), HashMap(int), HashMap(int, float), HashMap(Map), clone()

[HashSet](http://java.sun.com/j2se/1.5.0/docs/api/java/util/HashSet.html)
> HashSet(), HashSet(Collection), HashSet(int), HashSet(int, float), add(E), clear(), clone(), contains(Object), isEmpty(), iterator(), remove(Object), size(), toString()

[IdentityHashMap](http://java.sun.com/j2se/1.5.0/docs/api/java/util/IdentityHashMap.html)
> IdentityHashMap(), IdentityHashMap(int), IdentityHashMap(Map), clone(), equals(Object), hashCode()

[Iterator](http://java.sun.com/j2se/1.5.0/docs/api/java/util/Iterator.html)
> hasNext(), next(), remove()

[LinkedHashMap](http://java.sun.com/j2se/1.5.0/docs/api/java/util/LinkedHashMap.html)
> LinkedHashMap(), LinkedHashMap(int), LinkedHashMap(int, float), LinkedHashMap(int, float, boolean), LinkedHashMap(Map), clear(), containsKey(Object), containsValue(Object), entrySet(), get(Object), put(K, V), remove(Object), size()

[LinkedHashSet](http://java.sun.com/j2se/1.5.0/docs/api/java/util/LinkedHashSet.html)
> LinkedHashSet(), LinkedHashSet(Collection), LinkedHashSet(int), LinkedHashSet(int, float)

[LinkedList](http://java.sun.com/j2se/1.5.0/docs/api/java/util/LinkedList.html)
> LinkedList(), LinkedList(Collection), add(E), addFirst(E), addLast(E), clear(), element(), getFirst(), getLast(), listIterator(int), offer(E), peek(), poll(), remove(), removeFirst(), removeLast(), size()

[List](http://java.sun.com/j2se/1.5.0/docs/api/java/util/List.html)
> add(E), add(int, E), addAll(Collection), addAll(int, Collection), clear(), contains(Object), containsAll(Collection), equals(Object), get(int), hashCode(), indexOf(Object), isEmpty(), iterator(), lastIndexOf(Object), listIterator(), listIterator(int), remove(int), remove(Object), removeAll(Collection), retainAll(Collection), set(int, E), size(), toArray(), toArray(T[.md](.md))

[ListIterator](http://java.sun.com/j2se/1.5.0/docs/api/java/util/ListIterator.html)
> add(E), hasNext(), hasPrevious(), next(), nextIndex(), previous(), previousIndex(), remove(), set(E)

[Map](http://java.sun.com/j2se/1.5.0/docs/api/java/util/Map.html)
> clear(), containsKey(Object), containsValue(Object), entrySet(), equals(Object), get(Object), hashCode(), isEmpty(), keySet(), put(K, V), putAll(Map), remove(Object), size(), values()

[Map.Entry](http://java.sun.com/j2se/1.5.0/docs/api/java/util/Map.Entry.html)
> equals(Object), getKey(), getValue(), hashCode(), setValue(V)

[MissingResourceException](http://java.sun.com/j2se/1.5.0/docs/api/java/util/MissingResourceException.html)
> MissingResourceException(String, String, String), getClassName(), getKey()

[NoSuchElementException](http://java.sun.com/j2se/1.5.0/docs/api/java/util/NoSuchElementException.html)
> NoSuchElementException(), NoSuchElementException(String)

[PriorityQueue](http://java.sun.com/j2se/1.5.0/docs/api/java/util/PriorityQueue.html)
> PriorityQueue(), PriorityQueue(Collection), PriorityQueue(int), PriorityQueue(int, Comparator), PriorityQueue(PriorityQueue), PriorityQueue(SortedSet), addAll(Collection), clear(), comparator(), contains(Object), containsAll(Collection), isEmpty(), iterator(), offer(E), peek(), poll(), remove(Object), removeAll(Collection), retainAll(Collection), size(), toArray(), toArray(T[.md](.md)), toString()

[Queue](http://java.sun.com/j2se/1.5.0/docs/api/java/util/Queue.html)
> element(), offer(E), peek(), poll(), remove()

[RandomAccess](http://java.sun.com/j2se/1.5.0/docs/api/java/util/RandomAccess.html)


[Set](http://java.sun.com/j2se/1.5.0/docs/api/java/util/Set.html)
> add(E), addAll(Collection), clear(), contains(Object), containsAll(Collection), equals(Object), hashCode(), isEmpty(), iterator(), remove(Object), removeAll(Collection), retainAll(Collection), size(), toArray(), toArray(T[.md](.md))

[SortedMap](http://java.sun.com/j2se/1.5.0/docs/api/java/util/SortedMap.html)
> comparator(), firstKey(), headMap(K), lastKey(), subMap(K, K), tailMap(K)

[SortedSet](http://java.sun.com/j2se/1.5.0/docs/api/java/util/SortedSet.html)
> comparator(), first(), headSet(E), last(), subSet(E, E), tailSet(E)

[Stack](http://java.sun.com/j2se/1.5.0/docs/api/java/util/Stack.html)
> Stack(), clone(), empty(), peek(), pop(), push(E), search(Object)

[TooManyListenersException](http://java.sun.com/j2se/1.5.0/docs/api/java/util/TooManyListenersException.html)
> TooManyListenersException(), TooManyListenersException(String)

[TreeMap](http://java.sun.com/j2se/1.5.0/docs/api/java/util/TreeMap.html)
> TreeMap(), TreeMap(Comparator), TreeMap(Map), TreeMap(SortedMap), clear(), comparator(), containsKey(Object), entrySet(), firstKey(), get(Object), headMap(K), lastKey(), put(K, V), remove(Object), size(), subMap(K, K), tailMap(K)

[TreeSet](http://java.sun.com/j2se/1.5.0/docs/api/java/util/TreeSet.html)
> TreeSet(), TreeSet(Collection), TreeSet(Comparator), TreeSet(SortedSet), add(E), clear(), comparator(), contains(Object), first(), headSet(E), iterator(), last(), remove(Object), size(), subSet(E, E), tailSet(E)

[Vector](http://java.sun.com/j2se/1.5.0/docs/api/java/util/Vector.html)
> Vector(), Vector(Collection), Vector(int), Vector(int, int), add(E), add(int, E), addAll(Collection), addAll(int, Collection), addElement(E), capacity(), clear(), clone(), contains(Object), containsAll(Collection), copyInto(Object[.md](.md)), elementAt(int), elements(), ensureCapacity(int), firstElement(), get(int), indexOf(Object), indexOf(Object, int), insertElementAt(E, int), isEmpty(), iterator(), lastElement(), lastIndexOf(Object), lastIndexOf(Object, int), remove(int), removeAll(Collection), removeAllElements(), removeElement(Object), removeElementAt(int), set(int, E), setElementAt(E, int), setSize(int), size(), toArray(), toArray(T[.md](.md)), toString(), trimToSize()

## Package java.io ##

[FilterOutputStream](http://java.sun.com/j2se/1.5.0/docs/api/java/io/FilterOutputStream.html)
> FilterOutputStream(OutputStream)

[OutputStream](http://java.sun.com/j2se/1.5.0/docs/api/java/io/OutputStream.html)
> OutputStream()

[PrintStream](http://java.sun.com/j2se/1.5.0/docs/api/java/io/PrintStream.html)
> PrintStream(OutputStream), print(boolean), print(char), print(char[.md](.md)), print(double), print(float), print(int), print(long), print(Object), print(String), println(), println(boolean), println(char), println(char[.md](.md)), println(double), println(float), println(int), println(long), println(Object), println(String)

[Serializable](http://java.sun.com/j2se/1.5.0/docs/api/java/io/Serializable.html)


## Package java.sql ##

[Date](http://java.sun.com/j2se/1.5.0/docs/api/java/sql/Date.html)
> Date(int, int, int), Date(long), valueOf(String), getHours(), getMinutes(), getSeconds(), setHours(int), setMinutes(int), setSeconds(int), toString()

[Time](http://java.sun.com/j2se/1.5.0/docs/api/java/sql/Time.html)
> Time(int, int, int), Time(long), valueOf(String), getDate(), getDay(), getMonth(), getYear(), setDate(int), setMonth(int), setYear(int), toString()

[Timestamp](http://java.sun.com/j2se/1.5.0/docs/api/java/sql/Timestamp.html)
> Timestamp(int, int, int, int, int, int, int), Timestamp(long), valueOf(String), after(Timestamp), before(Timestamp), compareTo(Date), compareTo(Timestamp), equals(Object), equals(Timestamp), getNanos(), getTime(), hashCode(), setNanos(int), setTime(long), toString()
