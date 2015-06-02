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
      format = "html")
```


<table>
 <thead>
  <tr>
   <th style="text-align:right;"> lec </th>
   <th style="text-align:left;"> view </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 1.0 </td>
   <td style="text-align:left;"> 1,456,215 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2.0 </td>
   <td style="text-align:left;">   417,073 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3.0 </td>
   <td style="text-align:left;">   356,451 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 4.0 </td>
   <td style="text-align:left;">   301,951 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5.0 </td>
   <td style="text-align:left;">   208,554 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 6.0 </td>
   <td style="text-align:left;">   197,069 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 7.0 </td>
   <td style="text-align:left;">   150,147 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 8.0 </td>
   <td style="text-align:left;">   136,619 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 9.0 </td>
   <td style="text-align:left;">   150,267 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 10.0 </td>
   <td style="text-align:left;">   131,614 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 11.0 </td>
   <td style="text-align:left;">   103,357 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 12.0 </td>
   <td style="text-align:left;">    81,389 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 13.0 </td>
   <td style="text-align:left;">    76,344 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 14.0 </td>
   <td style="text-align:left;">   107,279 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 15.0 </td>
   <td style="text-align:left;">    98,549 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 16.0 </td>
   <td style="text-align:left;">    95,258 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 17.0 </td>
   <td style="text-align:left;">    94,545 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 18.0 </td>
   <td style="text-align:left;">    89,061 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 19.0 </td>
   <td style="text-align:left;">    78,405 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 20.0 </td>
   <td style="text-align:left;">    84,280 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 21.0 </td>
   <td style="text-align:left;">   158,119 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 22.0 </td>
   <td style="text-align:left;">   108,468 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 23.0 </td>
   <td style="text-align:left;">    83,880 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 24.0 </td>
   <td style="text-align:left;">    83,326 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 24.5 </td>
   <td style="text-align:left;">    35,586 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 25.0 </td>
   <td style="text-align:left;">    59,001 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 26.0 </td>
   <td style="text-align:left;">    61,814 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 27.0 </td>
   <td style="text-align:left;">    57,314 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 28.0 </td>
   <td style="text-align:left;">    69,184 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 29.0 </td>
   <td style="text-align:left;">    84,635 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 30.0 </td>
   <td style="text-align:left;">    98,086 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 31.0 </td>
   <td style="text-align:left;">    60,306 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 32.0 </td>
   <td style="text-align:left;">    35,670 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 33.0 </td>
   <td style="text-align:left;">    55,034 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 34.0 </td>
   <td style="text-align:left;">    49,936 </td>
  </tr>
</tbody>
</table>

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

Wow, the first lecture has 1,456,215 by far! However, the view count of the second lecture is about one million lower than the first one. It will be interesting to find out why lecture 21 and 22 have more view counts than their neighbors (I am getting their, at lecture 14 now!). The last lecture has about 50K views. Does this mean about 50K people finished all lectures? 

It clearly shows how hard it is to be persistent.


