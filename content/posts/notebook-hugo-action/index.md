---
author:
  name: "David Meszaros"
date: 2024-09-28
linktitle: nbc
title: Notebook Converter
type:
- post
- posts
weight: 10
aliases:
- /blog/notebook-converter/
---

# This is a test to test whether the code is working or not


```python
from pathlib import Path

myp = Path("parent/child.ipynb")
```


```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots()

data = [5, 10, 15, 20, 25]
ax.plot(data)
plt.show()
```


    
![png](resources/output_3_0.png)
    



```python
# read notebook with nbconvert and modify it

import nbformat

with open('index.ipynb') as f:
    nb = nbformat.read(f, as_version=4)
    print(not nb.cells[6].source)
```

    False



```python
import nbconvert
import os

with open('index.ipynb') as f:
    nb = nbformat.read(f, as_version=4)

    # convert notebook to markdown
    markdown_exporter = nbconvert.MarkdownExporter()

    (body, resources) = markdown_exporter.from_notebook_node(nb)

    # Save resources (e.g., images) if they exist
    output_dir = args.output_dir
    if resources.get('outputs'):
        os.makedirs(output_dir, exist_ok=True)
        for filename, data in resources['outputs'].items():
            output_path = os.path.join(output_dir, filename)
            with open(output_path, 'wb') as resource_file:
                resource_file.write(data)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    Cell In[6], line 13
         10 (body, resources) = markdown_exporter.from_notebook_node(nb)
         12 # Save resources (e.g., images) if they exist
    ---> 13 output_dir = args.output_dir
         14 if resources.get('outputs'):
         15     os.makedirs(output_dir, exist_ok=True)


    NameError: name 'args' is not defined



```python
import os
import datetime

t = os.path.getmtime("index.ipynb")
print(str(datetime.datetime.fromtimestamp(t).date()))
```

    2024-09-29

