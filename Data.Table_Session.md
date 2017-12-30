Data.Table  R Session
========================================================
author: Ramana Reddy  
date: 4 December 2015
width: 1700
height: 1500

data import of csv files with fread from data.table
========================================================

```r
library(data.table)
setwd("E:\\RTraining")
ptm<-NULL
ptm <- proc.time()
DT<-fread("trainData.csv",sep=",",header=TRUE,verbose = FALSE)
proc.time() - ptm
```

```
   user  system elapsed 
   0.15    0.00    0.15 
```

Since you are dealing with a lot of data, it is recommended that you do a lot of aggregations and stuff using data.table package!!

They say it can deal from 1GB to 100GB of data.


Check data overiew 
========================================================


```r
# get all the column names 
names(DT)
```

```
 [1] "Id"     "V1"     "V2"     "V3"     "V4"     "V5"     "V6"    
 [8] "V7"     "V8"     "V9"     "V10"    "V11"    "V12"    "V13"   
[15] "V14"    "V15"    "V16"    "V17"    "V18"    "V19"    "V20"   
[22] "V21"    "V22"    "V23"    "V24"    "V25"    "V26"    "V27"   
[29] "V28"    "V29"    "V30"    "V31"    "V32"    "Hazard"
```

Check data overiew contd..
========================================================


```r
# get dimensions of the data
dim(DT)
```

```
[1] 50999    34
```

```r
# get summary of the data
summary(DT)
```

```
       Id               V1               V2              V3       
 Min.   :     1   Min.   : 1.000   Min.   : 1.00   Min.   :1.000  
 1st Qu.: 25661   1st Qu.: 6.000   1st Qu.: 7.00   1st Qu.:2.000  
 Median : 50977   Median : 9.000   Median :14.00   Median :3.000  
 Mean   : 50930   Mean   : 9.722   Mean   :12.85   Mean   :3.186  
 3rd Qu.: 76269   3rd Qu.:14.000   3rd Qu.:18.00   3rd Qu.:4.000  
 Max.   :101999   Max.   :19.000   Max.   :24.00   Max.   :9.000  
      V4                 V5                 V6           
 Length:50999       Length:50999       Length:50999      
 Class :character   Class :character   Class :character  
 Mode  :character   Mode  :character   Mode  :character  
                                                         
                                                         
                                                         
      V7                 V8                 V9                 V10       
 Length:50999       Length:50999       Length:50999       Min.   : 2.00  
 Class :character   Class :character   Class :character   1st Qu.: 3.00  
 Mode  :character   Mode  :character   Mode  :character   Median : 8.00  
                                                          Mean   : 7.02  
                                                          3rd Qu.: 8.00  
                                                          Max.   :12.00  
     V11                V12                 V13          V14       
 Length:50999       Length:50999       Min.   : 5   Min.   :0.000  
 Class :character   Class :character   1st Qu.:10   1st Qu.:1.000  
 Mode  :character   Mode  :character   Median :15   Median :1.000  
                                       Mean   :14   Mean   :1.579  
                                       3rd Qu.:20   3rd Qu.:2.000  
                                       Max.   :20   Max.   :4.000  
     V15                V16                V17                 V18        
 Length:50999       Length:50999       Length:50999       Min.   :  1.00  
 Class :character   Class :character   Class :character   1st Qu.: 40.00  
 Mode  :character   Mode  :character   Mode  :character   Median : 56.00  
                                                          Mean   : 57.58  
                                                          3rd Qu.: 77.00  
                                                          Max.   :100.00  
      V19            V20                 V21            V22           
 Min.   : 1.00   Length:50999       Min.   : 1.00   Length:50999      
 1st Qu.: 9.00   Class :character   1st Qu.: 6.00   Class :character  
 Median :11.00   Mode  :character   Median :10.00   Mode  :character  
 Mean   :12.42                      Mean   :10.26                     
 3rd Qu.:15.00                      3rd Qu.:14.00                     
 Max.   :39.00                      Max.   :22.00                     
      V23             V24             V25             V26       
 Min.   :1.000   Min.   :22.00   Min.   :1.000   Min.   : 1.00  
 1st Qu.:2.000   1st Qu.:31.00   1st Qu.:1.000   1st Qu.: 6.00  
 Median :2.000   Median :34.00   Median :1.000   Median :14.00  
 Mean   :1.948   Mean   :33.49   Mean   :1.032   Mean   :12.49  
 3rd Qu.:2.000   3rd Qu.:40.00   3rd Qu.:1.000   3rd Qu.:18.00  
 Max.   :7.000   Max.   :40.00   Max.   :3.000   Max.   :25.00  
      V27            V28                V29                V30           
 Min.   :1.000   Length:50999       Length:50999       Length:50999      
 1st Qu.:3.000   Class :character   Class :character   Class :character  
 Median :4.000   Mode  :character   Mode  :character   Mode  :character  
 Mean   :4.497                                                           
 3rd Qu.:6.000                                                           
 Max.   :7.000                                                           
      V31             V32             Hazard      
 Min.   :1.000   Min.   : 1.000   Min.   : 1.000  
 1st Qu.:2.000   1st Qu.: 1.000   1st Qu.: 1.000  
 Median :2.000   Median : 2.000   Median : 3.000  
 Mean   :2.451   Mean   : 3.484   Mean   : 4.023  
 3rd Qu.:3.000   3rd Qu.: 5.000   3rd Qu.: 5.000  
 Max.   :7.000   Max.   :12.000   Max.   :69.000  
```


