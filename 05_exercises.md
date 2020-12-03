---
title: 'Weekly Exercises 5'
author: "Will Brazgel"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(googlesheets4) # for reading googlesheet data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(transformr)    # for "tweening" (gganimate)
library(shiny)         # for creating interactive apps
gs4_deauth()           # To not have to authorize each time you knit.
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
garden_harvest <- read_sheet("https://docs.google.com/spreadsheets/d/1DekSazCzKqPS2jnGhKue7tLxRU3GVL1oxi-4bEM5IWw/edit?usp=sharing") %>% 
  mutate(date = ymd(date))

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.


```r
garden_harvest_graph <- garden_harvest %>%
  filter(vegetable %in% c("cucumbers", "beans", "zucchini"), date < mdy("10/1/2020")) %>%
  select(vegetable, weight, date) %>%
  group_by(vegetable, date) %>%
  mutate(weight_lb = weight*0.0022) %>% 
  summarise(total_weight = sum(weight_lb)) %>% 
  ggplot(aes(x = date, y = total_weight, color = vegetable, shape = vegetable))+
  geom_point()+
  geom_line()+
  labs(title = "Growth in Weight (lb) of Vegetables",
       x = "",
       y = "")+
  scale_y_continuous(n.breaks = 10)+
  scale_colour_viridis_d()+
  theme_minimal()+
  theme(legend.position="top",
        panel.grid.minor.x = element_blank(),
        panel.grid.major.x = element_blank())

ggplotly(garden_harvest_graph)
```

