---
title: "Penalty champions in the Big Five leagues 2014/14-2018/19"
author: "@wiscostretford"
date: "2019-04-22"
output: 
  html_document:
    keep_md: yes
---

This is a super short code-through to illustrate 'penalty champions' in the top football leagues.

Data-wise, I will use data from understat, which provides extensive player-level statistics. Here, I want to look at the ratio of goals to non-penalty xG.

The data set looks like so:




```r
head(usfull)
```

```
##   X  id    player_name games time goals        xG assists       xA shots
## 1 1 619  Sergio Agüero    33 2551    26 25.270160       8 5.568922   148
## 2 2 647     Harry Kane    34 2589    21 17.157292       4 3.922501   112
## 3 3 802    Diego Costa    26 2111    20 15.219104       3 4.554671    76
## 4 4 848 Charlie Austin    35 3078    18 17.881850       5 2.548747   131
## 5 5 498 Alexis Sánchez    35 2967    16 13.451750       8 8.494180   122
## 6 6 502 Olivier Giroud    27 1871    14  8.846344       3 3.860136    70
##   key_passes yellow_cards red_cards position          team_title npg
## 1         33            4         0      F S     Manchester City  21
## 2         27            4         0    F M S           Tottenham  19
## 3         41            8         0      F S             Chelsea  19
## 4         23            4         1        F Queens Park Rangers  15
## 5         82            4         0    F M S             Arsenal  16
## 6         29            5         1      F S             Arsenal  14
##        npxG  xGChain xGBuildup league year
## 1 20.703184 27.80515  6.878173    EPL 2014
## 2 14.873823 16.48844  5.549699    EPL 2014
## 3 14.457935 21.36579  5.276973    EPL 2014
## 4 14.076043 13.71828  3.041321    EPL 2014
## 5 12.690581 27.15757 10.736753    EPL 2014
## 6  8.846344 13.63988  4.129247    EPL 2014
```

The simple code is below. It does the following:

* Summarizes, at the player level, total goals and non-penalty xG
* Calculates the ratio of interest (goals to non-penalty xG)
* Filters for players with more than 4 goals in total
* Arranges by that ratio in descending order
* Shows the top20


```r
library(dplyr)
```


```r
usfull %>% 
  select(player_name,team_title,goals,npxG) %>% 
  group_by(player_name) %>%
  summarize(totalgoals = sum(as.numeric(goals)), totalnpxg = sum(as.numeric(npxG))) %>% 
  mutate(ratio = totalgoals / totalnpxg) %>% 
  filter(totalgoals > 4) %>% 
  arrange(-ratio) %>% 
  slice(1:20)
```

```
## # A tibble: 20 x 4
##    player_name       totalgoals totalnpxg ratio
##    <fct>                  <dbl>     <dbl> <dbl>
##  1 Paul Verhaegh             17      1.79  9.52
##  2 Luka Milivojevic          24      4.17  5.76
##  3 Erick Pulgar              10      2.11  4.73
##  4 Antonin Bobichon           6      1.48  4.05
##  5 Kenny Lala                 7      1.79  3.92
##  6 Téji Savanier              6      1.56  3.84
##  7 Leighton Baines            8      2.23  3.59
##  8 Fabinho                   24      6.96  3.45
##  9 Verza                      6      1.79  3.35
## 10 Mark Noble                19      5.82  3.26
## 11 Vincent Koziello           5      1.65  3.03
## 12 Ricardo Rodríguez         11      3.71  2.96
## 13 Issiar Dia                 8      2.71  2.95
## 14 Maxwell                    6      2.08  2.88
## 15 Lucas Biglia              13      4.51  2.88
## 16 Roberto Torres             7      2.44  2.87
## 17 Mohamed Larbi              8      2.81  2.84
## 18 Steven Gerrard             9      3.30  2.72
## 19 Jordan Veretout           23      8.50  2.71
## 20 Eugen Polanski             6      2.23  2.69
```
