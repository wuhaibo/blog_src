---
title: python how to import my module from folder
description: Show how to add module to python module search path
date: 2021-06-17 14:02:33
updated: 2021-06-17 14:02:33
tags:
  - python
categories:
  - 技術
comments: true
---
# python_module_include_path
Show how to add module to python module search path



Code could be found at here(https://github.com/wuhaibo/python_module_include_path)

## Where does python search modules? 

Run the following cmd 

```Shell

python_module_include_path\myMod>python showSysPath.py
# myMod is in searching path

python_module_include_path>python myMod/showSysPath.py
# myMode is in searching path

```
The folder containing showSysPath.py would be added into the python module searching path.

## How to import module from folder?

```Shell
.
+-- test.py
+-- myMod
|   +-- __init__.py
|   +-- greeting.py
+-- test
|   +-- __init__.py
|   +-- test.py
```
say we want to import myMod/greeting in test/test.py
in test.py we add following code 

```python
## setup path
import sys
import os
basePath = os.path.dirname(os.path.abspath(__file__)) + "\.."
sys.path.append(basePath)
## end setup path
```

## Memo
the method import using relative path like 'from ..myMode import greeting' is not good, cause it lacks of the ability to handle namespace pullution. 





