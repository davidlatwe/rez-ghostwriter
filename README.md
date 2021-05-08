# rez-ghostwriter
Rez developer package ghostwriter for unit-test

### Install

Just copy the `ghostwriter.py` into your test module.

### Example

```python
from ghostwriter import DeveloperRepository, early, late, include

dev_repo = DeveloperRepository("test/dev")
dev_repo.debug = 1  # will print out saved package.py

@early()
def requires():
    return [] if building else ["!bar"]

@late()
def bar():
    return "cheers"

@include("util")
def commands():
    env.PATH.append("{this.root}/bin")
    env.BAR = this.bar
    util.do("stuff")

dev_repo.add("foo",
             version="1",
             requires=requires,
             variants=[["os-*"]],
             bar=bar,
             build_command=False,
             commands=commands)

'''output
# -*- coding: utf-8 -*-

name = 'foo'

version = '1'

@early()
def requires():
    return [] if building else ["!bar"]

variants = [['os-*']]

@include('util')
def commands():
    env.PATH.append("{this.root}/bin")
    env.BAR = this.bar
    util.do("stuff")

@late()
def bar():
    return "cheers"

build_command = False

'''
```
