
# Export in Dart

The code in `lib/src` in your Dart project isn't publicly exposed to other modules in your project. It is considered as private.
<br>
This is one of the structural advantages of the packaging system in Dart.

In case you want to export code present in the files in `lib/src`, you can use the `export` keyword in Dart.

Here's an example:

```
export 'src/file1.dart';
export 'src/file2.dart';
export 'src/handlers/file3.dart';
export 'src/viewmodels/file4.dart';
```
