title: Mobile App to detect (and then warn about) speed bumps
link: http://www.ashwinupadhyaya.com/2013/mobile-app-to-detect-and-then-warn-about-speed-bumps/
author: ashwin67
description: 
post_id: 1361
created: 2013/03/13 19:58:12
created_gmt: 2013/03/13 14:28:12
comment_status: open
post_name: mobile-app-to-detect-and-then-warn-about-speed-bumps
status: publish
post_type: post

# Mobile App to detect (and then warn about) speed bumps

As part of my MS project, I have been working on an android app that can detect and subsequently predict speed bumps. Although it is still a work in progress, I was able to get the detection part to work. Following image shows the speed bumps when travelling from Bangalore to Mysore:![Speed Bumps from Bangalore to Mysore](https://lh6.googleusercontent.com/-gBMvpQr0KXk/UUCI3druNQI/AAAAAAAACT4/6wlAE_-gf8I/s1157/bangalore_to_mysore_bumps.png) Based on GPS data and accelerometer data from my android app, I could detect using R scripts (run offline) if a bump has occurred at any point in time. These were then plotted over a lat-long map. Google maps integration is still not done. So, I have just overlaid my image over google maps using gimp. Next target is to upload the data points to a server running Python scripts to create a database of speed bumps. For me, the motivation to do this is simple. Speed bumps on highways are just frustrating. If a database of such speed bumps exists, then it is easy to get warnings on the phone based on GPS location. Building the database is the tough part.