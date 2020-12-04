---
layout: post
title:  "Python Directory Organizer"
date:   2020-12-01 15:00:00 -0700
categories: python
---
Python script to clean up a given directory. The given directory will be sorted using a dictonary that can be expanded to any file type. 
---
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
---

The main function is designed to be a cli function utilizing the `click` library. It takes in one option which is the path of the directory to be sorted.
```python
@click.option('-path', required=True, help='path of directory to clean')
```
---
The function then iterates over the directory and does the following for each item in the directory:
..* Gets the items path
..* Gets the items file type
..* Chooses the correct directory type to make
..* Makes that new directory
..* Moves the file into that new directory
---