<!--html_preserve--><div id="htmlwidget-5c18d99d563e07429fc0" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-5c18d99d563e07429fc0">{"x":{"data":[{"x":[18449,18451,18452,18454,18456,18458,18461,18463,18464,18465,18466,18467,18470,18472,18474,18477,18479,18482,18485,18487,18490,18491,18492,18494,18499,18506,18532],"y":[0.517,0.3916,0.308,1.5422,0.9746,1.6346,1.452,1.1418,0.0462,0.7722,0.2838,0.22,1.6016,0.671,1.3024,1.2584,0.605,1.705,2.3716,1.441,1.5246,1.001,0.495,1.353,1.1132,0.352,0.385],"text":["date: 2020-07-06<br />total_weight: 0.5170<br />vegetable: beans<br />vegetable: beans","date: 2020-07-08<br />total_weight: 0.3916<br />vegetable: beans<br />vegetable: beans","date: 2020-07-09<br />total_weight: 0.3080<br />vegetable: beans<br />vegetable: beans","date: 2020-07-11<br />total_weight: 1.5422<br />vegetable: beans<br />vegetable: beans","date: 2020-07-13<br />total_weight: 0.9746<br />vegetable: beans<br />vegetable: beans","date: 2020-07-15<br />total_weight: 1.6346<br />vegetable: beans<br />vegetable: beans","date: 2020-07-18<br />total_weight: 1.4520<br />vegetable: beans<br />vegetable: beans","date: 2020-07-20<br />total_weight: 1.1418<br />vegetable: beans<br />vegetable: beans","date: 2020-07-21<br />total_weight: 0.0462<br />vegetable: beans<br />vegetable: beans","date: 2020-07-22<br />total_weight: 0.7722<br />vegetable: beans<br />vegetable: beans","date: 2020-07-23<br />total_weight: 0.2838<br />vegetable: beans<br />vegetable: beans","date: 2020-07-24<br />total_weight: 0.2200<br />vegetable: beans<br />vegetable: beans","date: 2020-07-27<br />total_weight: 1.6016<br />vegetable: beans<br />vegetable: beans","date: 2020-07-29<br />total_weight: 0.6710<br />vegetable: beans<br />vegetable: beans","date: 2020-07-31<br />total_weight: 1.3024<br />vegetable: beans<br />vegetable: beans","date: 2020-08-03<br />total_weight: 1.2584<br />vegetable: beans<br />vegetable: beans","date: 2020-08-05<br />total_weight: 0.6050<br />vegetable: beans<br />vegetable: beans","date: 2020-08-08<br />total_weight: 1.7050<br />vegetable: beans<br />vegetable: beans","date: 2020-08-11<br />total_weight: 2.3716<br />vegetable: beans<br />vegetable: beans","date: 2020-08-13<br />total_weight: 1.4410<br />vegetable: beans<br />vegetable: beans","date: 2020-08-16<br />total_weight: 1.5246<br />vegetable: beans<br />vegetable: beans","date: 2020-08-17<br />total_weight: 1.0010<br />vegetable: beans<br />vegetable: beans","date: 2020-08-18<br />total_weight: 0.4950<br />vegetable: beans<br />vegetable: beans","date: 2020-08-20<br />total_weight: 1.3530<br />vegetable: beans<br />vegetable: beans","date: 2020-08-25<br />total_weight: 1.1132<br />vegetable: beans<br />vegetable: beans","date: 2020-09-01<br />total_weight: 0.3520<br />vegetable: beans<br />vegetable: beans","date: 2020-09-27<br />total_weight: 0.3850<br />vegetable: beans<br />vegetable: beans"],"type":"scatter","mode":"markers+lines","marker":{"autocolorscale":false,"color":"rgba(68,1,84,1)","opacity":1,"size":5.66929133858268,"symbol":"circle","line":{"width":1.88976377952756,"color":"rgba(68,1,84,1)"}},"hoveron":"points","name":"beans","legendgroup":"beans","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","line":{"width":1.88976377952756,"color":"rgba(68,1,84,1)","dash":"solid"},"frame":null},{"x":[18451,18452,18455,18456,18457,18458,18460,18461,18462,18463,18465,18467,18468,18470,18471,18472,18473,18474,18475,18477,18480,18481,18482,18485,18487,18489,18492,18494,18495,18497,18499],"y":[0.3982,0.1716,0.286,0.1034,0.3344,2.3254,0.7634,0.6468,1.1682,0.3938,1.441,1.155,1.9822,1.727,0.1672,1.1308,1.3772,0.3828,2.486,2.541,0.2794,2.9194,3.7334,2.2638,1.3178,0.7722,6.8662,0.154,2.1934,1.6434,0.3938],"text":["date: 2020-07-08<br />total_weight: 0.3982<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-09<br />total_weight: 0.1716<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-12<br />total_weight: 0.2860<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-13<br />total_weight: 0.1034<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-14<br />total_weight: 0.3344<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-15<br />total_weight: 2.3254<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-17<br />total_weight: 0.7634<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-18<br />total_weight: 0.6468<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-19<br />total_weight: 1.1682<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-20<br />total_weight: 0.3938<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-22<br />total_weight: 1.4410<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-24<br />total_weight: 1.1550<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-25<br />total_weight: 1.9822<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-27<br />total_weight: 1.7270<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-28<br />total_weight: 0.1672<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-29<br />total_weight: 1.1308<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-30<br />total_weight: 1.3772<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-07-31<br />total_weight: 0.3828<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-08-01<br />total_weight: 2.4860<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-08-03<br />total_weight: 2.5410<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-08-06<br />total_weight: 0.2794<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-08-07<br />total_weight: 2.9194<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-08-08<br />total_weight: 3.7334<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-08-11<br />total_weight: 2.2638<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-08-13<br />total_weight: 1.3178<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-08-15<br />total_weight: 0.7722<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-08-18<br />total_weight: 6.8662<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-08-20<br />total_weight: 0.1540<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-08-21<br />total_weight: 2.1934<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-08-23<br />total_weight: 1.6434<br />vegetable: cucumbers<br />vegetable: cucumbers","date: 2020-08-25<br />total_weight: 0.3938<br />vegetable: cucumbers<br />vegetable: cucumbers"],"type":"scatter","mode":"markers+lines","marker":{"autocolorscale":false,"color":"rgba(33,144,140,1)","opacity":1,"size":5.66929133858268,"symbol":"triangle-up","line":{"width":1.88976377952756,"color":"rgba(33,144,140,1)"}},"hoveron":"points","name":"cucumbers","legendgroup":"cucumbers","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","line":{"width":1.88976377952756,"color":"rgba(33,144,140,1)","dash":"solid"},"frame":null},{"x":[18449,18455,18456,18458,18461,18462,18463,18464,18465,18467,18470,18472,18474,18475,18476,18477,18478,18481,18482,18483,18485,18487,18488,18489,18490,18492,18494,18495,18497,18499,18502,18506,18512,18513],"y":[0.385,1.0824,0.319,0.8646,0.1782,0.7568,0.2948,0.242,0.1672,2.9062,3.3924,1.0054,2.673,0.3608,2.585,0.5544,0.9394,2.6818,0.979,0.9746,1.6082,3.9028,0.8162,1.8898,1.452,2.5322,1.8348,2.4684,5.3592,2.024,7.1368,6.2282,7.2248,2.86],"text":["date: 2020-07-06<br />total_weight: 0.3850<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-07-12<br />total_weight: 1.0824<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-07-13<br />total_weight: 0.3190<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-07-15<br />total_weight: 0.8646<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-07-18<br />total_weight: 0.1782<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-07-19<br />total_weight: 0.7568<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-07-20<br />total_weight: 0.2948<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-07-21<br />total_weight: 0.2420<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-07-22<br />total_weight: 0.1672<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-07-24<br />total_weight: 2.9062<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-07-27<br />total_weight: 3.3924<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-07-29<br />total_weight: 1.0054<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-07-31<br />total_weight: 2.6730<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-01<br />total_weight: 0.3608<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-02<br />total_weight: 2.5850<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-03<br />total_weight: 0.5544<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-04<br />total_weight: 0.9394<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-07<br />total_weight: 2.6818<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-08<br />total_weight: 0.9790<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-09<br />total_weight: 0.9746<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-11<br />total_weight: 1.6082<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-13<br />total_weight: 3.9028<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-14<br />total_weight: 0.8162<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-15<br />total_weight: 1.8898<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-16<br />total_weight: 1.4520<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-18<br />total_weight: 2.5322<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-20<br />total_weight: 1.8348<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-21<br />total_weight: 2.4684<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-23<br />total_weight: 5.3592<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-25<br />total_weight: 2.0240<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-08-28<br />total_weight: 7.1368<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-09-01<br />total_weight: 6.2282<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-09-07<br />total_weight: 7.2248<br />vegetable: zucchini<br />vegetable: zucchini","date: 2020-09-08<br />total_weight: 2.8600<br />vegetable: zucchini<br />vegetable: zucchini"],"type":"scatter","mode":"markers+lines","marker":{"autocolorscale":false,"color":"rgba(253,231,37,1)","opacity":1,"size":5.66929133858268,"symbol":"square","line":{"width":1.88976377952756,"color":"rgba(253,231,37,1)"}},"hoveron":"points","name":"zucchini","legendgroup":"zucchini","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","line":{"width":1.88976377952756,"color":"rgba(253,231,37,1)","dash":"solid"},"frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":25.5707762557078,"l":16.8036529680365},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Growth in Weight (lb) of Vegetables","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[18444.85,18536.15],"tickmode":"array","ticktext":["Aug","Sep","Oct"],"tickvals":[18475,18506,18536],"categoryorder":"array","categoryarray":["Aug","Sep","Oct"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"y","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.31273,7.58373],"tickmode":"array","ticktext":["0","1","2","3","4","5","6","7"],"tickvals":[0,1,2,3,4,5,6,7],"categoryorder":"array","categoryarray":["0","1","2","3","4","5","6","7"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.93503937007874},"annotations":[{"text":"vegetable","x":1.02,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"left","yanchor":"bottom","legendTitle":true}],"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"102f43e2d7194":{"x":{},"y":{},"colour":{},"shape":{},"type":"scatter"},"102f41eb44e08":{"x":{},"y":{},"colour":{},"shape":{}}},"cur_data":"102f43e2d7194","visdat":{"102f43e2d7194":["function (y) ","x"],"102f41eb44e08":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


