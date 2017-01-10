# 臺灣大學用電狀況分析與節電措施建議

### 第三組：王怡文、李碩儒、李宸馨、林芝伃、倪昆恩、傅子晏

## ※研究動機：

據媒體報導，臺灣大學每年電費達4億元，遠高於全台其它大學。近年來校方已有提出節電政策，然而我們好奇校園內各建物在推行節電政策情形，同時也希望身為台大學生能夠為天天接觸而容易被遺忘的議題盡一份心力，因此做出下列研究並提出建議。

## ※研究方法：透過R分析總務處提供的數據資料、進行口頭探訪和實地考察，試圖解答我們對於用電現狀的眾眾多疑問。
【前言】
在經過與總務處營繕組的交涉之後，我們取得了台大98至104年全校建築物的用電量以及電費等資料，於是我們將104年電量計費全年度的總用電量讀入R進行資料分析，並且找出全年度合計用電量前十五以及前三十多的建築物。

經過計算，全年合計用電量前十五名的建築物加總起來佔台大全年總用電量的55.43%，前三十名的建築物則佔69.81%；我們不禁好奇，耗電量這麼大的建築物究竟分布在校園的哪些區域呢？

Hide
library(readxl)
X104u_ed <- read_excel("~/dsR/data/104u_ed.xlsx")
X104u_ed <- X104u_ed[,-c(3,4,5,6,7,8,9,10,11,12,13,14,15)]

#[數字]用電量前15棟/30棟系館合計用電佔全校總用電量比例
sum(X104u_ed$`104年合計用電`[1:15])/sum(X104u_ed$`104年合計用電`)*100
sum(X104u_ed$`104年合計用電`[1:30])/sum(X104u_ed$`104年合計用電`)*100
我們在這張圖可以看到，前三十名的建築物皆位於校總區，而且大多數的建築物位於學校的後半部，也就是較接近辛亥門口的這些建築。名列前茅的建築物非常直覺地，就是可以容納最多學生的總圖書館，緊追在後的則大多是有實驗室在內的建築物。

