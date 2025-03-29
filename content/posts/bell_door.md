+++
title = 'Bell door'
date = 2024-11-08T18:25:40+01:00
draft = false
+++

//in construction

# Scan
Door bell signal on 433.93MHz
![Scanning with GQRX-SDR](/images/scanning.jpg)

# URH (Play and replay)
Recordings with Universal Radio Hacker (URH) and replay with HackRF One.
![urh recording](/images/urh_recording.jpg)

Door bell ring when the second pic is send.
The time between the two signals is 500ms (approximate).
Here, a bell ring 7 times.
![urh signal](/images/urh_complete_capture.png)

```
//first signal
10000 [Pause: 2713 samples]
100010001110111010001110100011101000100010001000100010001110100010001000100010001110100010000 [Pause: 2667 samples]
100010001110111010001110100011101000100010001000100010001110100010001000100010001110100010000 [Pause: 2718 samples]
100010001110111010001110100011101000100010001000000000000000000000000000 [Pause: 598911 samples]
//second signal
10000 [Pause: 2691 samples]
10001000111011101000111010001110100010001000100010001000111010001000100010001000111010001000 [Pause: 2801 samples]
100010001110111010001110100011101000100010001000100010001110100010001000100010001110100010000 [Pause: 2675 samples]
1000100011101110100011101000111010001000100010001000100011101000100010000000000000000000 [Pause: 5318425 samples]
```



Video of the replay with HackRF One.
<video width="640" height="360" controls>
  <source src="/videos/door_bell.TS.mp4" type="video/mp4">
      Your navigator does not support the video tag.
</video>





#TODO: exploite urh recoding and play with arduino rf433 module (try with Uno and MKR1010 wifi)

https://redfoxsec.com/blog/hacking-wireless-doorbells/

https://manuals.plus/fr/lexman/d06-wireless-doorbell-without-battery-manual