```r
mobile <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-11-10/mobile.csv')
landline <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-11-10/landline.csv')
```


```r
mobile_graph <- mobile %>% 
  filter(entity %in% c("United States", "Germany", "China", "Russia")) %>% 
  group_by(year) %>% 
  ggplot(aes(x = year, y = mobile_subs, color = entity))+
  geom_line(size = 1)+
  labs(title = "How Have the Number of Fixed Mobile Subcriptions (per 100 people)
  Changed Over the Last 30 Years?",
  x = "",
  y = "")

ggplotly(mobile_graph)
```

<!--html_preserve--><div id="htmlwidget-0e4932c3fc613d1e3a39" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-0e4932c3fc613d1e3a39">{"x":{"data":[{"x":[1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017],"y":[0.00156246108560127,0.00399803723482808,0.0146962148422474,0.0524184314303156,0.127584238689697,0.292675451093842,0.548248139949784,1.05101636952317,1.8827693383495,3.3946818715902,6.6443320165695,11.2182025299349,15.8630534010178,20.6647727085398,25.4811335251777,29.7668740739467,34.6866420099891,40.9414865975522,47.6969456401434,55.2645243959093,63.173361051305,72.1219183898949,80.8723179789639,88.8862477291362,92.5173289187736,92.4808728658819,97.2521300341807,104.58168186767],"text":["year: 1990<br />mobile_subs: 1.562461e-03<br />entity: China","year: 1991<br />mobile_subs: 3.998037e-03<br />entity: China","year: 1992<br />mobile_subs: 1.469621e-02<br />entity: China","year: 1993<br />mobile_subs: 5.241843e-02<br />entity: China","year: 1994<br />mobile_subs: 1.275842e-01<br />entity: China","year: 1995<br />mobile_subs: 2.926755e-01<br />entity: China","year: 1996<br />mobile_subs: 5.482481e-01<br />entity: China","year: 1997<br />mobile_subs: 1.051016e+00<br />entity: China","year: 1998<br />mobile_subs: 1.882769e+00<br />entity: China","year: 1999<br />mobile_subs: 3.394682e+00<br />entity: China","year: 2000<br />mobile_subs: 6.644332e+00<br />entity: China","year: 2001<br />mobile_subs: 1.121820e+01<br />entity: China","year: 2002<br />mobile_subs: 1.586305e+01<br />entity: China","year: 2003<br />mobile_subs: 2.066477e+01<br />entity: China","year: 2004<br />mobile_subs: 2.548113e+01<br />entity: China","year: 2005<br />mobile_subs: 2.976687e+01<br />entity: China","year: 2006<br />mobile_subs: 3.468664e+01<br />entity: China","year: 2007<br />mobile_subs: 4.094149e+01<br />entity: China","year: 2008<br />mobile_subs: 4.769695e+01<br />entity: China","year: 2009<br />mobile_subs: 5.526452e+01<br />entity: China","year: 2010<br />mobile_subs: 6.317336e+01<br />entity: China","year: 2011<br />mobile_subs: 7.212192e+01<br />entity: China","year: 2012<br />mobile_subs: 8.087232e+01<br />entity: China","year: 2013<br />mobile_subs: 8.888625e+01<br />entity: China","year: 2014<br />mobile_subs: 9.251733e+01<br />entity: China","year: 2015<br />mobile_subs: 9.248087e+01<br />entity: China","year: 2016<br />mobile_subs: 9.725213e+01<br />entity: China","year: 2017<br />mobile_subs: 1.045817e+02<br />entity: China"],"type":"scatter","mode":"lines","line":{"width":3.77952755905512,"color":"rgba(248,118,109,1)","dash":"solid"},"hoveron":"points","name":"China","legendgroup":"China","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017],"y":[0.344558604538726,0.66895576258212,1.21416197407203,2.20364637455438,3.07731121362939,4.5851456249636,6.7693085592196,10.1546179229301,17.0713265126228,28.7745902719745,59.1524442131841,68.8359799728826,72.4502470645568,79.3276783389959,87.2807009975897,97.0611023215346,105.042468972387,118.302990208663,130.065126568723,129.684686382658,109.277748868484,112.313764873543,113.980879929433,123.095833257604,122.138195201698,117.932453171631,126.314367711806,129.088475584936],"text":["year: 1990<br />mobile_subs: 3.445586e-01<br />entity: Germany","year: 1991<br />mobile_subs: 6.689558e-01<br />entity: Germany","year: 1992<br />mobile_subs: 1.214162e+00<br />entity: Germany","year: 1993<br />mobile_subs: 2.203646e+00<br />entity: Germany","year: 1994<br />mobile_subs: 3.077311e+00<br />entity: Germany","year: 1995<br />mobile_subs: 4.585146e+00<br />entity: Germany","year: 1996<br />mobile_subs: 6.769309e+00<br />entity: Germany","year: 1997<br />mobile_subs: 1.015462e+01<br />entity: Germany","year: 1998<br />mobile_subs: 1.707133e+01<br />entity: Germany","year: 1999<br />mobile_subs: 2.877459e+01<br />entity: Germany","year: 2000<br />mobile_subs: 5.915244e+01<br />entity: Germany","year: 2001<br />mobile_subs: 6.883598e+01<br />entity: Germany","year: 2002<br />mobile_subs: 7.245025e+01<br />entity: Germany","year: 2003<br />mobile_subs: 7.932768e+01<br />entity: Germany","year: 2004<br />mobile_subs: 8.728070e+01<br />entity: Germany","year: 2005<br />mobile_subs: 9.706110e+01<br />entity: Germany","year: 2006<br />mobile_subs: 1.050425e+02<br />entity: Germany","year: 2007<br />mobile_subs: 1.183030e+02<br />entity: Germany","year: 2008<br />mobile_subs: 1.300651e+02<br />entity: Germany","year: 2009<br />mobile_subs: 1.296847e+02<br />entity: Germany","year: 2010<br />mobile_subs: 1.092777e+02<br />entity: Germany","year: 2011<br />mobile_subs: 1.123138e+02<br />entity: Germany","year: 2012<br />mobile_subs: 1.139809e+02<br />entity: Germany","year: 2013<br />mobile_subs: 1.230958e+02<br />entity: Germany","year: 2014<br />mobile_subs: 1.221382e+02<br />entity: Germany","year: 2015<br />mobile_subs: 1.179325e+02<br />entity: Germany","year: 2016<br />mobile_subs: 1.263144e+02<br />entity: Germany","year: 2017<br />mobile_subs: 1.290885e+02<br />entity: Germany"],"type":"scatter","mode":"lines","line":{"width":3.77952755905512,"color":"rgba(124,174,0,1)","dash":"solid"},"hoveron":"points","name":"Germany","legendgroup":"Germany","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017],"y":[0,0.000202653548263003,0.00404536131060267,0.00673712019089118,0.0186939244172913,0.0596983566645902,0.150601803461358,0.328137203326143,0.506958152631899,0.932910736152488,2.22901482476557,5.31530105188727,12.1276698995492,24.9923652015865,51.1801333874305,83.5548785134696,105.129023559684,119.594711963331,139.444992932678,160.769833471315,166.037582959075,142.221119099379,145.07334884403,152.022690131279,153.748076204445,157.961804793678,159.154604996302,157.887534831124],"text":["year: 1990<br />mobile_subs: 0.000000e+00<br />entity: Russia","year: 1991<br />mobile_subs: 2.026535e-04<br />entity: Russia","year: 1992<br />mobile_subs: 4.045361e-03<br />entity: Russia","year: 1993<br />mobile_subs: 6.737120e-03<br />entity: Russia","year: 1994<br />mobile_subs: 1.869392e-02<br />entity: Russia","year: 1995<br />mobile_subs: 5.969836e-02<br />entity: Russia","year: 1996<br />mobile_subs: 1.506018e-01<br />entity: Russia","year: 1997<br />mobile_subs: 3.281372e-01<br />entity: Russia","year: 1998<br />mobile_subs: 5.069582e-01<br />entity: Russia","year: 1999<br />mobile_subs: 9.329107e-01<br />entity: Russia","year: 2000<br />mobile_subs: 2.229015e+00<br />entity: Russia","year: 2001<br />mobile_subs: 5.315301e+00<br />entity: Russia","year: 2002<br />mobile_subs: 1.212767e+01<br />entity: Russia","year: 2003<br />mobile_subs: 2.499237e+01<br />entity: Russia","year: 2004<br />mobile_subs: 5.118013e+01<br />entity: Russia","year: 2005<br />mobile_subs: 8.355488e+01<br />entity: Russia","year: 2006<br />mobile_subs: 1.051290e+02<br />entity: Russia","year: 2007<br />mobile_subs: 1.195947e+02<br />entity: Russia","year: 2008<br />mobile_subs: 1.394450e+02<br />entity: Russia","year: 2009<br />mobile_subs: 1.607698e+02<br />entity: Russia","year: 2010<br />mobile_subs: 1.660376e+02<br />entity: Russia","year: 2011<br />mobile_subs: 1.422211e+02<br />entity: Russia","year: 2012<br />mobile_subs: 1.450733e+02<br />entity: Russia","year: 2013<br />mobile_subs: 1.520227e+02<br />entity: Russia","year: 2014<br />mobile_subs: 1.537481e+02<br />entity: Russia","year: 2015<br />mobile_subs: 1.579618e+02<br />entity: Russia","year: 2016<br />mobile_subs: 1.591546e+02<br />entity: Russia","year: 2017<br />mobile_subs: 1.578875e+02<br />entity: Russia"],"type":"scatter","mode":"lines","line":{"width":3.77952755905512,"color":"rgba(0,191,196,1)","dash":"solid"},"hoveron":"points","name":"Russia","legendgroup":"Russia","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017],"y":[2.09205086366983,2.96388013123759,4.28532526240106,6.15700697945043,9.18561207022721,12.7176870362786,16.384832955104,20.3251980657314,25.1174664993638,30.8564514088078,38.8243678484507,45.1110835155321,49.3205645290249,55.3867930869923,63.1774892595294,69.0205483727633,77.0916423137437,82.9354629527902,86.1312908462937,89.6126045826433,92.3784068871048,95.6125019258475,97.2880745755963,98.4665250134029,111.891403183316,119.497390487961,122.875872294896,122.012468472834],"text":["year: 1990<br />mobile_subs: 2.092051e+00<br />entity: United States","year: 1991<br />mobile_subs: 2.963880e+00<br />entity: United States","year: 1992<br />mobile_subs: 4.285325e+00<br />entity: United States","year: 1993<br />mobile_subs: 6.157007e+00<br />entity: United States","year: 1994<br />mobile_subs: 9.185612e+00<br />entity: United States","year: 1995<br />mobile_subs: 1.271769e+01<br />entity: United States","year: 1996<br />mobile_subs: 1.638483e+01<br />entity: United States","year: 1997<br />mobile_subs: 2.032520e+01<br />entity: United States","year: 1998<br />mobile_subs: 2.511747e+01<br />entity: United States","year: 1999<br />mobile_subs: 3.085645e+01<br />entity: United States","year: 2000<br />mobile_subs: 3.882437e+01<br />entity: United States","year: 2001<br />mobile_subs: 4.511108e+01<br />entity: United States","year: 2002<br />mobile_subs: 4.932056e+01<br />entity: United States","year: 2003<br />mobile_subs: 5.538679e+01<br />entity: United States","year: 2004<br />mobile_subs: 6.317749e+01<br />entity: United States","year: 2005<br />mobile_subs: 6.902055e+01<br />entity: United States","year: 2006<br />mobile_subs: 7.709164e+01<br />entity: United States","year: 2007<br />mobile_subs: 8.293546e+01<br />entity: United States","year: 2008<br />mobile_subs: 8.613129e+01<br />entity: United States","year: 2009<br />mobile_subs: 8.961260e+01<br />entity: United States","year: 2010<br />mobile_subs: 9.237841e+01<br />entity: United States","year: 2011<br />mobile_subs: 9.561250e+01<br />entity: United States","year: 2012<br />mobile_subs: 9.728807e+01<br />entity: United States","year: 2013<br />mobile_subs: 9.846653e+01<br />entity: United States","year: 2014<br />mobile_subs: 1.118914e+02<br />entity: United States","year: 2015<br />mobile_subs: 1.194974e+02<br />entity: United States","year: 2016<br />mobile_subs: 1.228759e+02<br />entity: United States","year: 2017<br />mobile_subs: 1.220125e+02<br />entity: United States"],"type":"scatter","mode":"lines","line":{"width":3.77952755905512,"color":"rgba(199,124,255,1)","dash":"solid"},"hoveron":"points","name":"United States","legendgroup":"United States","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":25.5707762557078,"l":28.4931506849315},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"How Have the Number of Fixed Mobile Subcriptions (per 100 people)<br />  Changed Over the Last 30 Years?","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[1988.65,2018.35],"tickmode":"array","ticktext":["1990","2000","2010"],"tickvals":[1990,2000,2010],"categoryorder":"array","categoryarray":["1990","2000","2010"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-8.30187914795375,174.339462107029],"tickmode":"array","ticktext":["0","50","100","150"],"tickvals":[0,50,100,150],"categoryorder":"array","categoryarray":["0","50","100","150"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.93503937007874},"annotations":[{"text":"entity","x":1.02,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"left","yanchor":"bottom","legendTitle":true}],"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"102f45d142758":{"x":{},"y":{},"colour":{},"type":"scatter"}},"cur_data":"102f45d142758","visdat":{"102f45d142758":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).