Hide
#[圖]全校各系館標示
library(dplyr)
library(leaflet)
buildings <- read.csv(textConnection("
Building,Lat,Long,Volume
總圖書館,25.017366,121.540964,7.339184
凝態科學館,25.021772,121.536599,6.977391
積學館(新+舊),25.019307,121.538625,6.738286
天文數學館,25.021261,121.538258,5.70878
生命科學館,25.015500,121.539420,5.417377
電機二館(電機館及工圖館),25.018667,121.542116,4.809687
原分所(舊),25.018676,121.538329,4.625896
資訊大樓,25.020984,121.539785,4.409367
體育館,25.021568,121.534487,4.406833
工學院綜合大樓,25.018898,121.540954,3.507541
應用力學研究大樓,25.019860,121.541393,1.769137
二號館(物理系館),25.016893,121.535985,1.734416
博理館,25.019264,121.542468,1.699933
地質館(後棟),25.014563,121.537872,1.654443
化學工程館,25.018166,121.538963,1.470924
電機一館[舊電機館(原稱電機工程研究所),25.019047,121.538685,1.227423
人工控制氣候室,25.013650,121.539250,1.218567
資訊工程館(資訊系館),25.019668,121.541586,1.202483
家畜醫院新大樓,25.015632,121.543677,1.163429
國青大樓,25.020357,121.544382,1.145545
第二活動中心,25.013057,121.536930,1.130643
土木研究大樓,25.017930,121.547616,1.126754
畜產系畜牧大樓(動物科學技術學系),25.012659,121.544567,1.0611
活大,25.017750,121.540404,1.035598
浩瀚樓,25.014786,121.538421,1.007872
法學院(霖澤、萬才),25.020554,121.543677,1.006557
海洋研究所,25.021289,121.537716,1.003628
明達館,25.017912,121.544841,0.974453
管理學院一號館(管理學院),25.014039,121.538644,0.943276
語文大樓,25.020545,121.541293,0.901123
"))

leaflet(buildings) %>% addTiles() %>% addMarkers(
  clusterOptions = markerClusterOptions()
)
那麼這些建築物的用電量是多麼驚人呢？我們能夠從以下這張地圖看出一些端倪。每一個圓圈以「百萬度」為單位，圓圈大小表示用電量的多寡；以總圖書館為例，104年的全年用電量為733萬度，以此類推。

Hide
#[圖]全校用電量前三十系館(以圓圈表示大小)
buildings <- read.csv(textConnection("
Building,Lat,Long,Volume
總圖書館(用電量733萬度),25.017366,121.540964,7.339184
凝態科學館(用電量697萬度),25.021772,121.536599,6.977391
積學館(用電量673萬度),25.019307,121.538625,6.738286
天文數學館(用電量570萬度),25.021261,121.538258,5.70878
生命科學館(用電量541萬度),25.015500,121.539420,5.417377
電機二館(用電量480萬度),25.018667,121.542116,4.809687
原分所(用電量462萬度),25.018676,121.538329,4.625896
資訊大樓(用電量441萬度),25.020984,121.539785,4.409367
體育(用電量441萬度)館,25.021568,121.534487,4.406833
工學院綜合大樓(用電量351萬度),25.018898,121.540954,3.507541
應用力學研究大樓(用電量177萬度),25.019860,121.541393,1.769137
物理系二號館館(用電量173萬度),25.016893,121.535985,1.734416
博理館(用電量169萬度),25.019264,121.542468,1.699933
地質館(用電量165萬度),25.014563,121.537872,1.654443
化學工程館(用電量147萬度),25.018166,121.538963,1.470924
電機一館(用電量123萬度),25.019047,121.538685,1.227423
人工控制氣候室(用電量122萬度),25.013650,121.539250,1.218567
資訊工程館(用電量120萬度),25.019668,121.541586,1.202483
家畜醫院新大樓(用電量116萬度),25.015632,121.543677,1.163429
國青大樓(用電量114萬度),25.020357,121.544382,1.145545
第二活動中心(用電量113萬度),25.013057,121.536930,1.130643
土木研究大樓(用電量113萬度),25.017930,121.547616,1.126754
畜產系畜牧大樓(用電量106萬度),25.012659,121.544567,1.0611
活大(用電量104萬度),25.017750,121.540404,1.035598
浩瀚樓(用電量101萬度),25.014786,121.538421,1.007872
法學院(霖澤、萬才)(用電量101萬度),25.020554,121.543677,1.006557
海洋研究所(用電量100萬度),25.021289,121.537716,1.003628
明達館(用電量97萬度),25.017912,121.544841,0.974453
管理學院一號館(用電量94萬度),25.014039,121.538644,0.943276
語文大樓(用電量90萬度),25.020545,121.541293,0.901123
"))

n <- leaflet(buildings) %>% addTiles() %>%
  addCircles(lng = ~Long, lat = ~Lat, weight = 1,
    radius = ~sqrt(Volume) * 30, popup = ~Building
  )
n
誠如前述，這三十棟建築物中除了總圖書館這棟「開放時間長、學生容納量最高」的建築以外，其餘以「包含實驗室」的建築物為多；然而，台大校內仍有各式各樣型態的建築物，並且我們猜測，不同類型建築物的使用者應當有不一樣的使用方式與習慣，因此接下來的分析中,我們將把所有建築物去除掉總圖書館後，歸類為「共同教學類」「實驗室型系館」以及「學生宿舍類」來進行用電狀況的分析。

【共同教學類】
(子晏、芝伃)

※研究標的：新生教學大樓、博雅教學大樓、普通教學大樓

※研究目的：

由於教學館並無包含需長期供電的實驗室機電設備，場地主要為學生上課所用，我們認為若要訂立校園節電政策，可由校內教學館切入。

※研究方法：

我們挑選三棟與我們息息相關的教學大樓進行用電分析，分別是「博雅教學館、普通教學館及新生教學館」。以R程式畫出近4年來總用電趨勢圖、月用電量趨勢圖、日用電量趨勢圖，以分析三棟教學館歷年以來用電趨勢，觀察近年來校方節電政策之效果。(以下統計中博雅教學館用電不包含統計、寫作、教學發展中心，只採用教室區與公共區的用電量，來比較三棟大樓用電情形。)

Hide
require(ggplot2)
library(ggplot2)
data_1<-read.csv("~/dsR/data/building_v3.csv",header=T)
ggplot(data=data_1) +   
  geom_line(aes(x=data_1$`年份`,  
                y=data_1$`總用電量`,
                color=data_1$`建物`) ) +
  labs(title="教學大樓過去3年用電量分析",
       x="年份",
       y="用電") +theme_bw()
由上圖可明顯看出過去4年以來，三棟教學大樓中博雅總用電量最多之後依序為普通和新生，符合教室間數的關係，即教室間數越多耗電量越大，因此為排除此影響我們接下來以EUI來作分析比較。

Hide
data_2<-read.csv("~/dsR/data/building_eui.csv",header=T)
ggplot(data=data_2) +   
  geom_line(aes(x=data_2$年份,  
                y=data_2$EUI,
                color=data_2$建物) ) +
  labs(title="教學大樓過去3年用電量EUI分析",
       x="年份",
       y="EUI") +theme_bw()
101年下半年度博雅館才正式啟用，102年度博雅的EUI非常高，隔年跌至谷底，近兩年來卻又持續攀升。三大教學館不管是年、月、日用電量都有相似趨勢，與學生課表、教室使用狀況相關，但在EUI方面則有一些特別，博雅雖然是新啟用的教學館，但用電效率並無想像中的高，我們將進一步訪談分析為何有此情形發生。

Hide
data_3<-read.csv("~/dsR/data/building_monthly.csv",header=T)
ggplot(data_3, aes(x = factor(月份), y = 月用電量, fill = 建物)) + 
  geom_bar(stat = "identity", position = "dodge") + 
  labs(y = "月用電量") + labs(x = "月份")
三棟大樓每月變動趨勢大致相似，104年用電量最高的是六月，最低為二月。因為夏天冷氣使用量高所以用電量大幅提高，然而七八月雖然是夏天，但因為暑假的緣故用電量反而不高，九月及十月開學後天氣依舊炎熱，用電量也很高。進入冬天後用電開始大幅下滑，甚至只有夏天的一半，二月是一整年用電量最低的月份，因為適逢寒假及過年期間，大樓使用率極低。

Hide
data_4<-read.csv("~/dsR/data/building_june.csv",header=T)
ggplot(data=data_4) +   
  geom_line(aes(x=data_4$date,  
                y=data_4$月用電量,
                color=data_4$建物) ) +
  labs(title="104年6月用電量",
       x="日期",
       y="用電量(度)") +theme_bw()
接著我們選取年度用電量最高的六月份進行分析，可以看到明顯呈週期性變化，低點都發生在假日。在假日沒有上課的時候，普通和新生用電量都非常低，但博雅明顯高出許多，我們也將進一步討論此現象。

Hide
data_5<-read.csv("~/dsR/data/building_Feb.csv",header=T)
ggplot(data=data_5) +   
geom_line(aes(x=data_5$date,y=data_5$月用電量,color=data_5$建物) ) +
  labs(title="104年2月用電量",x="日期",y="用電量(度)") +theme_bw()
104年最低用電量的月份為二月，寒假期間用電量都很低，也沒有像六月有明顯周期性的變化，可以看到最低的時段落在18-22號正值農曆過年期間，2月24日開學後用電量瞬間升高。

[研究結論]
採訪教務處博雅教學館管理員：蘇成元、蔡孟寰

博雅為三者中最晚啟用的大樓，設備應耗電量最低，為何EUI卻最高? 原因為博雅不同於普通和新生是使用水冷式空調，原設計目的在於符合大教室使用，但只要有一間教室使用空調主機就必須運作，再加上博雅當初無設計通風系統(窗戶少)，主機不分季節只要開館時間幾乎就啟用運作，耗電量極高，因此雖極力推行省電政策，用電量仍持續攀升，同時也因為教室內位置皆裝設插座而使博雅EUI偏高。另外博雅有設置液晶螢幕持續播放，也造成用電量上升。

近兩年博雅用電量為何持續增加? 教室租借增加為電費提升的主因。104年活動租借同比提高50%，電費也隨之上升30%， 雖然學校租借場地有收取空調費用，但水冷式空調的耗電仍造成整體電量持續增加。

新生教學館面臨的問題 新生教學館為中央空調系統，然當初設計時有一些缺陷，空調設備磅數不夠，導致冷氣常有不夠冷的問題，教室坪數不大應可使用分離式冷氣，不會造成主機耗能。因為排課問題有時單間教室使用人數少，空調系統造成耗電問題嚴重，不符成本。另外晚間提供學生社團使用，然而向學生索取的費用遠小於實際用電耗費及場地維護費，且此筆收費由活大及課務組依比例均攤，因此營繕組對於新生大樓的電費未能得到補償。新生教室座位設有插座，導致EUI較普通高。

【實驗室型系館】
(碩儒、Ken)

※研究標的：博理館（電機系）、明達館（電機系）、積學館（化學系）

※研究歷程：

起初，我們看著資料中提供的六大項目，分別為 (1)合計用電 (2)興建日期
(3)高壓低壓
(4)自負比例
(5)EUI
(6)功率因數（％）

我們試圖透過「合計用電」和「興建日期」對建築物進行分類，想了解是否較落成年份較早的建物會因為設備老舊的原因而造用電的不效率。不幸的是，我們透過這種方法挑選出來的所有建築物都要面臨即將被拆除、正在拆除中，或是由於建物太老舊，已無完整資料得以分析。

於是我們決定忽略「興建日期」，轉往「電費自付比例」和「EUI」去探索，然而由於學校對於絕大多數的建築物皆施以相同的22%的電費自付比例，因此很難理解自付比例是否影響用電量。

緊接著我們看到電力輸送的方式，便馬上注意到天文數學館有別於其他建築物以高壓傳輸，竟是使用低壓。因此我們到天文數學館進行訪問。

[天文數學館]
（據管理員說法，天數館的用電狀況適用於凝態物理館等實驗室較多的建築物） -開放時間：07:00-21:00；21:00後僅能刷卡進入館區 -電力調控單位由以下四個單位每年輪流：中研院數學研究院、中研院天文研究院、台大數學系、台大天文研究所 -電費由上述四個單位依某公式計算所得出的比例進行分攤

Hide
#【訪談內容整理】

#Q1. 為何天文數學館的用電是以低壓方式輸送？
#A1. 「低壓」為誤植資訊，正確資訊為「高壓」輸送。高壓輸送意味著(1)此區用電符合規模經濟 (2)此建築物必有特殊需求，如高耗電之實驗室等等。然而天數館周圍樓層數量較少的建築物有部分為「相對低壓」輸送，意即由周圍高壓輸送的建築物再分流過來的輸送方式。

#Q2. 對於天文數學館而言，哪些儀器或設備佔了較多的用電量？
#A2.最耗電的設備依序為：冷凍空調設備→實驗室→電梯；我們原先猜測的燈具反而是佔相對小很多的耗電量。

#Q3.目前依照學校政策所進行的節能措施有哪些？就此政策是否仍有可以再改善的空間？
#A3.將舊式燈具換做LED燈，每小時僅能省下16瓦的電，然而每節省1,000瓦才能省下（1度電*NT$3）的電費，且學校在汰換設備的過程中早已逐步將傳統的T8燈管改為T5燈管，因此再由燈具下手所達到的節電效率並不高。（NT$3為全年均單位電費）
再來，「功率因數」是我們最初不了解的類別，然而在與能源工程教授訪談之後，我們了解：

功率因數=（實際使用的電量）%（台電簽約的供電量）

對於建築物或大型單位而言，通常會依據使用電量的估算，跟台電簽約以保障供電量（也方便供電方去計算需要輸送多少電，電網的負載多少），稱為「契約容量」。但台電的計價方式會跟契約容量有關，也就是契約容量越高，基礎電費也越高；若超過100%，甚至會被罰款。所以從用電者的角度來講，功率因數越接近 100% 越好。通常若發現功率因數太低，就代表其實可以調降「契約容量」，來達到節省電費的目的。

大多數建築物功因約為90％；然而上述所提到的學生宿舍部分，我們可以發現一些宿舍的功率因數只有20％。

最後，我們決定把注意力挪回合計用電上，並就「一周內每天的用電情形」以及「一天內每小時的用電狀況」進行更深入的分析和探討。

Hide
df_BL<- read_excel("~/dsR/data/r-school/daily_PowerUsage/Boli_DailyPowerUsage.xlsx")
df_MD<- read_excel("~/dsR/data/r-school/daily_PowerUsage/Mingda_DailyPowerUsage.xlsx")
df_JX<- read_excel("~/dsR/data/r-school/daily_PowerUsage/Jixue_DailyPowerUsage.xlsx")
df_XS<- read_excel("~/dsR/data/r-school/daily_PowerUsage/Xinsheng_DailyPowerUsage.xlsx")
df_HX<- read_excel("~/dsR/data/r-school/daily_PowerUsage/Huaxue_DailyPowerUsage.xlsx")
df_WX<- read_excel("~/dsR/data/r-school/daily_PowerUsage/Wenxue_DailyPowerUsage.xlsx")
df_WC<- read_excel("~/dsR/data/r-school/daily_PowerUsage/Wancai_DailyPowerUsage.xlsx")
df_SJ<- read_excel("~/dsR/data/r-school/daily_PowerUsage/Shengji_DailyPowerUsage.xlsx")

df_BL$Date <- factor(df_BL$Date,levels= c("Monday","Tuesday", "Wednesday", "Thursday", "Friday", "Saturday","Sunday"))
df_MD$Date <- factor(df_MD$Date,levels= c("Monday","Tuesday", "Wednesday", "Thursday", "Friday", "Saturday","Sunday"))
df_JX$Date <- factor(df_JX$Date,levels= c("Monday","Tuesday", "Wednesday", "Thursday", "Friday", "Saturday","Sunday"))
df_XS$Date <- factor(df_XS$Date,levels= c("Monday","Tuesday", "Wednesday", "Thursday", "Friday", "Saturday","Sunday"))
df_HX$Date <- factor(df_HX$Date,levels= c("Monday","Tuesday", "Wednesday", "Thursday", "Friday", "Saturday","Sunday"))
df_WX$Date <- factor(df_WX$Date,levels= c("Monday","Tuesday", "Wednesday", "Thursday", "Friday", "Saturday","Sunday"))
df_WC$Date <- factor(df_WC$Date,levels= c("Monday","Tuesday", "Wednesday", "Thursday", "Friday", "Saturday","Sunday"))
df_SJ$Date <- factor(df_SJ$Date,levels= c("Monday","Tuesday", "Wednesday", "Thursday", "Friday", "Saturday","Sunday"))

# Dec 26,27,28,29, Oct 17,18,19,20. For taida added nov 7,8,9,10
df_JX2 <- read_excel("~/dsR/data/r-school/hourly_PowerUsage/jixue_HourlyPowerUsage.xlsx")
df_SJ2 <- read_excel("~/dsR/data/r-school/hourly_PowerUsage/Shengji_HourlyPowerUsage.xlsx")
df_WX2 <- read_excel("~/dsR/data/r-school/hourly_PowerUsage/Wenxue_HourlyPowerUsage.xlsx")
df_HX2 <- read_excel("~/dsR/data/r-school/hourly_PowerUsage/Huaxue_hourlypowerusage.xlsx")
df_BL2 <- read_excel("~/dsR/data/r-school/hourly_PowerUsage/Boli_HourlyPowerUsage.xlsx")
df_MD2 <- read_excel("~/dsR/data/r-school/hourly_PowerUsage/Mingda_HourlyPowerUsage.xlsx")
df_XS2 <- read_excel("~/dsR/data/r-school/hourly_PowerUsage/Xinsheng_HourlyPowerUsage.xlsx")
df_WC2 <- read_excel("~/dsR/data/r-school/hourly_PowerUsage/Wancai_HourlyPowerUsage.xlsx")
df_ALL <- read_excel("~/dsR/data/r-school/hourly_PowerUsage/Taida_HourlyPowerUsage.xlsx")


## get time factors in right order 
df_BL2$Time<- factor(df_BL2$Time,levels=c("00~01","01~02","02~03","03~04","04~05","05~06","06~07","07~08","08~09","09~10","10~11","11~12","12~13","13~14","14~15","15~16","16~17","17~18","18~19","19~20","20~21","21~22","22~23","23~24"))
df_SJ2$Time<- factor(df_SJ2$Time,levels=c("00~01","01~02","02~03","03~04","04~05","05~06","06~07","07~08","08~09","09~10","10~11","11~12","12~13","13~14","14~15","15~16","16~17","17~18","18~19","19~20","20~21","21~22","22~23","23~24"))
df_WX2$Time<- factor(df_WX2$Time,levels=c("00~01","01~02","02~03","03~04","04~05","05~06","06~07","07~08","08~09","09~10","10~11","11~12","12~13","13~14","14~15","15~16","16~17","17~18","18~19","19~20","20~21","21~22","22~23","23~24"))
df_HX2$Time<- factor(df_HX2$Time,levels=c("00~01","01~02","02~03","03~04","04~05","05~06","06~07","07~08","08~09","09~10","10~11","11~12","12~13","13~14","14~15","15~16","16~17","17~18","18~19","19~20","20~21","21~22","22~23","23~24"))
df<- factor(df_BL2$Time,levels=c("00~01","01~02","02~03","03~04","04~05","05~06","06~07","07~08","08~09","09~10","10~11","11~12","12~13","13~14","14~15","15~16","16~17","17~18","18~19","19~20","20~21","21~22","22~23","23~24"))
df_MD2$Time<- factor(df_MD2$Time,levels=c("00~01","01~02","02~03","03~04","04~05","05~06","06~07","07~08","08~09","09~10","10~11","11~12","12~13","13~14","14~15","15~16","16~17","17~18","18~19","19~20","20~21","21~22","22~23","23~24"))
df_XS2$Time<- factor(df_XS2$Time,levels=c("00~01","01~02","02~03","03~04","04~05","05~06","06~07","07~08","08~09","09~10","10~11","11~12","12~13","13~14","14~15","15~16","16~17","17~18","18~19","19~20","20~21","21~22","22~23","23~24"))
df_WC2$Time<- factor(df_WC2$Time,levels=c("00~01","01~02","02~03","03~04","04~05","05~06","06~07","07~08","08~09","09~10","10~11","11~12","12~13","13~14","14~15","15~16","16~17","17~18","18~19","19~20","20~21","21~22","22~23","23~24"))
df_ALL$Time<- factor(df_ALL$Time,levels=c("00~01","01~02","02~03","03~04","04~05","05~06","06~07","07~08","08~09","09~10","10~11","11~12","12~13","13~14","14~15","15~16","16~17","17~18","18~19","19~20","20~21","21~22","22~23","23~24"))

## place time factors into 3 bins: morning, schoolday, night
morning = c("02~03","03~04","04~05","05~06","06~07","07~08")
jx<-df_JX2 %>%
  filter(Time %in% morning) %>%
  mutate(Time = "02~08")
sj<-df_SJ2 %>%
  filter(Time %in% morning) %>%
  mutate(Time = "02~08")
wx<-df_WX2 %>%
  filter(Time %in% morning) %>%
  mutate(Time ="02~08")
hx<-df_HX2 %>%
  filter(Time %in% morning) %>%
  mutate(Time ="02~08")
md<-df_MD2 %>%
  filter(Time %in% morning) %>%
  mutate(Time = "02~08")
bl<-df_BL2 %>%
  filter(Time %in% morning) %>%
  mutate(Time = "02~08")
xs<-df_XS2 %>%
  filter(Time %in% morning) %>%
  mutate(Time = "02~08")
wc<-df_WC2 %>%
  filter(Time %in% morning) %>%
  mutate(Time = "02~08")
td<-df_ALL%>%
  filter(Time %in% morning) %>%
  mutate(Time = "02~08")

schoolday =  c("08~09","09~10","10~11","11~12","12~13","13~14","14~15","15~16","16~17","17~18")
jx1<-df_JX2 %>%
  filter(Time %in% schoolday) %>%
  mutate(Time = "08~18")
sj1<-df_SJ2 %>%
  filter(Time %in% schoolday) %>%
  mutate(Time = "08~18")
wx1<-df_WX2 %>%
  filter(Time %in% schoolday) %>%
  mutate(Time = "08~18")
hx1<-df_HX2 %>%
  filter(Time %in% schoolday) %>%
  mutate(Time = "08~18")
md1<-df_MD2 %>%
  filter(Time %in% schoolday) %>%
  mutate(Time = "08~18")
bl1<-df_BL2 %>%
  filter(Time %in% schoolday) %>%
  mutate(Time = "08~18")
xs1<-df_XS2 %>%
  filter(Time %in% schoolday) %>%
  mutate(Time = "08~18")
wc1<-df_WC2 %>%
  filter(Time %in% schoolday) %>%
  mutate(Time = "08~18")
td1<-df_ALL %>%
  filter(Time %in% schoolday) %>%
  mutate(Time = "08~18")

night <- c("18~19","19~20","20~21","21~22","22~23","23~24","00~01","01~02")
jx2<-df_JX2 %>%
  filter(Time %in% night) %>%
  mutate(Time = "18~02")  
sj2<-df_SJ2 %>%
  filter(Time %in% night) %>%
  mutate(Time = "18~02")
wx2<-df_WX2 %>%
  filter(Time %in% night) %>%
  mutate(Time = "18~02")
hx2<-df_HX2 %>%
  filter(Time %in% night) %>%
  mutate(Time = "18~02")
md2<-df_MD2 %>%
  filter(Time %in% night) %>%
  mutate(Time = "18~02")  
bl2<-df_BL2 %>%
  filter(Time %in% night) %>%
  mutate(Time = "18~02")
xs2<-df_XS2 %>%
  filter(Time %in% night) %>%
  mutate(Time = "18~02")
wc2<-df_WC2 %>%
  filter(Time %in% night) %>%
  mutate(Time = "18~02")
td2<-df_ALL %>%
  filter(Time %in% night) %>%
  mutate(Time = "18~02")

df_JX2<-rbind(jx,jx1,jx2)
df_SJ2<-rbind(sj,sj1,sj2)
df_HX2<-rbind(hx,hx1,hx2)
df_WX2<-rbind(wx,wx1,wx2)
df_MD2<-rbind(md,md1,md2)
df_BL2<-rbind(bl,bl1,bl2)
df_XS2<-rbind(xs,xs1,xs2)
df_WC2<-rbind(wc,wc1,wc2)
df_ALL<-rbind(td,td1,td2)

### make data.Frames to be used to make the boxplots
df_new<-cbind(df_JX2,df_SJ2$`Total Electricity Usage`,df_HX2$`Total Electricity Usage`)
colnames(df_new)<-c("Time","JiXue","HuaXue")

df_new1<-cbind(df_BL2,df_MD2$`Total Electricity Usage`,df_JX2$`Total Electricity Usage`)
colnames(df_new1)<-c("Time","BoLi","MingDa","JiXue")

df_new2<-cbind(df_WX2,df_WX2$`Total Electricity Usage`,df_XS2$`Total Electricity Usage`)
colnames(df_new2)<-c("Time","WenXue","XinSheng")

df_new3<-cbind(df_JX2,df_HX2$`Total Electricity Usage`,df_BL2$`Total Electricity Usage`,df_MD2$`Total Electricity Usage`,df_WX2$`Total Electricity Usage`,df_XS2$`Total Electricity Usage`)
colnames(df_new3)<-c("Time","JiXue","HuaXue","BoLi","MingDa","WenXue","XinSheng")

library(reshape2)
[積學館與化學系館]
-開放時間：08:00-18:00；18:00後僅能刷卡進入館區 -電費由積學館支付；支付電費包含化學系館B棟等連結建物

Hide
ggplot(df_JX, aes(x = df_JX$Date,y = df_JX$`Total Electricity Usage`))+ 
  geom_boxplot()+
  xlab("Day of the Week") +
  ylab("Electricity Usage") +
  ggtitle("JiXue's Average Electricity Usage (day)")

ggplot(df_HX, aes(x = df_HX$Date,y = df_HX$`Total Electricity Usage`))+ 
  geom_boxplot()+
  xlab("Day of the Week") +
  ylab("Electricity Usage") +
  ggtitle("HuaXue's Average Electricity Usage (day)")

dfmelt<-melt(df_new, measure.vars = 2:3) 
dfmelt$bin <- factor(dfmelt$Time)
ggplot(dfmelt, aes(x=bin, y=value, fill=variable))+
  geom_boxplot()+
  facet_grid(.~bin, scales="free")+
  labs(x="Time of the Day",y="Electricity Usage")+
  theme(axis.text.x=element_blank())

#全校各時段用電量分布
Image5 <- readImage("~/dsR/data/ntu elec boxplot.png")
img5 <- display(Image5, method = "raster")

#化學館各時段用電量分布
Image6 <- readImage("~/dsR/data/huaxue elec boxplot.png")
img6 <- display(Image6, method = "raster")

#新生教學大樓各時段用電量分布
Image7 <- readImage("~/dsR/data/xinshen elec boxplot.png")
img7 <- display(Image7, method = "raster")
[訪談內容整理]

Q1. 為何02:00-08:00的用電量並沒有明顯少於08:00-18:00？

A1. 實驗數量遠多於教室，且實驗室內有許多必須24hr運作的儀器，例如：通風櫥、大型精密器械等

Q2. 校級單位是否有對積學館和化學館的用電做出評論？

A2. 至今沒有發生。積學館的用電量雖然較高，然而多半是為了維持儀器運作和實驗室的安全性，因此目前用電狀況皆屬於合理範圍。

[明達館與博理館]
-開放時間：07:00-18:20；18:20後僅能刷卡進入館區 -電費並非全數由明達和博理支付，因兩棟建築物皆包含產學合作樓層及校級單位保管組的辦公室

Hide
ggplot(df_BL, aes(x = df_BL$Date,y = df_BL$`Total Electricity Usage`))+ 
  geom_boxplot()+
  xlab("Day of the Week") +
  ylab("Electricity Usage") +
  ggtitle("Boli Electricity Usage (day)")

ggplot(df_MD, aes(x = df_MD$Date,y = df_MD$`Total Electricity Usage`))+ 
  geom_boxplot()+
  xlab("Day of the Week") +
  ylab("Electricity Usage") +
  ggtitle("Mingda's Average Electricity Usage (day)")

dfmelt1<-melt(df_new1, measure.vars = 2:3) 
dfmelt1$bin <- factor(dfmelt1$Time)
ggplot(dfmelt1, aes(x=bin, y=value, fill=variable))+
  geom_boxplot()+
  facet_grid(.~bin, scales="free")+
  labs(x="Time of the Day",y="Electricity Usage")+
  theme(axis.text.x=element_blank())

#產學合作樓層；例如：博理館7F
library(EBImage)
Image4 <- readImage("~/dsR/data/博理7F廣達辦公室.png")
img4 <- display(Image4, method = "raster")
[訪談內容整理]

Q1. 對於明達館和博理館而言，哪些儀器或設備佔了較多的用電量？

A1. 最耗電的設備依序為：空調設備/實驗室→電梯。

Q2. 有哪些設備是需要24小時維持運作的？最為耗電者為何？

A2. 夜間（禁止入館時段）仍需維持供電的設備，依耗電程度依序為(1)機房 (2)電梯 (3)LED逃生指示燈。

Q3. 明達館目前做到的節電措施有哪些？

A3.明達與博理為校內的節電措施相當有效率的建築物，例如：18:20後大部分的公共空間為感應式照明、較少使用的空間（如：女廁）皆為感應式照明、舊式燈管全數換完LED省電燈具等。

Q4. 校級單位是否有對明達館與博理館的用電做出評論？

A4. 目前沒有，因為電機系館在節能方面的動作一直是早於學校的政策。

[研究結論]
建議將電梯的停留樓層設定為僅停靠奇數樓層，以節省電梯運作所產生的用電量。
在上課時間以外，將大部分的公共空間或較少使用的空間改為感應式照明。
空調及照明系統改為自動偵測室內有人與否，而非手動關閉。
舊式燈管全數換為LED省電燈具（由T8燈管換為T5燈具）。
學生於課堂以外，使用教室須預先登記並刷學生證以啟動電器。
加強向館內人員宣導自付比率以及懲罰用電的觀念。
【學生宿舍類】
(怡文、宸馨)

※研究標的：學生宿舍男/女宿舍區

※研究目的：

由於宿舍區為學生日常生活接觸十分頻繁之處，因此我們想探討宿舍區用電情況，並探討改進之處。

※研究方法：

藉由R語言分析、一手資料訪問(輔導員與總務處)與二手資料(台灣大學電錶監視系統)蒐集，我們從全範圍宿舍區聚焦至女八、女九舍的探討。

Hide
library(ggplot2)
library(dplyr)

#總女生
Q<-read.csv("~/dsR/data/ALLGIRL.csv",header=T,encoding = "Big5")

ggplot(data=Q) +   
  geom_line(aes(x=Q$`年份`,  
                y=Q$`平均用電`,
                color=Q$`宿舍`) ) +
  labs(title="歷年用電變化圖",
       x="年份",
       y="平均用電") +theme_bw()
#總男生
R<-read.csv("~/dsR/data/ALLBOY.csv",header=T,encoding = "Big5")

ggplot(data=R) +   
  geom_line(aes(x=R$`年份`,  
                y=R$`平均用電`,
                color=R$`宿舍`) ) +
  labs(title="歷年用電變化圖",
       x="年份",
       y="平均用電") +theme_bw()
一、女生宿舍用電變化 與 男生宿舍用電變化

依據我們所得的資料，101年度的宿舍用電資料有很大程度的殘缺，因此101年度圖形呈現出異常的情況，我們決定忽略不計，而主要以98,99,100,102,103,104年度來我們研究主要參考資料。

在女宿部分，從圖中可以清楚看出女八女九於103年度之後呈現大幅上升的趨勢，從過去資料顯示雖然女八女九一直是人均用電量前兩名的宿舍，但103年後的大幅上升，仍然讓人產生好奇，對比其他宿舍五年來呈現緩步下降的趨勢，為什麼女八女九卻逆向而行？

而另外一方面，在男宿的部分扣除101年的資料不完全，都明顯呈現人均用電量下降的趨勢，而在不同宿舍之間也沒有非常大的平均差異。但值得一提的是，男四103年份加入水生館的合併資料導致當年人均用電飆升。這個特殊合併的現象，只出現在該年度，因此我們並不多加討論其原因。也因此本研究聚焦我們觀察之後的發現，決定針對女八女九的突出用電情形，開始分析的研究。

Hide
library(EBImage)
Image <- readImage("~/dsR/data/女八女九空調.png")
Image1 <- readImage("~/dsR/data/女二舍.png")
img <- display(Image,method = "raster")
img2 <- display(Image1, method = "raster")
二、女八女九用電變化圖與女八女九功因率圖

功率: 一有效功功率在電路中的特定時間作功的能力，為電壓和電流有效值的乘積。 若功率較高，所輸入電流也會減少，供電系統的效率能提昇。

我們選擇2017/1/4 下午進行功因率圖的分析，可以發現女八女九的用電功因率並不高，從20%至60%左右，與其他用電宿舍(此以女二舍為例)相比，用電變化功率可至50%至80%，因此女八女九舍可能因功因率不高，造成用電的效率不彰，耗用較多電力。

Hide
p<-read.csv("~/dsR/data/Longperiod.csv",header=T)
library(ggplot2)
library(dplyr)

ggplot(data=p) +   
  geom_line(aes(x=p$`年份`,  
                y=p$`用電`,
                color=p$`宿舍`) ) +
  labs(title="女八女九歷年用電變化圖",
       x="年份",
       y="用電") +theme_bw()
設定好女八女九為改善主題後，我們拉出此二宿舍單獨觀察，103年之前呈現持平的情況， 我們想去了解的是近兩年有什麼改變使得用量大增？我們決定找到女八女九的管理者進行訪問。

Hide
Image3 <- readImage("~/dsR/data/熱水器.png")
img3 <- display(Image3,method = "raster")
三、 訪談

【訪問對象】: 住宿組 陳心怡/營繕組 黃逸群

女八女九舍輔導員 敏慧

【訪談問題】:

1.想訪問看看是不是設備老舊？還是其他原因？

2.想知道冷氣或電燈的配置個數和品牌，我們主要針對使用者可控制的能源進行討論，希望能夠了解以上的資訊後，做出能夠改善的結論（如更換省電燈泡、冷氣等)

3.用電量最大的時候是否為學生在宿舍期間？

4.近幾年有注意到用電量高的問題，而進行改善嗎？困難點是什麼？

5.103年後有發生什麼特殊情況，導致平均用電量暴增？

[研究結論]
-電費由上述女八女九依照使用面積比例進行平均分攤 -女九特殊在於有些年分將女九餐廳合併計算

1.近年來學校也積極處理節能問題，學生宿舍全面改裝T5燈管、改善自然通風及自然採光環境以減少燈具使用，依季節氣溫變化調節熱水鍋爐溫度設定。

2.研一男女舍、男七舍及女二舍鍋爐系統由舊有瓦斯加熱系統改為熱泵系統已完工啟用，以減少能源耗損。

3.103年到104年，女八女九大幅上升的原因在於104年之前的電錶計算方式有誤，計算公式其實都偏低，104年之後才反映真實的女八九用電量，確實多年來平均用電樣都比女生其他宿舍高出不少。 解釋可能原因: 女八女九舍的房間內面積較大，學生人數和其他宿舍相比少，學生可利用的空間多，舍胞習慣擁有個人小電鍋、電磁爐等高電壓電子產品，在房內煮飯比例高，使用電量較大。此外女九舍因為衛浴空間為各房間獨立使用，採用電熱水器提供熱水，因此所需電量也會較高，亦可能是造成用電量明顯偏高。

4.建議電熱水器的更換，為了達到更有效率的用電，可從日常所需的熱水器下手，目前女九所使用的電熱水器為耗能最高的產品，本組建議在經費充足時可更換成效率更高的熱泵熱水器，女二舍即為成功案例，更換熱泵熱水器之後，目前人均用電量為女宿最低。

註: 女八女九舍冷器品牌為 TATUNG 大同

Hide
#104總男生總女生
d<-read.csv("~/dsR/data/104boygirl.csv",header=T,encoding = "Big5")

ggplot(d, aes(x = `宿舍`, y = `平均用電`, fill = `季節`)) + 
  geom_bar(stat = "identity", position = "dodge") + 
  labs(y = "人均用電") + labs(x = "宿舍")
圖中季節資訊: 冬季12-2月 春季3-5月 夏季6-8月 秋季9-11月

為了探索女生與男生在不同季節的用電習慣，我們以104年季節用電變化作分析，依據圖中資料，可以發現總女生人均用電在夏季時較高，總男生用電則在秋季較為突出，但以女生用電為較多。我們猜測可能與女生較為怕熱，夏季時較常使用冷氣，因此用電量較高，而冬季為次高原因在於熱水溫度會隨著氣溫調整。然而，此圖資料總女生中，包含用電極大的女八與女九極端值，因此無法全盤認定一般女生日常用電行為皆大於男生。

【總結】
這份報告的目的不僅是提出潛在的節能解決方案，更著重於討論用電狀況的經濟效率。然而，我們嚴重低估了這個問題的複雜性。雖然不完全是這樣的問題，但是台大的校級單位並非細部去管理問題，反而是減少每個部門的預算，讓各棟建築物的管理人員自行應變。

以明達館和文學院的對比為例，明達館擁有系友的捐贈、較多的預算，因此得以承擔節能技術的前期成本，所以他們一直積極地「走向綠色」。反觀文學院，由於預算顯著減少，管理人員就算想要更換較環保的電器設備，卻也礙於經費不足而只好持續使用老舊的耗電設備，甚至是不使用電子設備，最後只能等到設備壞損了才有更新技術的可能，然而這並不是長久之計。

即使我們找到一個可以節省資金且有效節能的解決方案，但仍然可能因為種種因素，如資金等，而不能保證這個解決方案的實施，因此有效節能的議題仍然是不易解的問題。
