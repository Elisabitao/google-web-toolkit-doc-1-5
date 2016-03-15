# Filtering Public and Source Packages #

The [module XML format's](DevGuideModuleXml.md) `<public>`, `<source>` and `<super-source>` elements supports certain attributes and nested elements to allow pattern-based inclusion and exclusion in the [public path](DevGuideModules.md).  These elements follow the same rules as [Ant](http://ant.apache.org/)'s `FileSet` element. Please see the [documentation](http://ant.apache.org/manual/CoreTypes/fileset.html) for `FileSet` for a general overview.  These elements do not support the full `FileSet` semantics. Only the following attributes and nested elements are currently supported:

  * The `includes` attribute
  * The `excludes` attribute
  * The `defaultexcludes` attribute
  * The `casesensitive` attribute
  * Nested `include` tags
  * Nested `exclude` tags

Other attributes and nested elements are not supported.

### Important ###
The default value of `defaultexcludes` is `true`. By default, the patterns listed [here](http://ant.apache.org/manual/dirtasks.html#defaultexcludes) are excluded.
