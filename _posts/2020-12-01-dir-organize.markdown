---
layout: post
title:  "Python Directory Organizer"
date:   2020-12-01 15:00:00 -0700
categories: python
---
### Directory Organizer
Python script to clean up a given directory. The given directory will be sorted using a dictonary that can be expanded to any file type. 

### Sorting Dictonary
The dictonary used to sort is defined as:

```python
DIRECTORIES = {
	"Videos" : ['.mp4', '.mov', '.wmv', '.avi', '.flv'],
	"Audio"  : ['.mp3', '.oog', '.wav', '.wma'],
	"Pictures" : ['.jpg', '.gif', '.tif', '.png'],
	"Documents" : ['.txt', '.pdf', '.doc', '.bat', '.odt', '.docx', '.tex'],
	"Zipped" : ['.rar', '.zip', '.7z', '.gz'],
	"Code" : ['.py', '.java', '.cpp', '.c', '.mat'],
	}
```

Here the keys will become the new directories according to the file types. This section can be expanded to suit any file type.

### @Click
The main function is designed to be a cli function utilizing the `click` library. It takes in one option which is the path of the directory to be sorted.
```python
@click.option('-path', required=True, help='path of directory to clean')
```

### Organize Function
The function then iterates over the directory and does the following for each item in the directory:
* Gets the items path
```python
item_path = Path(item)
```
* Gets the items file type
```python
item_type = item_path.suffix.lower()
```
* Chooses the correct directory type to make
```python
directory_type = pickDirectory(item_type)
directory = Path(path).joinpath(pickDirectory(item_type))
```
* Makes that new directory
```python
if not os.path.exists(directory):
	directory.mkdir()
```
* Moves the file into that new directory
```python
new_file_path = f"{directory}/{item.name}"
os.replace(item_path, new_file_path)
```

### The whole script:
```python
import os
from pathlib import Path
import click


DIRECTORIES = {
	"Videos" : ['.mp4', '.mov', '.wmv', '.avi', '.flv'],
	"Audio"  : ['.mp3', '.oog', '.wav', '.wma'],
	"Pictures" : ['.jpg', '.gif', '.tif', '.png'],
	"Documents" : ['.txt', '.pdf', '.doc', '.bat', '.odt', '.docx', '.tex'],
	"Zipped" : ['.rar', '.zip', '.7z', '.gz'],
	"Code" : ['.py', '.java', '.cpp', '.c', '.mat'],
	}


def pickDirectory(item_type):
	for category, suffixes in DIRECTORIES.items():
		for suffix in suffixes:
			if suffix == item_type:
				return category
	return 'Unknown'


@click.command()
@click.option('-path', required=True, help='path of directory to clean')
def organize_dir(path):
	for item in os.scandir(path): 
		if not item.is_dir():
			item_path = Path(item)
			item_type = item_path.suffix.lower()
			directory_type = pickDirectory(item_type)
			directory = Path(path).joinpath(pickDirectory(item_type))
			if not os.path.exists(directory):
				directory.mkdir()
			new_file_path = f"{directory}/{item.name}"
			os.replace(item_path, new_file_path)
```



