# benchmarkViewer #

Reads [benchmark](DevGuideJUnitBenchmarking.md) reports from a folder and displays their results, including various visualizations.

```
$PP_OFF
benchmarkViewer [path]
```


|   `path`  |  Specify the path to the XML `benchmark` reports. If the path is not specified, it defaults to the current working directory. |
|:----------|:------------------------------------------------------------------------------------------------------------------------------|


## Example ##

```
$PP_OFF
 ~/Foo> benchmarkViewer my/benchmark/results
```
Looks for report XML files in the folder `my/benchmark/results` and displays them in the viewer.

### See Also ###

  * [JUnit Benchmarking](DevGuideJUnitBenchmarking.md)
