##### Sub setting
    x <- data.frame("var1" = sample(1:5), "var2" = sample(6:10), "var3" = sample(11:15))
    x <- x[sample(1:5),]; x$var2[c(1,3)] = NA
    x[,1]
    x[, "var1"]
    x[1:2, "var2"]
    x[1:2, ]
    x[(x$var1 <= 3 & x$var3 > 11),]
    x[(x$var1 <= 3 | x$var3 > 15),]
    x[which(x$var2 > 8),] # eliminating NAs 
    
##### Sorting
    sort(x$var1)
    sort(x$var1, decreasing = TRUE)
    sort(x$var2, na.last = TRUE) # NA at the end
    x[order(x$var1),] #order data frame
    x[order(x$var1, x$var3),] # first order var1, afeter var3
##### Tidying Data with tidyr
    library(tidyr)
    gather(students, sex, count, -grade)
    res <- gather(students2, sex_class, count, -grade)
    separate(data = res, col = sex_class, into = c("sex", "class"))
    

##### Sorting with plyr
    library(plyr)
    arrange(x, var1)
    arrange(x, desc(var1))
##### Managing DF with dplyr
    library("dplyr")
    packageVersion("dplyr")
    head(select(chicago, city:dptp)) 
    head(select(chicago, -(city:dptp)))
    **replace from ..**
    i <- match("city", names(chicago))
    j <- match("dptp", names(chicago))
    head(chicago[, -(i:j)])
    .. replace to
    chic.f <- filter(chicago, pm25tmean2 > 30)
    chic.f <- filter(chicago, pm25tmean2 > 30 & tmpd > 80)
    chicago <- arrange(chicago, date) # order
    chicago <- arrange(chicago, desc(date)) # descending order
    chicago <- rename(chicago, pm25 = pm25tmean2, dewpoint = dptp) #rename rows
    head(select(chicago, pm25, pm25detrend))
    chicago <- mutate(chicago, tempcat = factor(1 * (tmpd > 80), labels = c("cold", "hot")))
    hotcold <- group_by(chicago, tempcat)
    summarize(hotcold, pm25 = mean(pm25, na.rm = TRUE), o3 = max(o3tmean2), no2 = median(no2tmean2))
    chicago <- mutate(chicago, year = as.POSIXlt(date)$year + 1900)
    years <- group_by(chicago, year)
    chicago %>% mutate(month = as.POSIXlt(date)$mon + 1) %>% group_by(month) %>% summarize(pm25 = mean(pm25, na.rm = TRUE), o3 = max(o3tmean2), no2 = median(no2tmean2)) 
   ##### Merging Data
    fileUrl1 = "https://raw.githubusercontent.com/DataScienceSpecialization/courses/master/03_GettingData/04_01_editingTextVariables/data/reviews.csv"
    fileUrl2 = "https://raw.githubusercontent.com/DataScienceSpecialization/courses/master/03_GettingData/04_01_editingTextVariables/data/solutions.csv"
    download.file(fileUrl1,destfile="./data/reviews.csv",method="curl")
    download.file(fileUrl2,destfile="./data/solutions.csv",method="curl")
    reviews = read.csv("./data/reviews.csv"); solutions <- read.csv("./data/solutions.csv")
    mergedData = merge(reviews, solutions, by.x = "solution_id", by.y = "id", all = TRUE)
    intersect(names(solutions), names(reviews))
    mergedData2 = merge(reviews, solutions, all = TRUE)
    
   ###### Another way by dplyr
    df1 = data.frame(id = sample(1:10), x = rnorm(10))
    df2 = data.frame(id = sample(1:10), y = rnorm(10))
    arrange(join(df1, df2), id)
    df3 = data.frame(id = sample(1:10), z = rnorm(10))
    dfList = list(df1, df2, df3)
    join_all(dfList)
    
##### Adding Rows and columns
    x$var4 <- rnorm(5)
    y <- cbind(x, rnorm(5))

##### Sum 
    sum(doc[, "V4"])
    mydf <- read.csv(path2csv,stringsAsFactors = FALSE)
    cran <- tbl_df(mydf)
    
    select(cran, ip_id, package, country)
    select(cran, r_arch:country)
    select(cran, r_arch:country)
    select(cran, country:r_arch)
    select(cran, -time)
    select(cran, -X:-size)
    select(cran, -(X:size))
    cran2 <- select(cran, size:ip_id)

##### Filter
    filter(cran, package == "swirl")
    filter(cran, r_version == "3.1.1", country == "US")
    filter(cran, r_version <= "3.1.1", country == "IN")
    filter(cran, r_version <= "3.0.2", country == "IN")
    filter(cran, country == "IN" | country == "US")
    filter(cran, size > 100500, r_os == "linux-gnu")
    filter(cran, !is.na(r_version))

