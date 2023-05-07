Download Link: https://assignmentchef.com/product/solved-homework-1-extracting-data-from-a-csv-file
<br>
This assignment is based on a class of problem solved in enterprise computing; extraction, transformation, and loading. This is often referred to as ETL. The inputs will be data extracted from a leading aviation industry data and consulting firm, GCR. (See <a href="http://www.gcr1.com/5010web/advancedsearch.cfm">GCR.com</a> for additional data.) The data is in a well known format where each data element is separated from the previous and following data elements by using a comma. It should be noted that this method of data manipulation is extremely common. The explicit order of the data fields and the desired outputs are defined in the “Specifications”.

<h1>1          Objectives</h1>

The objectives of this assignment are to demonstrate proficiency in file I/O, data structures, and data transformation using C language resources. Specifically, you will read in data from a text file, use that data to populate a data structure, and print that data to <em>STDOUT </em>by accessing the newly populated structure.

1.1     Extraction

The first part of ETL is extraction. The filename of a text file will be passed to your program via the command line. The data contained in that file is to be read into memory (<em>i.e.</em>, extracted). Your program will be compiled and run on Eustis using the following commands:

gcc -o etl hw1etl.c ./etl <em>inputFile</em>

It is entirely possible that the input file either does not exist or is not where it is supposed to be. In such an event, your program should print an error message to <em>STDERR </em>that indicates which file is missing, then your program should exit safely. Use the following format for your error message (<em>fileName </em>should display

the actual name of the missing file):

etl ERROR: File “<em>fileName</em>” not found.

The input file is in CSV (comma separated values) format where each line contains the data for one airport and the fields are as printed below. Note that these fields vary in size and content. Some fields may even be empty. Also note that the data for some of the fields are a <em>melange </em>of types. Specifically, the FAA Site Number and both latitude and longitude contain numbers, punctuation, and text.

<em>For this assignment, treat all input data as character data.</em>

Table 1: Airports Data Fields

<table width="615">

 <tbody>

  <tr>

   <td width="205">Field Title</td>

   <td width="205">Description</td>

   <td width="205">Size</td>

  </tr>

  <tr>

   <td width="205">FAA Site Number</td>

   <td width="205">Contains leading digits followed by a decimal point and short text</td>

   <td width="205">Leading digits followed by a decimal point and zero to two digits and a letter</td>

  </tr>

  <tr>

   <td width="205">Loc ID</td>

   <td width="205">The airport’s short name, i.e.MCO for Orlando</td>

   <td width="205">4 characters</td>

  </tr>

  <tr>

   <td width="205">Airport Name</td>

   <td width="205">The airport’s full name, i.e. Orlando International</td>

   <td width="205">≤ 50 characters</td>

  </tr>

  <tr>

   <td width="205">Associated City</td>

   <td width="205">The nearest city</td>

   <td width="205">≤ 50 characters</td>

  </tr>

  <tr>

   <td width="205">State</td>

   <td width="205">State</td>

   <td width="205">2 characters</td>

  </tr>

  <tr>

   <td width="205">Region</td>

   <td width="205">FAA Region</td>

   <td width="205">3 characters</td>

  </tr>

  <tr>

   <td width="205">ADO</td>

   <td width="205">Airline Dispatch Office</td>

   <td width="205">3 characters</td>

  </tr>

  <tr>

   <td width="205">Use</td>

   <td width="205">Public or Private</td>

   <td width="205">2 characters</td>

  </tr>

  <tr>

   <td width="205">Latitude</td>

   <td width="205">DD-MM-ss.ssssDirection</td>

   <td width="205">Degrees, minutes, seconds. Direction is either N,S,E or W.Treated as a string (for now).</td>

  </tr>

  <tr>

   <td width="205">Longitude</td>

   <td width="205">See Latitude above.</td>

   <td width="205">ditto</td>

  </tr>

  <tr>

   <td width="205">Airport Ownership</td>

   <td width="205">Public or Private</td>

   <td width="205">2 characters</td>

  </tr>

  <tr>

   <td width="205">Part 139</td>

   <td width="205">FAA Regulation</td>

   <td width="205">No data</td>

  </tr>

  <tr>

   <td width="205">NPIAS Service Level</td>

   <td width="205">National Plan Integrated AirportSystems Descriptor</td>

   <td width="205">≤ 50 characters</td>

  </tr>

  <tr>

   <td width="205">NPIAS Hub Type</td>

   <td width="205">Intentionally left blank</td>

   <td width="205">n/a</td>

  </tr>

  <tr>

   <td width="205">Airport Control Tower</td>

   <td width="205">Y/N</td>

   <td width="205">one character</td>

  </tr>

  <tr>

   <td width="205">Fuel</td>

   <td width="205">Fuel types available</td>

   <td width="205">up to 6 characters</td>

  </tr>

  <tr>

   <td width="205">Other Services</td>

   <td width="205">Collections of tag indicating INSTRuction, etc.</td>

   <td width="205">12 characters</td>

  </tr>

  <tr>

   <td width="205">Based Aircraft Total</td>

   <td width="205">Number of aircraft (may beblank)</td>

   <td width="205">Integer number</td>

  </tr>

  <tr>

   <td width="205">Total Operations</td>

   <td width="205">Takeoffs/Landings/etc (may be blank)</td>

   <td width="205">Integer number</td>

  </tr>

 </tbody>

