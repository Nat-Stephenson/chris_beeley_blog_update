---
author: chrisbeeley
categories:
- Uncategorized
date: "2012-07-25T08:49:43Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=239
id: 239
title: Super easy heatmaps of postcodes
url: /?p=239
---

Whatever I want to do, there are always intrepid explorers who’ve been there and blogged it, and so the satisfaction of my long held desire to get to know more about how Nottinghamshire Healthcare’s services are spread geographically has been wonderfully expedited by [these](http://blog.revolutionanalytics.com/2012/07/making-beautiful-maps-in-r-with-ggmap.html) amazing [blog posts](http://stevendkay.wordpress.com/2010/04/21/plotting-postcode-density-heatmaps-in-r/).

Special thanks, of course, go to David Kahle and Hadley Wickham, progenitors of the mighty [ggmap](http://cran.r-project.org/web/packages/ggmap/index.html) package and also to the fine folk at [geonames](http://www.geonames.org/) who freely distribute postcodes from around the world in .csv format.

With the thanks out of the way, there’s almost no work for me to do at all, and I’ve produced this lovely heatmap with absolutely minimal coding. I can’t tell you what it represents, I’m afraid, because I haven’t cleared the data for release, and actually it doesn’t represent anything particularly interesting at the moment. I need to do some preparation of the data but I naturally did this bit first because it’s more fun.

[![](http://chrisbeeley.net/wp-content/uploads/2012/07/map2.png?w=300 "Map")Click to expand!](http://chrisbeeley.net/wp-content/uploads/2012/07/map2.png)

```
<pre class="brush: r; title: ; notranslate" title="">
library(ggmap)

myUni=mydata[!duplicated(mydata$ClientID),] # produce dataframe with unique individuals

mywhere=merge(myUni, mycodes, by.x="ClientHomePostcode",
              by.y="Postcode", all=FALSE) # merge with postcode data

### Plot!

map.center = geocode("Nottingham, UK") # Centre map on Nottingham

myMap = qmap(c(lon=map.center$lon, lat=map.center$lat),
              source="google", zoom=10) # download map from Google

myMap + stat_bin2d(bins=80, aes(x=Long, y=Lat), alpha=.6, data=mywhere) +
  scale_fill_gradient(low = "blue", high ="red")
  # plot with a bit of transparency
```

Note finally that you can use Google’s map API to give you latitudes and longitudes from postcode data (using the geocode() function), but you are limited to 2500 queries per day. I had many more than that so I needed to download the postcode data.