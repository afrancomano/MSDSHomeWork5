---
title: "Doing Data Science - section 403 - Unit 5 - Live Assignment #5"
author: "AFrancomano"
date: "June 12, 2018"
output: 
  html_document:
    keep_md: true
---

## Question 1A + 1B + 1C + 1D:  R Code and output below

Data Munging (30 points): Utilize yob2016.txt for this question. This file is a series of popular children's names born in the year 2016 in the United States.  It consists of three columns with a first name, a gender, and the amount of children given that name.  However, the data is raw and will need cleaning to make it tidy and usable.

a.	First, import the .txt file into R so you can process it.  Keep in mind this is not a CSV file.  You might have to open the file to see what you're dealing with.  Assign the resulting data frame to an object, df, that consists of three columns with human-readable column names for each.

b.	Display the summary and structure of df

c.	Your client tells you that there is a problem with the raw file.  One name was entered twice and misspelled. The client cannot remember which name it is; there are thousands he saw! But he did mention he accidentally put three y's at the end of the name.  Write an R command to figure out which name it is and display it.

d.	Upon finding the misspelled name, please remove this particular observation, as the client says it's redundant.  Save the remaining dataset as an object: y2016 



```r
#a. Bring yob2016.txt into R; note the txt file contains semi-colon delimited data
df <- read.table("yob2016.txt", header = FALSE, sep = ";")    #read in the text file
dim(df)                                                       #confirm all data file lines were read in
```

```
## [1] 32869     3
```

```r
colnames(df) <- c("FirstName", "Gender", "NumberOfChildren2016")   #set the column names
#b.
summary(df)           #display the summary of df
```

```
##    FirstName     Gender    NumberOfChildren2016
##  Aalijah:    2   F:18758   Min.   :    5.0     
##  Aaliyan:    2   M:14111   1st Qu.:    7.0     
##  Aamari :    2             Median :   12.0     
##  Aarian :    2             Mean   :  110.7     
##  Aarin  :    2             3rd Qu.:   30.0     
##  Aaris  :    2             Max.   :19414.0     
##  (Other):32857
```

```r
str(df)               #display the structure of df
```

```
## 'data.frame':	32869 obs. of  3 variables:
##  $ FirstName           : Factor w/ 30295 levels "Aaban","Aabha",..: 9317 22546 3770 26409 12019 20596 6185 339 9298 11222 ...
##  $ Gender              : Factor w/ 2 levels "F","M": 1 1 1 1 1 1 1 1 1 1 ...
##  $ NumberOfChildren2016: int  19414 19246 16237 16070 14722 14366 13030 11699 10926 10733 ...
```

```r
#c.  Look for one bad name in the data
df[grep("yyy$", df$FirstName),"FirstName"]  #locate and display the firstname that ends in three y's
```

```
## [1] Fionayyy
## 30295 Levels: Aaban Aabha Aabid Aabir Aabriella Aadam Aadarsh ... Zyva
```

```r
#d.  Resolve issue with one name in the data that ends in three y's
badentry <- grep("yyy$", df$FirstName)  #assign row location of bad first name to a variable
y2016 <- df[(-1*badentry),]       # create new dataset that will not contain the bad firstname
grep("yyy$", y2016$FirstName)     # confirm that the new dataset does not contain a name with three y's at end
```

```
## integer(0)
```

```r
dim (df) [1] - dim (y2016)  [1]   # confirm that the new  dataset has one less entry than original dataset
```

```
## [1] 1
```

## Question 2A + 2B + 2C:  R Code and output below

Data Merging (30 points): Utilize yob2015.txt for this question.  This file is similar to yob2016, but contains names, gender, and total children given that name for the year 2015.

a.	Like 1a, please import the .txt file into R.  Look at the file before you do.  You might have to change some options to import it properly.  Again, please give the dataframe human-readable column names.  Assign the dataframe to y2015.  

b.	Display the last ten rows in the dataframe.  Describe something you find interesting about these 10 rows.

