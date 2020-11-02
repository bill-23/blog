---
layout: post
title:  "Python Convert .mp4 to .mp3"
date:   2020-11-01 15:00:00 -0700
categories: python
---
Simple Python method to convert a movie `.mp4` to an audio file `.mp3`. The script utilizes the click library to convert this into a CLI tool.

---

This method uses two external libraries `click` and `moviepy`

To get click install via pip: {% highlight ruby %} pip install click {% endhighlight %}

To get moviepy install via pip: {% highlight ruby %} pip install moviepy {% endhighlight %}

---

To begin a few click paramaters are set. Click needs to be told that this method will be a command, and what options will be set. In the case of this method the following options are added: 

```python
@click.command()
@click.option('-d', is_flag=True, help='delete .mp4 after converting to .mp3')
@click.option('-p', required=True, help='path of files to convert')
```

The option `-d` is set as a boolean flag, which if set will remove the .mp4 file after converting it to a .mp3

The `-p` option is the filepath of the directory that holds the .mp4's to be converted. 

---

This method is set up to log the entire process to a file `logfile.log`. This is especially helpful if the script is run in the background. To achieve this python's built in `logging` package is utilized. 

To do this import `logging`

A basic configuration is then set: 
{% highlight ruby %} 
logging.basicConfig(filename='/mnt/cloud/code/python/convert/logfile.log', level=logging.INFO) 
{% endhighlight %}

This sets the path of the `logfile.log`. 

To begin logging any of the built in attributes [Docs](https://docs.python.org/3/library/logging.html) can be used

In this case the `logging.info` attribute will be used for general logging, and the `logging.error` for catching any errors. 

---

The basic flow of the method will be to check each filename in the chosen directory to see if it ends in `.mp4`. If so then the file will be converted, if not it will move on.

---

The actual conversion of the file will be handled by the `moviepy` package.

To use:
```python
import moviepy.editor as mp
```

---

The final method is as follows:

```python
import moviepy.editor as mp
import os
import click
import logging
from datetime import datetime


@click.command()
@click.option('-d', is_flag=True, help='delete .mp4 after converting to .mp3')
@click.option('-p', required=True, help='path of files to convert')
def convert_video_to_audio(d, p):
	logging.basicConfig(filename='/mnt/cloud/code/python/convert/logfile.log', level=logging.INFO)
	logging.info(f"{datetime.now().strftime('%Y-%m-%d %H:%M:%S')} - Starting new audio conversion")
	for filename in os.listdir(p):
		if os.path.splitext(filename)[1] == '.mp4':
			try:
				logging.info(f"{datetime.now().strftime('%Y-%m-%d %H:%M:%S')} - Converting {filename}")
				clip = mp.VideoFileClip(filename)
				clip.audio.write_audiofile(f"{os.path.splitext(filename)[0]}.mp3")
				if d:
					logging.info(f"{datetime.now().strftime('%Y-%m-%d %H:%M:%S')} - Removing {filename}")
					os.remove(filename)
			except KeyError as ke:
				logging.error(f"{datetime.now().strftime('%Y-%m-%d %H:%M:%S')} - Exception: {str(ke)}")
```

---

In case this is to  be integrated without the need for a CLI, the code would be transformed into:

```python
import moviepy.editor as mp
import os


def convert_video_to_audio(delete: bool, path: str):
	for filename in os.listdir(path):
		if os.path.splitext(filename)[1] == '.mp4':
			try:
				clip = mp.VideoFileClip(filename)
				clip.audio.write_audiofile(f"{os.path.splitext(filename)[0]}.mp3")
				if delete:
					os.remove(filename)
			except KeyError as ke:
				pass
```

 