##### Arrange
    arrange(cran2, ip_id)
    arrange(cran2, desc(ip_id))
    arrange(cran2, package, ip_id)
    arrange(cran2, country, desc(r_version), ip_id)
    cran3 <- select(cran, ip_id, package, size)

##### Mutate
    mutate(cran3, size_mb = size / 2^20)
    mutate(cran3, size_mb = size / 2^20, size_gb = size_mb / 2^10)
    mutate(cran3, correct_size = size + 1000 )
    summarize(cran, avg_bytes = mean(size))

##### Read XML With HTTPS
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

##### Getting data from the web and subsetting
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
    restData$zipWrong = ifelse(restData$zipCode < 0, TRUE, FALSE)
    table(restData$zipWrong, restData$zipCode < 0)
   ###### Cutting
    restData$zipGroups = cut(restData$zipCode, breaks = quantile(restData$zipCode))
    table(restData$zipGroups)
    table(restData$zipGroups, restData$zipCode)
   ###### Easier Cutting
    library(Hmisc)
    restData$zipGroups = cut2(restData$zipCode, g = 4)
    table(restData$zipGroups)
   ###### Creating factor variables    
    restData$zcf <- factor(restData$zipCode)
    restData$zcf[1:10]
   ###### Levels of factor variables
    yesno <- sample(c("yes", "no"), size = 10, replace = TRUE)
    yesnofac = factor(yesno, levels = c("yes", "no"))
    relevel(yesnofac, ref="yes")
   ###### using the mutate function
    library(plyr)
    library(Hmisc)
    restData2 = mutate(restData, zipGroups = cut2(zipCode, g = 4))
    table(restData2$zipGroups)
   ###### Common transforms
    abs(x)
    sqrt(x)
    ceiling(x) ceiling(3.475) is 4
    floor(x) floor(3.475) is 3
    round(x, digits = n) round(3.475, digits = 2) is 3.48
    signif(x, digits = n) signif(3.475, digits = 2) is 3.5
    log(x) natural logarimt
    log2(x), log10(x) others

    DF <- as.data.frame(UCBAdmissions)
    xt <- xtabs(Freq ~ Gender + Admit, data = DF) # cross table
    warpbreaks$replicate <- rep(1:9, len = 54)xt
    ftable(xt)

    fakeData = rnorm(1e5)
    object.size(fakeData)
    print(object.size(fakeData), units = "Mb")
    xt = xtabs(breaks ~ ., data = warpbreaks)

##### Creating new variables
    s1 <- seq(1, 10, by=2) # Create variables from 1 to 10 2 by 2
    s2 <- seq(1, 10, length=3) # same 1 but three elements
    x <- c(1, 3, 8, 25, 100); seq(along x) # variables in that order

##### READ HPPTS
    library(xml2)
    suppressWarnings(dx<-read_xml("https://www.espn.com/nfl/team/_/name/bal/baltimore-ravens", as_html=TRUE))
    teams<-as.character(xml_contents(xml_find_all(dx,"//div[@class='game-info']")))
    scores<-as.character(xml_contents(xml_find_all(dx,"//div[@class='score']")))

##### READ JSON
    library(jsonlite)
    jsonData <- fromJSON("https://api.github.com/users/jtleek/repos")
    cat(myjson)
    jsonData$id
    myjson <- toJSON(iris, pretty = TRUE) #Write
    cat(myjson)

##### Read SQL 
    acs <- read.csv("https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06pid.csv")
    sqldf works if you detach RMySQL
    detach("package:RMySQL", unload=TRUE)
    sqldf("select pwgtp1 from acs where AGEP < 50")

##### Read Lines by Lines in HTML 
    html <- readLines(url)
    nchar(html[10]) #Get the number of charcaters in the line 10

##### Read Fixed Files 
    the lenghts in width must be included the blank spaces
    doc <- read.fwf(fileUrl, widths = c(10, 9, 4, 9, 4, 9, 4, 9, 4), skip=4)

##### Reshaping Data
    library(reshape2)
    mtcars$carname <- rownames(mtcars)
    carMelt <- melt(mtcars, id = c("carname", "gear", "cyl"), measure.vars = c("mpg", "hp"))
    head(carMelt, n = 3)
    cylData <- dcast(carMelt, cyl ~ variable)

##### Averaging values
    tapply(InsectSprays$count, InsectSprays$spray, sum)
    spIns = split(InsectSprays$count, InsectSprays$spray) another way
    sprCount = lapply(spIns, sum)
    unlist(sprCount)
    sapply(spIns, sum)
    ddply(InsectSprays, .(spray), summarise, sum = sum(count))
    spraySums <- ddply(InsectSprays, .(spray), summarise, sum = ave(count, FUN = sum))

