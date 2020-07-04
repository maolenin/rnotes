# sub setting #
x <- data.frame("var1" = sample(1:5), "var2" = sample(6:10), "var3" = sample(11:15))\n
x <- x[sample(1:5),]; x$var2[c(1,3)] = NA
x[,1]
x[, "var1"]
x[1:2, "var2"]
x[1:2, ]
x[(x$var1 <= 3 & x$var3 > 11),]
x[(x$var1 <= 3 | x$var3 > 15),]
x[which(x$var2 > 8),] # eliminating NAs

# Sorting #
sort(x$var1)
sort(x$var1, decreasing = TRUE)
sort(x$var2, na.last = TRUE) # NA at the end
x[order(x$var1),] #order data frame
x[order(x$var1, x$var3),] # first order var1, afeter var3

# Sorting with plyr
library(plyr)
arrange(x, var1)
arrange(x, desc(var1))

# Adding Rows and columns
x$var4 <- rnorm(5)
y <- cbind(x, rnorm(5))

# Sum #
sum(doc[, "V4"])

mydf <- read.csv(path2csv,stringsAsFactors = FALSE)
cran <- tbl_df(mydf)
packageVersion("dplyr")
select(cran, ip_id, package, country)
select(cran, r_arch:country)
select(cran, r_arch:country)
select(cran, country:r_arch)
select(cran, -time)
select(cran, -X:-size)
select(cran, -(X:size))
cran2 <- select(cran, size:ip_id)

#Filter
filter(cran, package == "swirl")
filter(cran, r_version == "3.1.1", country == "US")
filter(cran, r_version <= "3.1.1", country == "IN")
filter(cran, r_version <= "3.0.2", country == "IN")
filter(cran, country == "IN" | country == "US")
filter(cran, size > 100500, r_os == "linux-gnu")
filter(cran, !is.na(r_version))

#Arrange
arrange(cran2, ip_id)
arrange(cran2, desc(ip_id))
arrange(cran2, package, ip_id)
arrange(cran2, country, desc(r_version), ip_id)
cran3 <- select(cran, ip_id, package, size)

#Mutate
mutate(cran3, size_mb = size / 2^20)
mutate(cran3, size_mb = size / 2^20, size_gb = size_mb / 2^10)
mutate(cran3, correct_size = size + 1000 )
summarize(cran, avg_bytes = mean(size))

# Read XML With HTTPS ##
library(XML)
library(RCurl)
fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml"
xData <- getURL(fileUrl)
doc <- xmlParse(xData)
rootNode <- xmlRoot(doc)
xmlName(rootNode)
Access like a List
rootNode[[1]]
rootNode[[1]][[1]]
xmlSApply(rootNode, xmlValue) #join all values
xpathSApply(rootNode, "//name", xmlValue)

## Getting data from the web
if(!file.exists("./data")){dir.create("./data")}
download.file(fileUrl, destfile = "./data/restaurants.csv", method = "curl")
restData <- read.csv("./data/restaurants.csv")
head(restData, n = 3) # look at the bot of the data 
tail(restData, n = 3) # look at the bot of the data
summary(restData) #summary 
str(restData) #summary
quantile(restData$councilDistrict, na.rm = TRUE) # Quantile of quiantitative variables
quantile(restData$councilDistrict, probs = c(0.5, 0.75, 0.9)) # 50% , 75% and 90%
table(restData$zipCode, useNA = "ifany") # Make table
table(restData$councilDistrict, restData$zipCode) # cross table
sum(is.na(restData$councilDistrict)) # there isn't Na values
any(is.na(restData$councilDistrict)) # there isn't Na values
all(restData$zipCode > 0) 
colSums(is.na(restData)) #check how many missing data have
all(colSums(is.na(restData)) == 0)
table(restData$zipCode %in% c("21212")) # value of 21212
table(restData$zipCode %in% c("21212", "21213")) # value of 21212 or 21213
restData[restData$zipCode %in% c("21212", "21213"), ] # Sub setting with the condition
restData$nearMe = restData$neighborhood %in% c("Roland Park", "Homeland") 
table(restData$nearMe) # create a table with restaurants near me

DF <- as.data.frame(UCBAdmissions)
xt <- xtabs(Freq ~ Gender + Admit, data = DF) # cross table
warpbreaks$replicate <- rep(1:9, len = 54)xt
ftable(xt)

fakeData = rnorm(1e5)
object.size(fakeData)
print(object.size(fakeData), units = "Mb")
xt = xtabs(breaks ~ ., data = warpbreaks)

## Ccreating new variables ##
s1 s1 <- seq(1, 10, by=2) # Create variables from 1 to 10 2 by 2
s1 <- seq(1, 10, length=3) # same 1 but three elements
x <- c(1, 3, 8, 25, 100); seq(along x) # variables in that order


## READ HPPTS ##
library(xml2)
suppressWarnings(dx<-read_xml("https://www.espn.com/nfl/team/_/name/bal/baltimore-ravens", as_html=TRUE))
teams<-as.character(xml_contents(xml_find_all(dx,"//div[@class='game-info']")))
scores<-as.character(xml_contents(xml_find_all(dx,"//div[@class='score']")))

## READ JSON ##
library(jsonlite)
jsonData <- fromJSON("https://api.github.com/users/jtleek/repos")
cat(myjson)
jsonData$id
myjson <- toJSON(iris, pretty = TRUE) #Write
cat(myjson)

## Read SQL ##
acs <- read.csv("https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06pid.csv")
sqldf works if you detach RMySQL
detach("package:RMySQL", unload=TRUE)
sqldf("select pwgtp1 from acs where AGEP < 50")

## Read Lines by Lines in HTML ##
html <- readLines(url)
nchar(html[10]) #Get the number of charcaters in the line 10

## Read Fixed Files ##
the lenghts in width must be included the blank spaces
doc <- read.fwf(fileUrl, widths = c(10, 9, 4, 9, 4, 9, 4, 9, 4), skip=4)

#Group
by_package <- group_by(cran, package)
summarize(by_package, mean(size))