</table>

1.2     Transformation

The second part of ETL is transformation. A list of comma separated values is convenient for text files, but it is far less convenient in memory. Once the data for a single airport has been read into a buffer, you will need to parse the buffer based on the commas between the data fields. The parsed data will then be used to populate a structure of the type struct airPdata (<em>i.e. </em>the data has been transformed from CSV to a data structure). The format of airPdata, shown below, will be defined in airPdata.h. Note that the airPdata structure uses the same names as the input file’s <em>Field Names </em>(See Table 1 on page 2), though not all of the <em>Field Names </em>are used.

typedef struct airPdata{

char *siteNumber; //FAA Site Number char *LocID; //Airport’s ‘‘Short Name’’, textit{e.g.} MCO char *fieldName; //Airport Name char *city; //Associated City char *state; //State char *latitude; //Latitude char *longitude; //Longitude char controlTower;//Control Tower, this is a single character (Y/N)

} airPdata;

Remember, some of these fields will be of differing lengths for each airport. When you allocate memory structure’s fields, you can assume that no entry will be longer than 50 characters (plus 1 character for the terminating NULL).

1.3     Loading

Finally, the third part of ETL is loading. With the data now in an airPdata structure it can be easily accessed by functions and/or other programs (<em>i.e.</em>, loaded). For this assignment, you will use pass the airPdata structure to a function ( PrintData(airPdata airport) ) that will print the data to <em>STDOUT </em>(aka the console). Before calling PrintData for the first time, make sure you print a header line that names each column. Specifically, use the following two lines of code:

printf(“%-12s %-11s %-42s %-34s %-3s %-15s %-16s Tower
”,

“FAA Site”, “Short Name”, “Airport Name”, “City”, “ST”,

“Latitude”, “Longitude”);

printf(“%-12s %-11s %-42s %-34s %-3s %-15s %-16s =====
”,

“========”, “==========”, “============”, “====”, “==”,

“========”, “=========”);

The “-” preceding each of the format specifiers left-justifies the printed values, while the numbers indicate the width of the printed field. Your data should be left-justified as well and should use widths that are identical to those in the header line. It is your choice whether you want to populate one airPdata structure and then print it, or to populate an array of airPdata structures and then print each of them. If you choose to populate and print one at a time, be sure to free any allocated memory before reallocating for the same variable. If you choose to read in all of the airports before printing them, you will have an easier time modifying your HW1 code when it comes time for HW3 and will only need to free the memory when you are done. Again, the choice is yours. An example of what the output should look like is shown on the next page.




