# pip-openexr

The pip package for OpenEXR Python bindings:

https://pypi.org/project/OpenEXR/

## Installation

Make sure your system already has the OpenEXR library installed. It can be installed with homebrew on macOS:
Thanks to [ztzhang](https://github.com/jamesbowman/openexrpython/issues/44#issuecomment-1305799651) that commented on the [openexrpython](https://github.com/jamesbowman/openexrpython) repo explaining why the `openexr` package is not working on macOS.
> The root cause of this is that openexr bumped to version 3+ and refactored out Imath as a separate lib, and this binding is for version 2+.


```bash
#First, install openexr@2 with home brew by:
brew install openexr@2
```

Then type:

```bash

pip install .
```
The modification is to add the following lines to the `setup.py` file to make it work on macOS:

```diff
diff --git a/setup.py b/setup.py
index 9d1b069..0225f3a 100644
--- a/setup.py
+++ b/setup.py
@@ -22,11 +22,8 @@ https://github.com/sanguinariojoe/pip-openexr/issues
 
 
 print("Looking for libOpenEXR...")
-if platform.system() == "Linux" and system("ldconfig -p | grep libOpenEXR"):
-    # There is no libOpenEXR, probably an old version of OpenEXR
-    libraries=['Iex', 'Half', 'Imath', 'IlmImf', 'z']
-else:
-    libraries=['Iex', 'OpenEXR', 'z']
+
+libraries=['Iex', 'Half', 'Imath', 'IlmImf', 'z']
 
 extra_compile_args = ['-g', '-DVERSION="%s"' % VERSION]
 if platform.system() == 'Darwin':
@@ -49,10 +46,14 @@ setup(name='OpenEXR',
                             '/opt/local/include/OpenEXR',
                             '/usr/include/Imath',
                             '/usr/local/include/Imath',
-                            '/opt/local/include/Imath'],
+                            '/opt/local/include/Imath',
+                            '/opt/homebrew/opt/openexr@2/include/OpenEXR/',
+                            '/opt/homebrew/opt/ilmbase/include/OpenEXR'],
               library_dirs=['/usr/lib',
                             '/usr/local/lib',
-                            '/opt/local/lib'],
+                            '/opt/local/lib',
+                            '/opt/homebrew/opt/openexr@2/lib',
+                            '/opt/homebrew/opt/ilmbase/lib'],
               libraries=libraries,
               extra_compile_args=extra_compile_args)
   ],
```



(In some old pip versions you would need to add the `--user` option, or run the command above as superuser).

## Packaging

To create the same package you might get from pipy, you can follow [this guide](https://betterscientificsoftware.github.io/python-for-hpc/tutorials/python-pypi-packaging/)

## Report issues

If you find any problem, please feel free to report the issue [here](https://github.com/sanguinariojoe/pip-openexr/issues). If you already has the solution, please create a fork, add the required changes and submit a Pull Request.