```r
train_anim <- small_trains %>%
  mutate(month = factor(month.abb[month], levels = month.abb)) %>% 
  ggplot(aes(x = month, y = num_arriving_late)) +
  geom_col() +
  transition_states(year)+
  labs(title = "Number of Late Arriving Trains by Month", 
       subtitle = "Year: {next_state}",
       x = "",
       y = "")

anim_save("train.gif", train_anim)
```


```r
knitr::include_graphics("train.gif")
```

![](train.gif)<!-- -->


## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each vegetable and arranged (HINT: `fct_reorder()`) from most to least harvested (most on the bottom).  
  * Add animation to reveal the plot over date. 

I have started the code for you below. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0.


```r
garden_anim <- garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(date, variety) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>% 
  ungroup() %>% 
  complete(variety, date, fill = list(daily_harvest_lb = 0)) %>% 
  group_by(variety) %>% 
  mutate(total = cumsum(daily_harvest_lb)) %>%
  ungroup() %>% 
  mutate(variety = fct_reorder(variety, total, max)) %>% 
  ggplot(aes(x = date, y = total))+
  geom_area(aes(fill = variety))+
  transition_reveal(date)+
  labs(title = "Total Tomato Harvest",
       x = "",
       y = "weight (lb)")

anim_save("garden.gif", garden_anim)
```


