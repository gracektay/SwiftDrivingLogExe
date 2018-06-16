# SwiftDrivingLogExe
Developer: Grace Tay
Xcode Version 9.2
Apple Swift version 4.0.3

## Build Instructions
```
To Configure
    git clone https://github.com/gracektay/SwiftDrivingLogExe.git 
    cd SwiftDrivingLogExe/
    git submodule init
    git submodule update
  
To Run
	swift run SwiftDrivingLogExe data.txt

To Test
	cd SwiftDrivingLog/
	swift test
```

## Planning
---
* Broke down the assignment into a pseudocode style checklist to think through the problem
* Opened up playground to code the functionality and test out the best approach.
* After getting the program to print out values that were correct functionally, I transitioned to a command line project.
* I am going to break down my thought process into the main files 
	* Extensions
		* This area just had one computed property that is a Double, rounds itself, and converts itself to an Int
		* If I did not create this extension, I would write out Int(round(doubleValue)). It is a bit a more readable to write  doubleValue.roundedInt, especially since I had to write it a handful of times.
	* Driver
		* 	I selected a class for Driver, since it has a property, trips, that will be constantly updated in the future. 
		* In addition to description, I calculate total miles driven and average speed for each driver as computed properties, both of which are referenced in the “description property”.
		* 	I conformed Driver to the CustomStringConvertible for its description. This allows me to only type out print(driver) and it will return the appropriate string for miles driven at the computed speed.
	* Trip
		* 	I selected a struct for Trip because it encapsulates a few simple variables. I also wanted a value type so I can “copy and paste” each trip, instead of passing a reference to it.
		* I use a failable initializer with a guard statement to validate the parameters being passed in.
	* Parser
		* Structs are a bit safer in Swift, so I prefer to use them when possible.
		* To prevent calling DateFormatter too many times, I pass it in as a parameter. DateFormatter() is only called once
		* I use an enum to clearly identify cases of “Driver” and “Trip”, anything else if default and is later used to simply ignore commands that do not abide by either of those cases.
		* Its primary function is to create the log, which reads in a file (input via command line) and returns an array of drivers. First, it takes the data and breaks it down into an array separated by new lines. It then goes through each item from that array and calls my private functions:
			* I determine the command from the line, returning the command type and array of each word from the line, separated by spaces.
			* Depending on the command type, I have a switch case to figure out if the program should create and return a driver or a trip. I append the driver to an array.
		* After we loop through each line in the data file I return the driver log (Driver array).
	* TrackDriving
		* I createLog from that file using my parser and it returns an array of drivers
		* I opted to sort my driver log here. To me, the sorting should not be the responsibility of any other struct or class. The sorting is something that may want to be changed by the user of the program easily.
			* If this were a more complex program, I would likely create a function that takes in an array (i.e. driver log), a value (i.e. total miles driven), and a sort by enum value (i.e. ascending, descending, data)
* After creating this project, I realized the easiest way to connect this to terminal was to create a package via Terminal using “swift package init --type library” and an executable through “swift package init --type executable”.

# Testing
---
* Test create driver to make sure the user can log a driver.
* Test description on driver
	* This computed property is based on other computed properties, so it can also verify that the double.roundedint property will work and that totalMilesDriven and averageSpeed compute from trips.
* The driver  description depends on an accurate trip duration, therefore I must verify that a trip can be created manually and test that computed properties equal out to what is expected.
* test_createDriverWithTrip function will see what happens with different types of data. Create Log will also test the private functions called in the createLog function: determineCommand, createDriver, and createTrip.
* test_rootExampleData uses the provided data and makes sure it works as expected 
* I reached an overall coverage of 85% according to Xcode and my output worked as intended, so I finished my testing phase.