slicing the data
========================================================

```r
# Select only V1, V2
subsetdata<-DT[,.(V1,V2)]
dim(subsetdata)
```

```
[1] 50999     2
```

```r
head(subsetdata)
```

```
   V1 V2
1: 15  3
2: 16 14
3: 10 10
4: 18 18
5: 13 19
6: 14 12
```

```r
# select data where V1>10
subsetdata<-subsetdata[V1>10]
dim(subsetdata)
```

```
[1] 21939     2
```

```r
head(subsetdata)
```

```
   V1 V2
1: 15  3
2: 16 14
3: 18 18
4: 13 19
5: 14 12
6: 14 20
```

```r
# use head and tail funtions to view the first and last few rows of the data like in unix/linux
# tip DT[ Vx %in% c("A","C")] Select all rows that have the value A or C in column Vx
```

Aggregating of columns sums/means/stand dev. etc.,
========================================================


```r
# get sum of V1 and stdev of V2 by each value of V7

DT[,.(sum_of_v1_by_v7=sum(V1),std_dev_of_V2_by_v7=sd(V2)), by=V7]
```

```
   V7 sum_of_v1_by_v7 std_dev_of_V2_by_v7
1:  B          466111            6.259855
2:  D           20986            6.205740
3:  A            5019            6.105265
4:  C            3701            5.671599
```

```r
# get sum of V1 and stdev of V2 by each value of V7 and v8

head(DT[,.(sum_of_v1_by_v7=sum(V1),std_dev_of_V2_by_v7=sd(V2)), by=.(V7,V8)])
```

```
   V7 V8 sum_of_v1_by_v7 std_dev_of_V2_by_v7
1:  B  B          423194            6.281217
2:  D  B           18807            6.231568
3:  B  D           18184            6.247884
4:  B  A            9226            6.253390
5:  A  A              85            6.429101
6:  B  C           15507            5.589871
```

```r
# ofcourse you can save them to a new data.table by just assigning the above to a data.table

sum_std_of_V1_v2_by_v7_v8<- DT[,.(sum_of_v1_by_v7=sum(V1),std_dev_of_V2_by_v7=sd(V2)), by=.(V7,V8)]
head(sum_std_of_V1_v2_by_v7_v8)
```

```
   V7 V8 sum_of_v1_by_v7 std_dev_of_V2_by_v7
1:  B  B          423194            6.281217
2:  D  B           18807            6.231568
3:  B  D           18184            6.247884
4:  B  A            9226            6.253390
5:  A  A              85            6.429101
6:  B  C           15507            5.589871
```

Some cool stuff - pretty useful at times
========================================================


```r
# adding the maxium of sum(v1) to the above dataset sum_std_of_V1_v2_by_v7_v8

sum_std_of_V1_v2_by_v7_v8<-sum_std_of_V1_v2_by_v7_v8[,max_sum_v1:= max(sum_of_v1_by_v7)]
head(sum_std_of_V1_v2_by_v7_v8)
```

```
   V7 V8 sum_of_v1_by_v7 std_dev_of_V2_by_v7 max_sum_v1
1:  B  B          423194            6.281217     423194
2:  D  B           18807            6.231568     423194
3:  B  D           18184            6.247884     423194
4:  B  A            9226            6.253390     423194
5:  A  A              85            6.429101     423194
6:  B  C           15507            5.589871     423194
```

```r
# the above is very useful if you have derive %of total or difference from mean during mdoelling

# Creating two columns now instaed of one. You can create new columns based on existing columns too.
sum_std_of_V1_v2_by_v7_v8<-sum_std_of_V1_v2_by_v7_v8[,c("max_sum_v1","mean_sum_v1") := list(max(sum_of_v1_by_v7),mean(sum_of_v1_by_v7))]
head(sum_std_of_V1_v2_by_v7_v8)
```

```
   V7 V8 sum_of_v1_by_v7 std_dev_of_V2_by_v7 max_sum_v1 mean_sum_v1
1:  B  B          423194            6.281217     423194    30988.56
2:  D  B           18807            6.231568     423194    30988.56
3:  B  D           18184            6.247884     423194    30988.56
4:  B  A            9226            6.253390     423194    30988.56
5:  A  A              85            6.429101     423194    30988.56
6:  B  C           15507            5.589871     423194    30988.56
```

```r
# TIP: DT[,.N,by=V1] gives Count the number of rows for every group in V1.
# TIP DT[, c("V1","V2") := NULL] Removes columns V1 and V2. 
# you can also delete the columns creating a parameter variable

columns_to_be_deleted = c("V1","V2")
DT[,(columns_to_be_deleted) :=NULL ]
names(DT)
```