```r
knitr::include_graphics("garden.gif")
```

![](garden.gif)<!-- -->


## Maps, animation, and movement!

  4. Map my `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.


```r
mallorca_map <- get_stamenmap(
    bbox = c(left = 2.382, bottom = 39.575, right = 2.616, top = 39.7), 
    maptype = "terrain",
    zoom = 13
)
bike_anim <- ggmap(mallorca_map) +
  geom_path(data = mallorca_bike_day7, 
             aes(x = lon, y = lat, color = ele),
             size = 1.5) +
  scale_color_viridis_c() +
  geom_point(data = mallorca_bike_day7, 
             aes(x = lon, y = lat),
             color = "red", size = 2)+
  theme_map() +
  labs(title = "Mallorca Bike Ride", 
       subtitle = "Time: {frame_along}",
       color = "elevation")+
  theme(legend.background = element_blank())+
  transition_reveal(along = time)

anim_save("bike.gif", bike_anim)
```


```r
knitr::include_graphics("bike.gif")
```

![](bike.gif)<!-- -->

I really enjoy this animation compared to the static map because I can actually watch the bike ride!


  5. In this exercise, you get to meet my sister, Heather! She is a proud Mac grad, currently works as a Data Scientist at 3M where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files (HINT: `bind_rows()`, 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
ironman <- bind_rows(panama_swim, panama_run, panama_bike)

panama_map <- get_stamenmap(
    bbox = c(left = -79.6428, bottom = 8.8755, right = -79.4392, top = 8.9940), 
    maptype = "terrain",
    zoom = 13
)

ironman_anim <- ggmap(panama_map) +
  geom_path(data = ironman,
             aes(x = lon, y = lat, color = event),
             size = 1.5) +
  geom_point(data = ironman,
             aes(x = lon, y = lat, color = event),
             size = 1.5) +
  theme_map() +
  labs(title = "Panama Ironman 70.3 Pan Am Championships", 
       subtitle = "",
       color = "")+
  theme(legend.background = element_blank())+
  transition_reveal(time)

anim_save("ironman.gif", ironman_anim)
```


