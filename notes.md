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

#Group
by_package <- group_by(cran, package)
summarize(by_package, mean(size))
