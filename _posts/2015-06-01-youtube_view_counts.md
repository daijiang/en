---
layout: post
title:  Youtube view counts of Linear Algebra lectures 
categories: [notes]
tags: [notes, R, youtube]
---

I am learning linear algebra these days by watching the excellent series of lectures taught by Prof. Gilbert Strang at [Youtube](https://www.youtube.com/playlist?list=PLE7DDD91010BC51F8). During this journey, I think it would be interesting to look how many view count for all lectures. I expect the view counts will decline for later lectures.

Alright, first load some R packages in order to get data from Youtube.


```r
library(plyr)
library(dplyr)
```

```r
library(rvest) # for webpage scripting
library(stringr) # string handling
library(ggplot2) # plotting
library(knitr)
```

Then I searched online to find out the url of the playlist for all lectures. To find the correct CSS part, I followed [this tutorial](http://cran.r-project.org/web/packages/rvest/vignettes/selectorgadget.html).


```r
# the playlist first
url = html("https://www.youtube.com/playlist?list=PLE7DDD91010BC51F8")
lectures = html_nodes(url, ".yt-uix-tile-link")
# length(lectures) # 35 vedio
# get lecture names
lec_names = html_text(lectures) %>% 
  sapply(function(x) str_replace(x, "^.*Lec ([b0-9]*) .*", "\\1")) %>% 
  unname() %>% as.character()
lec_names[lec_names == "24b"] = 24.5
lec_names = as.numeric(lec_names)
```

Then, get urls for all lectures and extract their view counts.


```r
# get url for all lectures
url_all = ldply(lectures, function(x){
  paste0("https://www.youtube.com", html_attr(x, name = "href"))
})

# for each lecture, get the view count
view_all = sapply(url_all$V1, function(x){
  xx = html(x)
  view_count = html_nodes(xx, ".watch-view-count") %>% html_text() %>%
    gsub(",", "", .) %>% 
    as.numeric()
  view_count
})
```

Now, combine lecture names with their view counts.


```r
# combine lecture names with view count
dat = data_frame(lec = lec_names, view = unname(view_all))
kable(data.frame(lec = lec_names, view = format(dat$view, big.mark = ",")), 
      format = "pandoc")
```

  lec  view      
-----  ----------
  1.0  1,456,204 
  2.0    417,071 
  3.0    356,448 
  4.0    301,950 
  5.0    208,553 
  6.0    197,066 
  7.0    150,143 
  8.0    136,618 
  9.0    150,266 
 10.0    131,612 
 11.0    103,355 
 12.0     81,386 
 13.0     76,344 
 14.0    107,276 
 15.0     98,546 
 16.0     95,255 
 17.0     94,544 
 18.0     89,055 
 19.0     78,404 
 20.0     84,278 
 21.0    158,118 
 22.0    108,468 
 23.0     83,878 
 24.0     83,326 
 24.5     35,583 
 25.0     59,001 
 26.0     61,812 
 27.0     57,314 
 28.0     69,182 
 29.0     84,635 
 30.0     98,086 
 31.0     60,306 
 32.0     35,670 
 33.0     55,034 
 34.0     49,936 

Finally, let's plot it.


```r
# plot
ggplot(dat, aes(x = lec, y = view)) +
  geom_point(color = "red", size = 2) + 
  geom_line(color = "blue") +
  labs(x = "Lectures", y = "Youtube view count",
       title = "Youtube view counts of Linear Algebra lectures taught by 
       Gilbert Strang, Srping 2005")
```

![Imgur](http://i.imgur.com/DtGk7Rt.png)

Wow, the first lecture has 1,456,155 by far! However, the view count of the second lecture is about one million lower than the first one. It will be interesting to find out why lecture 21 and 22 have more view counts than their neighbors (I am getting their, at lecture 14 now!). The last lecture has about 50K views. Does this mean about 50K people finished all lectures? 

It clearly shows how hard it is to be persistent.


