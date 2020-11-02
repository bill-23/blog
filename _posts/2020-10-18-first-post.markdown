---
layout: post
title:  "Python Convert .mp4 to .mp3"
date:   2020-11-01 15:00:00 -0700
categories: python
---
Simple python project to convert a .mp4 file to a .mp3 file. This function now runs as a CLI tool using the click library. 

{% highlight ruby %}
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
{% endhighlight %}


