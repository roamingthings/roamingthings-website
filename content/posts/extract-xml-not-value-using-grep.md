+++
author = "Alexander Sparkowsky"
categories = ["bash", "script", "shell", "xml"]
date = 2017-06-21T05:28:22Z
description = ""
draft = false
slug = "extract-xml-not-value-using-grep"
tags = ["bash", "script", "shell", "xml"]
title = "Extract XML node values using grep"

+++

To extract the value(s) of XML nodes with a specified name you can use grep and a regular expression:

```
$ grep -oP "(?<=<mytag>)[^<]+" <file>
```

To limit the number of nodes that are examined you can specify a maximum number:

```
$ grep -oPm1 "(?<=<mytag>)[^<]+" <file>
```