```
 [1] "Id"     "V3"     "V4"     "V5"     "V6"     "V7"     "V8"    
 [8] "V9"     "V10"    "V11"    "V12"    "V13"    "V14"    "V15"   
[15] "V16"    "V17"    "V18"    "V19"    "V20"    "V21"    "V22"   
[22] "V23"    "V24"    "V25"    "V26"    "V27"    "V28"    "V29"   
[29] "V30"    "V31"    "V32"    "Hazard"
```


Keys and Indexes
========================================================


```r
# Use setkey() to set a key on a DT. The data is sorted on the column we specified by reference

setkey(DT,V4) 
# now you can use the above key to kind of filter and slice the database real fast
# lets create a datatable where V4=="N". All you need is just one line. Imagine filtering out a particular account given a set of data whose name is N
DT_N<-DT["N"]
dim(DT_N)
```

```
[1] 25112    32
```

```r
# we can always multiple filters like V4==N OR V4== H
DT_N<-DT[c("N","H")]
dim(DT_N)
```

```
[1] 25726    32
```



changing column names and reordering columns
========================================================


```r
DT<-fread("trainData.csv",sep=",",header=TRUE,verbose = FALSE)

# Sets the name of column V2 to Rating
setnames(DT,"V2","Rating") 
names(DT)
```

```
 [1] "Id"     "V1"     "Rating" "V3"     "V4"     "V5"     "V6"    
 [8] "V7"     "V8"     "V9"     "V10"    "V11"    "V12"    "V13"   
[15] "V14"    "V15"    "V16"    "V17"    "V18"    "V19"    "V20"   
[22] "V21"    "V22"    "V23"    "V24"    "V25"    "V26"    "V27"   
[29] "V28"    "V29"    "V30"    "V31"    "V32"    "Hazard"
```

```r
# Changes two column names. 
setnames(DT,c("Rating","V3"),c("V2.rating","V3.DataCamp"))
names(DT)
```

```
 [1] "Id"          "V1"          "V2.rating"   "V3.DataCamp" "V4"         
 [6] "V5"          "V6"          "V7"          "V8"          "V9"         
[11] "V10"         "V11"         "V12"         "V13"         "V14"        
[16] "V15"         "V16"         "V17"         "V18"         "V19"        
[21] "V20"         "V21"         "V22"         "V23"         "V24"        
[26] "V25"         "V26"         "V27"         "V28"         "V29"        
[31] "V30"         "V31"         "V32"         "Hazard"     
```

```r
# head(DT)
# select only four columns
DT<-DT[,.(V2.rating,V1,V4,V3.DataCamp)]
# Changes the column ordering to the contents of the vector.
setcolorder(DT,c("V2.rating","V1","V4","V3.DataCamp"))
head(DT)
```

```
   V2.rating V1 V4 V3.DataCamp
1:         3 15  N           2
2:        14 16  H           5
3:        10 10  N           5
4:        18 18  N           5
5:        19 13  N           5
6:        12 14  N           2
```


merging tables
========================================================

```r
# lets merge DT with sum_std_of_V1_v2_by_v7_v8
# notice in head(), i can give the number of rows i want see,only 2 here
head(sum_std_of_V1_v2_by_v7_v8,2)
```

```
   V7 V8 sum_of_v1_by_v7 std_dev_of_V2_by_v7 max_sum_v1 mean_sum_v1
1:  B  B          423194            6.281217     423194    30988.56
2:  D  B           18807            6.231568     423194    30988.56
```

```r
# lets read DT again  as I changed it before
DT<-fread("trainData.csv",sep=",",header=TRUE,verbose = FALSE)

# so we need all the columns and rowsin DT but merged with sum_std_of_V1_v2_by_v7_v8
dim(DT)
```

```
[1] 50999    34
```

```r
DT<-merge(DT,sum_std_of_V1_v2_by_v7_v8,all.x=TRUE,by = c("V7","V8"))
# after merge we are adding 4 columns.
dim(DT)
```

```
[1] 50999    38
```

```r
head(DT,2)
```

```
   V7 V8   Id V1 V2 V3 V4 V5 V6 V9 V10 V11 V12 V13 V14 V15 V16 V17 V18 V19
1:  A  A  139  8 10  3  N  K  N  F  12   H   B  15   1   A   N   N  55  11
2:  A  A 3505  6 18  2  B  A  Y  D   8   B   B  20   1   A   B   N  47  11
   V20 V21 V22 V23 V24 V25 V26 V27 V28 V29 V30 V31 V32 Hazard
1:   Y   3   C   1  25   1  23   5   Y   N   E   1   1     13
2:   Y   2   B   2  37   1   7   3   Y   N   E   2   1      1
   sum_of_v1_by_v7 std_dev_of_V2_by_v7 max_sum_v1 mean_sum_v1
1:              85            6.429101     423194    30988.56
2:              85            6.429101     423194    30988.56
```



Final things before we wrap up.
========================================================
find more stuff here 

https://s3.amazonaws.com/assets.datacamp.com/img/blog/data+table+cheat+sheet.pdf

also try and finsih this course

http://blog.datacamp.com/data-analysis-the-data-table-way-introducing-datacamps-newest-course/

Next session

RMysQL

sapply / lapply 



