# RCode-for-Electricity-Power-Plots

## Step 1: Calculate Memory Req's

setwd("C:/Users/gsgau/Desktop/coursera")

data <- read.table("household_power_consumption.txt", header = TRUE, sep =";", stringsAsFactors = FALSE)

## Memory calcultaion

memory <- nrow(data)*ncol(data)*8

names(data)

names(data) <- tolower(names(data))

sum(data$voltage == "?")         ## check "?" in 'data'

data[data == "?"] <- NA          ## replacing ? with NA in the data

sum(is.na(data$voltage))         ## check number of of NA 

str(data)

data[,3:8] <- lapply(data[,3:8], as.numeric)	## Converting the 3 to 8 columns to numeric

data$date <-strptime(as.character(data$date),"%d/%m/%Y")	##Changing date format from char

str(data)

subdata <- subset(data, date == "2007-02-01" | date =="2007-02-02")

rm(data)  #clearing the space

## Plot 1

png("plot1.png", width = 480, height = 480)
  
with(subdata, hist(global_active_power, main = "Global Active Power", col = "red", ylab = "Frequency", xlab = "Global Active Power (kilowatts)"))
 
dev.off()
 

data <- read.table("household_power_consumption.txt", header = TRUE, sep =";", stringsAsFactors = FALSE)

names(data) <- tolower(names(data))

subdata <- subset(data, date == "1/2/2007" | date =="2/2/2007"); 

rm(data)  #clear the space

subdata[,3:8] <- lapply(subdata[,3:8], as.numeric)

subdata$date_time <- strptime(paste(subdata$date, subdata$time, sep = ""), "%d/%m/%Y %H:%M:%S") ## Combining date and time and converting to time-date formate from character


## Plot 2

png("plot2.png", width = 480, height = 480)

with(subdata, plot(date_time, global_active_power, type = "l", col = "red", xlab = "", ylab = "Global Active Power (kilowatts)"))

dev.off()
      


## Plot 3:

png("plot3.png", width = 480, height = 480)

with(subdata, plot(x = date_time, y = sub_metering_1, type = "l", col = "black", xlab = "", ylab = "Energy sub-metering" ))

with(subdata, lines(date_time, sub_metering_2, type = "l", col = "red"))

with(subdata, lines(date_time, sub_metering_3, type = "l", col = "blue"))

legend("topright", c("Sub_metering_1", "Sub_metering_2", "Sub_metering_3"), lty=1, lwd=2, col=c("black", "red", "blue"))

dev.off()
      

## Plot 4
png("plot4.png", width = 480, height = 480)

par(mfrow = c(2,2))

with(subdata, plot(date_time, global_active_power, type ="l", col = "black", xlab = "", ylab = "Global Active Power"))

with(subdata, plot(date_time, voltage, type ="l", col = "black", xlab = "", ylab = "Voltage"))

with(subdata, plot(x = date_time, y = sub_metering_1, type = "l", col = "gray", xlab = "", ylab = "Energy sub-metering"))

with(subdata, lines(date_time, sub_metering_2, type = "l", col = "red"))

with(subdata, lines(date_time, sub_metering_3, type = "l", col = "blue"))

legend("topright", c("Sub_metering_1", "Sub_metering_2", "Sub_metering_3"), lty=1, lwd=2, col=c("gray", "red", "blue"))

with(subdata, plot(date_time, global_reactive_power, type ="l", col = "black", xlab = "", ylab = "Global_Reactive_Power"))

dev.off()