```r
knitr::include_graphics("ironman.gif")
```

![](ironman.gif)<!-- -->
  
## COVID-19 data

  6. In this exercise, you are going to replicate many of the features in [this](https://aatishb.com/covidtrends/?region=US) visualization by Aitish Bhatia but include all US states. Requirements:
 * Create a new variable that computes the number of new cases in the past week (HINT: use the `lag()` function you've used in a previous set of exercises). Replace missing values with 0's using `replace_na()`.  
  * Filter the data to omit rows where the cumulative case counts are less than 20.  
  * Create a static plot with cumulative cases on the x-axis and new cases in the past 7 days on the x-axis. Connect the points for each state over time. HINTS: use `geom_path()` and add a `group` aesthetic.  Put the x and y axis on the log scale and make the tick labels look nice - `scales::comma` is one option. This plot will look pretty ugly as is.
  * Animate the plot to reveal the pattern by date. Display the date as the subtitle. Add a leading point to each state's line (`geom_point()`) and add the state name as a label (`geom_text()` - you should look at the `check_overlap` argument).  
  * Use the `animate()` function to have 200 frames in your animation and make it 30 seconds long. 
  * Comment on what you observe.
  

```r
covid19_anim <- covid19 %>% 
  group_by(state) %>% 
  mutate(lag_7day = lag(cases, 7, order_by = date)) %>% 
  replace_na(list(lag_7day = 0)) %>%
  mutate(new_cases = cases - lag_7day) %>% 
  filter(cases >= 20) %>% 
  ggplot()+
  geom_path(aes(x = cases, y = new_cases, group = state), color = "blue")+
  scale_x_log10(label = scales::comma)+
  scale_y_log10(label = scales::comma)+
  geom_point(aes(x = cases, y = new_cases, group = state))+
  geom_text(aes(x = cases, y = new_cases, group = state, label = state))+
  labs(title = "New Covid Cases by Cumulative Cases Over Time",
       subtitle = "Date: {frame_along}",
       x = "",
       y = "")+
  transition_reveal(date)

  animate(covid19_anim, nframes = 200, duration = 30)
  
anim_save("covid.gif", covid19_anim)
```



```r
knitr::include_graphics("covid.gif")
```

![](covid.gif)<!-- -->

This animation shows me that COVID cases in the US have grown exponentially in every state as time has progressed, really taking off in April and continuing to rise even now. California, Florida, and Texas have had up to 100,000 new cases in a week and almost every state has cumulatively around 500,000 to 1,000,000 cases.

  
  7. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. Put date in the subtitle. Comment on what you see. The code below gives the population estimates for each state. Because there are so many dates, you are going to only do the animation for all Fridays. So, use `wday()` to create a day of week variable and filter to all the Fridays. HINT: use `group = date` in `aes()`.


```r
states_map <- map_data("state")

census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))

covidmap <- covid19 %>%
  mutate(state = str_to_lower(state)) %>% 
  left_join(census_pop_est_2018,
            by = c("state" = "state")) %>% 
  mutate(per_capita = (cases/est_pop_2018)*10000,
         weekday = wday(date, label = TRUE)) %>%
  filter(weekday == "Fri") %>% 
  ggplot()+
  geom_map(map = states_map,
           aes(map_id = state,
               group = date,
               fill = per_capita))+
  expand_limits(x = states_map$long, y = states_map$lat)+
  labs(title = "Most Recent Cumulative Number of COVID-19 Cases Per 10000 People",
       subtitle = "Date: {next_state}")+
  theme_map()+
  theme(legend.background = element_blank())+
  transition_states(date)

anim_save("covidmap.gif", covidmap)
```


```r
knitr::include_graphics("covidmap.gif")
```

![](covidmap.gif)<!-- -->


## Your first `shiny` app (for next week!)

NOT DUE THIS WEEK! If any of you want to work ahead, this will be on next week's exercises.

  8. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' cumulative number of COVID cases over time. The x-axis will be number of days since 20+ cases and the y-axis will be cumulative cases on the log scale (`scale_y_log10()`). We use number of days since 20+ cases on the x-axis so we can make better comparisons of the curve trajectories. You will have an input box where the user can choose which states to compare (`selectInput()`) and have a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
[github link](https://github.com/wbrazgel/Exercise5)

  9. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 05_exercises.Rmd, provide a link to the 05_exercises.md file, which is the one that will be most readable on GitHub. If that file isn't very readable, then provide a link to your main GitHub page.


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
