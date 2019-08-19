---
title: "Alfred Workflow Python Development"
author: "Haobo Gu"
tags: [Alfred]
date: 2019-08-19T11:33:02+08:00
draft: true
---

<!--more-->

# Alfred Workflow Python

Install Alfred workflow python develop toolkit: https://www.deanishe.net/alfred-workflow/installation.html



### Setting variables

In the upstream python script, `root variable` and `item variable` can be used to pass variables to downstream.

```python
it = wf.add_item(title, subtitle, arg, valid)
it.setvar(var_name, var_value)
```

`wf.add_item` adds an item, which is a collection of variables as well as one candidate in alfred window. When the candidate is chosen, the item is ***actioned***. 

When an item is actioned, all variables in `arg` is passed as `{query}` to downstream

`title` and `subtitle` is the text appears in the alfred window.

### Getting variables

In the downstream script, `os.getenv(var_name)` is used to get variables:

```python
import os
var_value = os.getenv(var_name)
```