##### Group
    by_package <- group_by(cran, package)
    summarize(by_package, mean(size))
    
   ##### Fixing Character Vectors
    tolower("A") A => a
    toupper("a") a => A
    strsplit("Hello.1", "\\.") # "Hello" "1" 
    sub("_", "", "sorted_by",) # "sortedby"
    sub("_", "", "sorted_by_name",) #"sortedby_name"
    gsub("_", "", "sorted_by_name",) #"sortedbyname"
    grep("Alameda", datatable$var) #positions
    grep("Alameda", datatable$var, value = TRUE) #positions
    table(grepl("Alameda", datatable$var))
    datatable[!grepl("Alameda", datatable$var),]
    nchar("Jeffrey Leek") # number of characters
    substr("Jeffrey Leek", 1, 7) # "Jeffrey"
    paste("Hello", "World") # "Hello World"
    paste("Hello", "World") # "HelloWorld"
    str_trim("Hello    ") # "Hello"
    
##### Regular Expressions
###### Metacharacters
    ^i think # at the begining of the line
    morning$ # at the end of the line
    [Bb] # Match Lower and uppercases
    ^[Ii] am = search "I am" and "i am"
    ^[0-9][a-zA-Z] # range of numbers and range of letters
    [^?.]$ # at the end of the line
    9.1 # search 9_1, 09-10, 09.11, 9/11
    flood|fire # search: firewire, flood, firewall
    ^[Gg]ood|[Bb]ad # Good or good athe the begining, Bad or bad in other positions.
    ^([Gg]ood|[Bb]ad) # Good/good/Bad/bad a the the begining of the line.
    [Gg]eorge( [Ww]\.)? [Bb]ush #
    (.*) # Search somithing between ()
    [0-9]+ (.*)[0-9]+ # any number +repetitive ....
    [Bb]ush( +[^ ]+ +){1,5} debate
    m,n # at least m but not more than n matches
    m # exactly m matches
    m, # At least m matches
    +([a-zA-Z]+) +\1 + # Repetitions
    ^s(.*)s # between s
    
##### Working with dates
    date() #character
    Sys.Date() #Date
    format(d, "%a %b %d"
    z = as.Dates(x, "%d%b%Y")
    as.numeric(date1-date2)
    library(lubridate)
    mdy("08/04/2013") # "2013-08-04 UTC"
    mdy_hms("08/04/2013 10:15:03") # "2013-08-04 10:15:03 UTC"
    mdy_hms("08/04/2013 10:15:03", tz="Pacific/Auckland") # "2013-08-04 10:15:03 NZST"
   [tutorial lubridate](https://lubridate.tidyverse.org/)
   
   [formats of dates](https://www.statmethods.net/input/dates.html)

##### Plots
###### Base
    boxplot(pollution$pm25, col = "blue")
    hist(pollution$pm25, col = "green")
    rug(pollution$pm25)
    boxplot(pollution$pm25, col = "blue")
    abline(h = 12)
    abline(v = 12, lwd = 2)
    abline(v = 12, lwd = 2)
    boxplot(pollution$pm25, col = "blue")
    hist(pollution$pm25, col = "green")
    abline(v = 12, lwd = 2)
    abline(v = median(pollution$pm25), col = "magenta", lwd = 4)
    barplot(table(pollution$region), col = "wheat", main = "Number of Counties in each Region")
    boxplot(pm25 ~ region, data = pollution, col = "red")
    par(mfrow = c(2, 1), mar = c(4, 4, 2, 1))
    hist(subset(pollution, region = "east")$pm25, col = "green")
    hist(subset(pollution, region = "west")$pm25, col = "green")
    with(pollution, plot(latitude, pm25))
    abline(h = 12, lwd = 2, lty =2)
    with(pollution, plot(latitude, pm25), col = region)
    with(pollution, plot(latitude, pm25, col = region))
    abline(h = 12, lwd = 2, lty =2)
    par(mfrow = c(1, 2), mar = c(5, 4, 2, 1))
    with(subset(pollution, region == "west"), plot(latitude, pm25, main = "West"))
    with(subset(pollution, region == "east"), plot(latitude, pm25, main = "East"))
###### ggplot2
    qplot(displ, hwy, mpg)
    qplot(x = displ, y = hwy, data = mpg)    
    qplot(x = displ, y = c(hwy, cty), data = mpg)    
    qplot(x = displ, y = hwy, data = mpg, color = drv)
    qplot(x = displ, y = hwy, data = mpg, color = drv, geom = c("point", "smooth"))
    qplot(hwy, mpg, color = drv)
    qplot(y = hwy, data = mpg, color = drv)    
    qplot(drv, hwy, data = mpg, geom = "boxplot")
    qplot(drv, hwy, data = mpg, geom = "boxplot", color = manufacturer)
    qplot(hwy, data = mpg, fill = drv)
    qplot(displ, hwy, data = mpg, facets = .~drv)    
    qplot(hwy, data = mpg, facets = drv~., binwidth = 2)
    
##### Clean variables into the workspace
    rm(list = ls())

##### Links
[Tutorial plyr] (http://plyr.had.co.nz/09-user/)

[Competitions in data Science] (https://lubridate.tidyverse.org/)
