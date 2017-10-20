
## **lubridate** 简介

lubridate 包是 Hadley 开发的用于高效处理时间数据的 R 包。

### 关于时区、UTC和GMT的说明

为克服国际时间的混乱，1884年的国际经度会议规定，以英国格林尼治（Greenwich）天文台的经线为零度经线（即本初子午线），将全球划分为24个时区，每个时区都有自己的本地时间，同时提倡使用标准时间，即格林尼治标准时间，简称GMT（Greenwich Mean Time）。理论上讲，格林尼治标准时间的正午是指当太阳横穿格林尼治子午线时的时间。但由于地球自转并不完全规则，故这一时间与实际的太阳时可能差到16分钟。为调整误差，自1924年2月5日开始，格林尼治天文台每隔一小时会向全世界发放调时信息。为进一步协调时间误差，1972年1 月1日，精度更高的协调世界时（Universal Time Coordinated，简称UTC）成为新的世界标准时间，广泛应用于国际无线电通信场合。但为了方便，在不需要精确到秒的情况下，通常也将GMT和UTC视为等同。通常所谓的GMT手表，仅指显示两个或两个以上时区时间的手表，并不涉及时间确定方式。

时区编码通常采用`国家名（或大洲名）/城市名`的方式列示，如`America/Chicago`、`Asia/Tokyo`。中国采用北京所在地东八区的时间为全国统一使用时间，称为北京时间，或称中国标准时间（China Standard Time ，简称CST）。北京时区是东八区，领先UTC八个小时，在电子邮件信头的Date域记为+0800。

但在协调世界时的国际时区代码中，并不使用`Asia/Beijing`作为标记，而使用`Asia/Shanghai`。这也是 lubridate 包中`tz = `参数中使用北京时间的标记方式。

### 日期格式转化


```{r}
library(lubridate)
raw_time1 <- "2017/9/12  12:47:28"
```

在`raw_time1`中，时间的显示方式为“年/月/日 时：分：秒”。其中，年、月、日之间用`/`分隔，日与时之间用空格分隔，时、分、秒之间用`:`分隔。这会给数据处理带来不变。lubridate 包的一大优势就在于它可自动识别多种分隔符，并按人类自然理解的方式进行操纵。下面使用`ymd_hms()`函数进行格式转化，其中`tz`表示设置时区（time zone）。

```{r}
r_time1 <- ymd_hms(raw_time1, tz = "Asia/Shanghai")
r_time1
```
此时数据已转为R中的标准日期格式，`CST`表示中国标准时间（即北京时间），具体参见本节关于时区的说明。


若时间中只包含日期、时、分的信息，可使用`ymd_hm()`函数。

```{r}
raw_time2 <- "20170412 12:45"
r_time2 <- ymd_hm(raw_time2, tz = "Asia/Shanghai")
```

若包含日期与时的信息，则可用`ymd_h()`函数。这里不再举例。

使用`ymd()`或`ydm()`可将“年月日”或“年日月”日期格式转为 R 中的标准格式。
```{r}
raw_time3 <- "20170405"
ymd(raw_time3)
ydm(raw_time3)
```
类似地，`dmy()`和`mdy()`也可转化相应的日期格式。


### 时刻信息提取

对符合指定格式的日期数据，lubridate 包中的函数可方便地提取日期（年月日）、年、月、日、时、分、秒、时区、周几等信息，这主要涉及`date()`、`year()`、`month()`、`day()`、`hour()`、`minute()`、`second()`、`tz()`和`wday()`等函数。


```{r}
date(r_time1)
year(r_time1)
month(r_time1)
day(r_time1)
hour(r_time1)
minute(r_time1)
second(r_time1)
tz(r_time1)
```

使用`wday()`可给出指定的日子是周几，周日至周六依次编码为整数1-7。也可设置参数`label = TRUE`显示Sunday至Saturday的字母缩写。

```{r}
wday(r_time1)
wday(r_time1, label = TRUE)
```


### 时间长度计算


```{r}
library(readxl)
library(dplyr)
library(tibble)
library(lubridate)
time_data <- read_excel("PDSurveyBasic.xlsx") %>% select(time1) %>% as_tibble() 
head(time_data)
```


```{r, eval=FALSE}
library(readxl)
library(tibble)
library(tidyverse)
library(stringr)
PDSurveyBasic <- read_excel("PDSurveyBasic.xlsx")
time2 <- PDSurveyBasic$time2
ip.location <- str_extract(PDSurveyBasic$ip, "(?<=\\().*(?=\\))") %>% 
  str_split_fixed("-", n = 2) %>%
  as_tibble %>%
  transmute(province = .[[1]], city = .[[2]]) %>% 
  group_by(city) %>% 
  summarise(n = n()) %>% 
  arrange(desc(n))
```



