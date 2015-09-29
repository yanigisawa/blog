---
layout: post
title: Making Dance Requests with Python
created: 1408337874
---
The normal flow for requesting specific songs during a dance party (at least those held at Five Star Dance Studios in the Indianapolis area) is for the student (aka party attendee) to request the song of the DJ, and for the DJ to find the song on YouTube, and have it played from there. For a time, this was satisfactory, and each song that I requested was successfully found, and the correct version was played.

However, during one party, we requested a song, and while the correct arrangement was played, the version of the song on YouTube was very "tin-y", and didn't sound very good when played on the studio's speakers. 

After this experience, I decided I needed a way to request songs for the party such that I knew exactly which version would be played. In so doing, I also realized that this would take pressure off of the DJ of the evening, and allow me to embed the you tube video such that if needed, it would start X number of seconds into the video. (e.g. <a href="https://www.youtube.com/watch?v=gnFgdxW-UdE" target="_blank">"Thousand Years" from Sting</a> doesn't really start until 40 seconds in) I started maintaining <a href="https://docs.google.com/spreadsheet/ccc?key=0AqcS_eDL_8umdHVGTklWRWNwdGNpRzFpUExFOThnLXc&usp=drive_web#gid=0" target="_blank">a list</a> of songs that I would want to have played during a party.

Entering a number in the "Request" column indicates that I would like to request that song for the evening, and the numeric value indicates in what order I want the request to appear. All the selected requests appear on the adjoining sub-domain: <a href="http://partyrequests.jamesralexander.com/" target="_blank">partyrequests.jamesralexander.com</a>. By including the "You Tube Embed Code" in this spread sheet, I can automatically embed it in my party requests page later. This has the benefit of me being able to request the specific song and arrangement I'm interested in, and saves the DJ from having to look-up the song while "on-the-spot" in front of the room of dancers.
