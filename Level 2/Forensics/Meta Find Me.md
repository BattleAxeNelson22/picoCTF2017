# Meta Find Me - 70 PTS
Find the location of the flag in the image: image.jpg. Note: Latitude and longitude values are in degrees with no degree symbols,/direction letters, minutes, seconds, or periods. They should only be digits. The flag is not just a set of coordinates - if you think that, keep looking!

## Hint
How can images store location data? Perhaps search for GPS info on photos.

# Write Up
Visit the image link in the challenge text, mine was:https://webshell2017.picoctf.com/static/3e3e5d085bb54e41cf652a0311fdf7bb/image.jpg

our challenge is clear, find out where the image was taken. Lucky for us theres already tools that can do this.

"OK Google, get gps info from a photo online" - http://www.verexif.com/en/

Neat...

Grab the url of your image and paste it into the URL field. Then lets check out that exif. 

Hey look at the comment!

>"Your flag is flag_2_meta_4_me_<lat>_<lon>_5d71. Now find the GPS coordinates of this image! (Degrees only please)"

So now we just need the latitude and longitude. well verexif just told us tthat too. Above the comment are the GPS Latitude and the GPS Longitude. So my new flag is:

>flag_2_meta_4_me_72_138_5d71

There are actually a handful of ways you can see EXIF data. To get the coordinates you can also save the image to your desktop, then right click > view properties > details. 

# Resources
http://www.verexif.com/en/

https://en.wikipedia.org/wiki/Exif