<table width="867">

 <tbody>

  <tr>

   <td width="83">FAA Site</td>

   <td colspan="2" width="351">Short Name Airport Name</td>

   <td width="159">City</td>

   <td width="32">ST</td>

   <td width="242">Latitude                         Longitude                    Tower</td>

  </tr>

  <tr>

   <td width="83">========</td>

   <td colspan="2" width="351">========== ============</td>

   <td width="159">====</td>

   <td width="32">==</td>

   <td width="242">========                     =========                   =====</td>

  </tr>

  <tr>

   <td width="83">03406.20*H</td>

   <td width="77">2FD7</td>

   <td width="274">AIR ORLANDO</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-26-08.0210N 081-28-23.2590W N</td>

  </tr>

  <tr>

   <td width="83">03406.31*H</td>

   <td width="77">3FD5</td>

   <td width="274">ARNOLD PALMER HOSPITAL</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-31-21.0090N 081-22-49.2520W N</td>

  </tr>

  <tr>

   <td width="83">03406.36*H</td>

   <td width="77">2FL5</td>

   <td width="274">BROOKSVILLE INTL AIRWAYS- INC</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-25-26.0000N 081-27-35.0000W N</td>

  </tr>

  <tr>

   <td width="83">03406.24*H</td>

   <td width="77">FD99</td>

   <td width="274">DR P PHILLIPS HOSPITAL</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-25-43.0220N 081-28-38.2590W N</td>

  </tr>

  <tr>

   <td width="83">03408.*A</td>

   <td width="77">ORL</td>

   <td width="274">EXECUTIVE</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-32-43.7000N 081-19-58.5000W Y</td>

  </tr>

  <tr>

   <td width="83">03406.11*H</td>

   <td width="77">37FA</td>

   <td width="274">FLORIDA HOSPITAL</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-34-32.0020N 081-22-06.2490W N</td>

  </tr>

  <tr>

   <td width="83">03406.22*H</td>

   <td width="77">FD36</td>

   <td width="274">FLORIDA HOSPITAL EAST ORLANDO</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-32-26.7000N 081-16-51.0000W N</td>

  </tr>

  <tr>

   <td width="83">03406.40*H</td>

   <td width="77">FL76</td>

   <td width="274">HELI-PARTNERS I-DRIVE</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">27-23-04.0000N 081-29-07.0000W N</td>

  </tr>

  <tr>

   <td width="83">03406.39*H</td>

   <td width="77">97FD</td>

   <td width="274">HELICOPTERS INTL</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-27-51.8300N 081-27-35.8800W N</td>

  </tr>

  <tr>

   <td width="83">03407.2*A</td>

   <td width="77">ISM</td>

   <td width="274">KISSIMMEE GATEWAY</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-17-23.3000N 081-26-13.5000W Y</td>

  </tr>

  <tr>

   <td width="83">03406.*C</td>

   <td width="77">91FL</td>

   <td width="274">LAKE CONWAY NORTH</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-28-45.0140N 081-22-03.2510W N</td>

  </tr>

  <tr>

   <td width="83">03406.33*C</td>

   <td width="77">89FL</td>

   <td width="274">LAKE HIAWASSEE</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-31-45.0100N 081-28-51.2600W N</td>

  </tr>

  <tr>

   <td width="83">03407.15*A</td>

   <td width="77">54FD</td>

   <td width="274">LM-ETS</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-22-03.0000N 081-04-34.0000W N</td>

  </tr>

  <tr>

   <td width="83">03407.09*H</td>

   <td width="77">82FD</td>

   <td width="274">LOCKHEED MARTIN</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-26-48.4900N 081-27-03.6900W N</td>

  </tr>

  <tr>

   <td width="83">03406.18*H</td>

   <td width="77">32FL</td>

   <td width="274">MEYER</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-30-05.0120N 081-26-39.2560W N</td>

  </tr>

  <tr>

   <td width="83">03408.4*H</td>

   <td width="77">27FA</td>

   <td width="274">ORANGE COUNTY SHERIFF’S OFFICE</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-30-27.0110N 081-24-48.2540W N</td>

  </tr>

  <tr>

   <td width="83">03407.*A</td>

   <td width="77">MCO</td>

   <td width="274">ORLANDO INTL</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-25-45.8000N 081-18-32.4000W Y</td>

  </tr>

  <tr>

   <td width="83">03406.21*H</td>

   <td width="77">FD28</td>

   <td width="274">ORLANDO RGNL MEDICAL CENTER</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-31-31.0090N 081-22-37.2510W N</td>

  </tr>

  <tr>

   <td width="83">03407.1*A</td>

   <td width="77">SFB</td>

   <td width="274">ORLANDO SANFORD INTL</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-46-37.1000N 081-14-05.7000W Y</td>

  </tr>

  <tr>

   <td width="83">03406.29*H</td>

   <td width="77">7FA5</td>

   <td width="274">PREMIUM</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-23-21.0000N 081-29-19.0000W N</td>

  </tr>

  <tr>

   <td colspan="2" width="159">03406.113*H 26FA</td>

   <td width="274">PRINCETON HOSPITAL</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-34-06.0040N 081-26-02.2550W N</td>

  </tr>

  <tr>

   <td colspan="2" width="159">03406.14*A           01FA</td>

   <td width="274">RYBOLT RANCH</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-35-21.9970N 081-08-39.2290W N</td>

  </tr>

  <tr>

   <td colspan="2" width="159">03406.38*C           12FL</td>

   <td width="274">TIMBERLACHEN</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-35-34.0000N 081-24-14.0000W N</td>

  </tr>

  <tr>

   <td colspan="2" width="159">03406.34*H           0FL7</td>

   <td width="274">WKMG-TV</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-35-38.7000N 081-25-11.6000W N</td>

  </tr>

  <tr>

   <td colspan="2" width="159">03406.3*H            13FD</td>

   <td width="274">YELVINGTON</td>

   <td width="159">ORLANDO</td>

   <td width="32">FL</td>

   <td width="242">28-31-07.0090N 081-22-59.2520W N</td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="49"></td>

  </tr>

  <tr>

   <td></td>

   <td></td>

  </tr>

 </tbody>

