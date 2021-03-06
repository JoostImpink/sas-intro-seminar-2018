# Importing and Exporting data

## Importing


### Import from text file (.txt or .csv)


```SAS
filename MYFILE "C:\temp\myData.csv";
data myData;
infile MYFILE dsd delimiter=","  firstobs=2 LRECL=32767 missover;
input id var1 var2 var3;
run;
```

For tab delimited files, use delimiter='09'x. Variables that are text, add a $ after the variable name; e.g. input id var1 $ var2 var3 if var1 is a string. For long strings, add the LENGHT statement after data (but before infile) with the variable length (example: length var1 $20;).


### Import from Excel

```SAS
proc import 
  OUT       = work.myData DATAFILE= "C:\temp\myData.xlsx" 
  DBMS      = xlsx REPLACE;
  SHEET     = "worksheet1"; 
  GETNAMES  = YES;
run;
```

Note that sheet= needs to match the name of the worksheet you want to import (needs to match exactly).

Make sure that the data is clean in Excel. For example, if in a column with numbers there is a cell with 'N/A' then the whole column will be imported as text. Also, columns that contain a date will be correctly imported (and will be a date in SAS as well) as long as all the fields are valid dates.

## Exporting

### Export as csv

The following macro exports a dataset as csv

```SAS
PROC EXPORT DATA= work.myDate OUTFILE="C:\temp\myExportFile.csv" DBMS=CSV REPLACE;
RUN;
```

### Export to Stata

SAS will write in Stata format when the filename ends with .dta, for example: `proc export data=work.myData outfile="C:\temp\exportStata.dta" replace; run;`.

### Export pipe delimited

To use another delimiter (for example a pipe, '|'), use dmbs=dlm with delimiter='|'.

For example: `proc export data=work.myData outfile='c:\myData.txt' dbms=dlm; delimiter='|'; run;`

## Example


### Download the Excelsheet 

Included in the [data folder](datasets/) is an Excelsheet with some data. There are two date columns that are identical, but in one cell the date is replaced by 'N/A'. There are also three columns with numbers; one column has an empty cell, and one column has a cell with the text 'missing'. Import the Excelsheet (the worksheet name is 'Sheet1' in SAS) and inspect the types of the columns.

Download the dataset to some folder on your hard drive, for example `C:\temp'.

```SAS
/* assign folder to macro variable dbaFiles */
%let myFiles = C:\temp; /* note: no quotes needed here */

proc import 
  OUT       = work.myData DATAFILE= "&myFiles.\Dates_Numbers.xlsx" 
  DBMS      = xlsx REPLACE;
  SHEET     = "Sheet1"; 
  GETNAMES  = YES;
run;
```

Inspect the types of the columns of the dataset (doubleclick the column header). 