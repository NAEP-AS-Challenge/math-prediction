
Importing the data into R or Python
================

We have provided the data in two formats. For importing easily in to R, we provide an `*.rda` format file. We strongly recommend using this file if you plan to work in R. We have also provided the data as a tab delimited text file. We assume most people who are not working in R will use Python and have created the text file format version with that in mind. Of course, the text file can be opened in Excel or OpenOffice and many other software systems, but we have not tested beyond R and Python. 

If using the text file, it's important to know that the original data have non-ascii (or extended "high" ascii) characters that require UTF-8 encoding to maintain full fidelity to the original, for example, mathematical operators and symbols like the pi symbol ($\pi$), multiplication sign ($\times$), greater-than-or-equal-to, etc... Because of this, to safely export the data from R to tab delimited text files, we've used the R `readr` package's `write_excel_csv()` function, which writes faithfully encoded UTF-8 data on our Windows platform. This package also adds the appropriate UTF-8 BOM to help ensure the file will be read correctly by other software. We have tested that the text files written this way can be imported back into R (using the analogous `readr::read_delim()` function with default settings), and that after the text file is read back into R it matches the original. 

## Reading the data into Python

We suggest importing the tab delimited text files into Python using the Pandas `read_csv()` function. While there are many ways to read the data, we have only tested reading the delimited file with Pandas `read_csv()` with the `"\t"` delimiter option (see syntax below). That is the method we recommend, though there may be many other ways that work just as well (e.g. Polars' read_csv()). 

We caution against reading the R (*.rda) format file into Python directly, because the tests we performed using `pyreadr` package to read the R file directly showed discrepancies when tested against a file exported from R into the arrow ipc binary format (which we are not releasing). We strongly recommend not using `pyreadr`, but using Pandas read_csv as shown in the sytnax below. 

### Python syntax for reading tab delimited file 
#### (tested on Python 3.7)
```
import pandas as pd
import csv, os 

os.chdir(r"path_to_files" +"\\")

# use Int64 rather than int since it allows NA values
dtypelist =  {
 "student_id"         :str,
 "accession"          :str,
 "score_to_predict"   :"Int64",
 "predict_from"       :str,
 "year"               :str,
 "srace10"            :"Int64",
 "dsex"               :"Int64",
 "accom2"             :"Int64",
 "iep"                :"Int64",
 "lep"                :"Int64",
 "rater_1"            :str,
 "pta_rtr1"           :str,
 "ptb_rtr1"           :str,
 "ptc_rtr1"           :str,
 "composite"          :str,
 "score"              :str,
 "assigned_score"     :"Int64",
 "ee_use"             :"Int64",
 "parsed_xml_v1"      :str,
 "parsed_xml_v2"      :str,
 "parsed_xml_v3"      :str,
 "source1"            :str,
 "source2"            :str,
 "source3"            :str,
 "source4"            :str,
 "target1"            :str,
 "target2"            :str,
 "target3"            :str,
 "target4"            :str,
 "eliminations"       :str,
 "selected"           :str,
 "eliminated"         :str,
 "selected1"          :str,
 "selected2"          :str,
 "selected3"          :str,
 "selected4"          :str,
 "eliminated1"        :str,
 "eliminated2"        :str,
 "eliminated3"        :str,
 "eliminated4"        :str,
 "selected1.1"        :str,
 "selected2.1"        :str,
 "eliminated1.1"      :str,
 "eliminated2.1"      :str,
 "partA_response_val" :str,
 "partB_response_val" :str,
 "partB_eliminations" :str,
 "response_val"       :str,
 "response_val.1"     :str,
 "response_val.2"     :str}

 x = pd.read_csv("all_items_train_2023-02-17.txt", sep='\t', dtype=dtypelist, na_values= ['N/A', 'NA', 'NULL', 'NaN', 'n/a', 'nan', 'null',''])
 ```

 Even with this syntax there may be slight differences to the coding of missing values between R and Python ("NA" missing values in R may be "NaN" or "None" in Python), but the contents of the files, and most importantly the text of student constructed responses and values of forced choice responses, will match.