c.	Merge y2016 and y2015 by your Name column; assign it to final.  The client only cares about names that have data for both 2016 and 2015; there should be no NA values in either of your amount of children rows after merging

```r
#a. Bring yob2015.txt into R   
y2015 <- read.csv("yob2015.txt", header = FALSE)              #Read in the y2015.txt datafile
dim(y2015)                                                    #confirm all data lines were read in
```

```
## [1] 33063     3
```

```r
colnames(y2015) <- c("FirstName", "Gender", "NumberOfChildren2015")  #set the column names

#b. Retrieve the last ten rows of the y2015 dataframe
tail(y2015, 10)
```

```
##       FirstName Gender NumberOfChildren2015
## 33054      Ziyu      M                    5
## 33055      Zoel      M                    5
## 33056     Zohar      M                    5
## 33057    Zolton      M                    5
## 33058      Zyah      M                    5
## 33059    Zykell      M                    5
## 33060    Zyking      M                    5
## 33061     Zykir      M                    5
## 33062     Zyrus      M                    5
## 33063      Zyus      M                    5
```

```r
#OBSERVATIONS ABOUT THE TEN ROWS LISTED ABOVE: The 33,054th through 33,063rd entries are displayed.
#It looks like the datafile was already sorted alphabetically by Gender and then in descending 
#order by FirstName within Gender. The names in these entries seem to have unique spellings and are
#not common names. One in particular, Zoel, seems to be a variant of Joel.
#These ten first names were each given to five children in the Unites States in 2015.


#c. Merge

#Verify that there are no NAs in each dataset before merging
sum(is.na(y2015$FirstName))
```

```
## [1] 0
```

```r
sum(is.na(y2015$Gender))
```

```
## [1] 0
```

```r
sum(is.na(y2015$NumberOfChildren2015))
```

```
## [1] 0
```

```r
sum(is.na(y2016$FirstName))
```

```
## [1] 0
```

```r
sum(is.na(y2016$Gender))
```

```
## [1] 0
```

```r
sum(is.na(y2016$NumberOfChildren2016))
```

```
## [1] 0
```

```r
# We want to be sure to account for Gender as well as Firstname in order to preserve the fact 
# that some names will appear under both genders and will need to keep their distinct counts

final <- merge (y2015, y2016, by=c("FirstName","Gender"))
```


## Question 3A + 3B + 3C + 3D:  R Code and output below

Data Summary (30 points): Utilize your data frame object final for this part.

a.	Create a new column called "Total" in final that adds the amount of children in 2015 and 2016 together.  In those two years combined, how many people were given popular names?

b.	Sort the data by Total.  What are the top 10 most popular names?

c.	The client is expecting a girl!  Omit boys and give the top 10 most popular girl's names.

d.	Write these top 10 girl names and their Totals to a CSV file.  Leave out the other columns entirely.


```r
#a. Add a Total column that combines the 2015 and 2016 counts for each First name
final$Total <- final$NumberOfChildren2015 + final$NumberOfChildren2016
prettyNum(sum(final$Total), big.mark=",")    #number of people with the names listed in both the 2015 and 2016 popular names data files combined
```

```
## [1] "7,239,231"
```

```r
#b. Sort in descending order by Total column and then retrieve just first names in the top 10
finalsort <- final[order(-final$Total),]   
finalsort[1:10, "FirstName",]            
```

```
##  [1] Emma     Olivia   Noah     Liam     Sophia   Ava      Mason   
##  [8] William  Jacob    Isabella
## 30553 Levels: Aaban Aabha Aabriella Aada Aadam Aadan Aadarsh Aaden ... Zyvon
```

```r
#c. Filter out only the female names and then retrieve just the first names in the top 10
Girls <- subset(finalsort, Gender =="F")
Girls[1:10, "FirstName",]
```

```
##  [1] Emma      Olivia    Sophia    Ava       Isabella  Mia       Charlotte
##  [8] Abigail   Emily     Harper   
## 30553 Levels: Aaban Aabha Aabriella Aada Aadam Aadan Aadarsh Aaden ... Zyvon
```