</table>




<h1>2          Required Functions</h1>

void printData(airPdata airport);

Description: Prints the data for a given airport, using the same format as the provided header line.

Input: A pointer to an airPdata structure.

Special Cases: If a NULL pointer is passed to this function, print an error message to <em>STDERR </em>and return from the function without printing anything to <em>STDOUT</em>.

Returns: Nothing

<h1>3          Testing</h1>

There are several possible approaches for parsing the input data. Regardless of the approach you use, make sure to test your code on Eustis <u>even if it works perfectly on your machine </u>. If your code does not compile on Eustis you will receive a 0 for the assignment. There will be four (4) files provided for testing your code, they are as follows.

Table 2: Test Files

<table width="333">

 <tbody>

  <tr>

   <td width="91">Filename</td>

   <td width="243">Description</td>

  </tr>

  <tr>

   <td width="91">twolines.csv</td>

   <td width="243">Two lines of test data, where one line consists of lower case letters, one unique letter per field, the other line will consist of uppercase letters.</td>

  </tr>

  <tr>

   <td width="91">orlando5.csv</td>

   <td width="243">Five lines of Orlando airport data.</td>

  </tr>

  <tr>

   <td width="91">orlando.csv</td>

   <td width="243">All 26 of the Orlando airports.</td>

  </tr>

  <tr>

   <td width="91">florida.csv</td>

   <td width="243">All 877 of Florida’s airports.</td>

  </tr>

 </tbody>

</table>

The expected output for these test cases will also be provided. To compare your output to the expected output you will first need to redirect <em>STDOUT </em>to a text file. Run your code with the following command (substitute the actual name of the input CSV file):

./etl <em>inputFile </em>&gt; output.txt

The run the following command (substitute the actual name of the expected output file):

diff output.txt <em>expectedOutputFile</em>

If there are any differences the relevant lines will be displayed (note that even a single extra space will cause a difference to be detected). If nothing is displayed, then congratulations the outputs match! For each of the four (4) test cases, your code will need to output to <em>STDOUT </em>text that is identical to the corresponding <em>expectedOutputFile</em>. If your code crashes for a particular test case, you will not get credit for that case.





