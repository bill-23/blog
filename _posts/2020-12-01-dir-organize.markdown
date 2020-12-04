---
layout: post
title:  "Python Directory Organizer"
date:   2020-12-01 15:00:00 -0700
categories: python
---
Python script to clean up a given directory. The given directory will be sorted using a dictonary that can be expanded to any file type. 
---
The dictonary used to sort is defined as

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


  