```r
#d. Write the top 10 Girls names with their Totals to a .csv file
write.csv(Girls[1:10, c(1, 5)], file="Top10GirlsNames.csv", row.names=FALSE)
```

## Question 4:  Github link below

Upload to GitHub (10 points): Push at minimum your RMarkdown for this homework assignment and a Codebook to one of your GitHub repositories (you might place this in a Homework repo like last week).  The Codebook should contain a short definition of each object you create, and if creating multiple files, which file it is contained in.  You are welcome and encouraged to add other files-just make sure you have a description and directions that are helpful for the grader.

***THE GITHUB LINK IS  https://github.com/afrancomano/MSDSHomeWork5

**For Reference, if needed, the Git bash command window interaction is as follows:
anne@lenovo-PC MINGW64 ~/gitlocalhomework5
$ ls
AFRANCOMANO_MSDS_6306_403_Unit5_HW5.html  README.md            yob2016.txt
AFRANCOMANO_MSDS_6306_403_Unit5_HW5.Rmd   Top10GirlsNames.csv
CodebookHW5.txt                           yob2015.txt

anne@lenovo-PC MINGW64 ~/gitlocalhomework5
$ git init
Initialized empty Git repository in C:/Users/anne/gitlocalhomework5/.git/

anne@lenovo-PC MINGW64 ~/gitlocalhomework5 (master)
$ git add .
warning: LF will be replaced by CRLF in CodebookHW5.txt.
The file will have its original line endings in your working directory.

anne@lenovo-PC MINGW64 ~/gitlocalhomework5 (master)
$ git commit -m "the first commit from localhomework 5"
[master (root-commit) 7b783cd] the first commit from localhomework 5
 7 files changed, 66483 insertions(+)
 create mode 100644 AFRANCOMANO_MSDS_6306_403_Unit5_HW5.Rmd
 create mode 100644 AFRANCOMANO_MSDS_6306_403_Unit5_HW5.html
 create mode 100644 CodebookHW5.txt
 create mode 100644 README.md
 create mode 100644 Top10GirlsNames.csv
 create mode 100644 yob2015.txt
 create mode 100644 yob2016.txt

anne@lenovo-PC MINGW64 ~/gitlocalhomework5 (master)
$ git remote add origin https://github.com/afrancomano/MSDSHomeWork5

anne@lenovo-PC MINGW64 ~/gitlocalhomework5 (master)
$ git remote -v
origin  https://github.com/afrancomano/MSDSHomeWork5 (fetch)
origin  https://github.com/afrancomano/MSDSHomeWork5 (push)

anne@lenovo-PC MINGW64 ~/gitlocalhomework5 (master)
$ git push origin master
Username for 'https://github.com': amf42002@yahoo.com
Counting objects: 9, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (9/9), done.
Writing objects: 100% (9/9), 504.70 KiB | 3.12 MiB/s, done.
Total 9 (delta 0), reused 0 (delta 0)
To https://github.com/afrancomano/MSDSHomeWork5
 * [new branch]      master -> master

anne@lenovo-PC MINGW64 ~/gitlocalhomework5 (master)
$ git add .
warning: LF will be replaced by CRLF in CodebookHW5.txt.
The file will have its original line endings in your working directory.

anne@lenovo-PC MINGW64 ~/gitlocalhomework5 (master)
$ git commit -m "update the codebook for ocalhomework 5"
[master 2b832f4] update the codebook for ocalhomework 5
 1 file changed, 2 insertions(+)

anne@lenovo-PC MINGW64 ~/gitlocalhomework5 (master)
$ git push origin master
Username for 'https://github.com': amf42002@yahoo.com
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 410 bytes | 410.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/afrancomano/MSDSHomeWork5
   7b783cd..2b832f4  master -> master

anne@lenovo-PC MINGW64 ~/gitlocalhomework5 (master)
$
