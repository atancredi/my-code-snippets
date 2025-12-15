
statisticians should be able to handle the data in whatever state they arrive. It is important to see the raw data, understand the steps in the processing pipeline, and be able to incorporate hidden sources of variability in one's data analysis.

the information you should pass to a statistician:

1. The raw data.
2. A tidy data set
3. A code book describing each variable and its values in the tidy data set.
4. An explicit and exact recipe you used to go from 1 -> 2,3

It is critical that you include the rawest form of the data that you have access to. This ensures that data provenance can be maintained throughout the workflow. 

If you made any modifications of the raw data it is not the raw form of the data. Reporting modified data as raw data is a very common way to slow down the analysis process, since the analyst will often have to do a forensic study of your data to figure out why the raw data looks weird. (Also imagine what would happen if new data arrived?)
## tidy data set

Each variable you measure should be in one column
Each different observation of that variable should be in a different row
There should be one table for each "kind" of variable
If you have multiple tables, they should include a column in the table that allows them to be joined or merged
Add an header row with descriptive column names

share the data in a CSV or TAB-delimited text file
## code book

should contain:

Information about the variables (including units!) in the data set not contained in the tidy data
Information about the summary choices you made
Information about the experimental study design you used
## instructions

you should provide the statistician is something called pseudocode. It should look something like:

Step 1 - take the raw file, run version 3.1.2 of summarize software with parameters a=1, b=2, c=3
Step 2 - run the software separately for each sample
Step 3 - take column three of outputfile.out for each sample and that is the corresponding row in the output data set

You should also include information about which system (Mac/Windows/Linux) you used the software on and whether you tried it more than once to confirm it gave the same results.