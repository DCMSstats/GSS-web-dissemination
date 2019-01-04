# Reproducible Analysis Approaches in Government

There are lots of different approaches and levels of sophistication to producing reproducible analysis in government. I thought it would be useful to lay this out to help people understand why they are using a RAP approach, and which approach best meets their needs.

There is often confusion around how and why a more sophisticated approach should be taken, which this article aims to address.

I will refer to all data processing and analysis pipelines simply as pipelines.

## Automation
Automation is achieved by codifying pipelines. This can be done with legacy tools such as SPSS, VBA, SAS. Or modern tools such as Python or R.

Why? 
Automation can increase efficiency by removing the often time consuming of processing data manually, for example in Excel, or point and click functionality of tools like SPSS. This is especially important in cases where for example, a time consuming pipeline has been manually processed, and then the underlying data needs to be changed or a mistake was made in the process. The entire process might need to be repeated, and this could happen multiple times. Automation means that time spent rerunning the pipeline is trivial.  
Automation removes the possibility of errors and mistakes that can be introduced by manual processes, for example copy and paste errors.

When to use?  
Automation is important to ensure value for money and efficiency especially when manual pipelines take non-trivial amounts of time to process. For example if a process takes 2 days and is rerun multiple times, automation would save a significant amount of time.
It is also a necessary precursor to the below approaches for reproducibility and consistency.

Comparison of tools:  
Whilst legacy tools are all mostly capable of reading and processing data, they have limitations. 
Modern tools have better support for:  
reading a diverse range of data formats.  
VBA is primarily for working with Excel data and isn’t as easy to work with external data such as CSV.  
Web scraping and APIs. SPSS and SAS have no support for web scraping.  

## Reproducibility
Reproducibility allows other to rerun your analysis. There is varying levels of sophistication to reproducibility.
Access to underlying data. All pipelines are only reproducible if you have access to the underlying data. Ensuring that the correct people have access to the underlying data is important.

Naive approach:
The most basic approach to reproducibility is to write a desk notes style document detailing the steps someone can take to go from raw data to output. This could be something like:  
Load /shared-drive/data/raw_data2010.csv into SPSS  
Choose summarise by category and year from the dropdown menu  
Output to excel  
Copy output into output tab in /share-drive/publications/publication_2010.xlsx  

The fact that the instructions are not tied to the process and tools, means that as tools get updated to new versions, or the process gets updated, the document becomes inaccurate and potentially unusable.

A better approach is to codify the process as much as possible. You could create an SPSS script which loads the data, summarises and saves to Excel. The scripts itself documents the process so we aren’t reliant on up to date documentation.

This approach still has drawbacks. Tools like SAS and SPSS have limited functionality which means part of the pipeline needs to be performed in Excel, which adds manual components to the pipeline.

Using Python or R typically means the entire pipeline can be codified in script, since they have support reading the vast majority of data formats, can programmatically excel files and pdfs or word documents etc. examples

All things being equal, if I write an R or Python script for my pipeline, I can’t rerun it as many times as I want, producing identical output, with the click off a button. However we still face limitations. If a colleague

Where raw data is stored  
Even if the most sophisticated approach is taken, if the location of the raw data cannot be readily found, the pipeline fails at the first hurdle. For example, if our data is excel files or csvs, that are stored on a drive or server somewhere, they might be moved, renamed, or moved to a new updated server, all of which will break previous file paths. The ideal situation is to aim to get data from the source, e.g. the database where it is stored and csvs etc are output from. The source database is far less likely to move location or be renamed, since it is part of would developers would refer to as a production environment, and is maintained by developers. Reproducibility is default in the world of developers and as such things typically remain constant since it is part of standard working practice (this is part of the reason why it can be hard to reform and innovate IT).

Codifying our pipeline, and using industry standard tools like Python, allows us to integrate with other systems, for example integrating a statistical publication with the GSS data project.

A better approach is to automate the process entirely, for example using VBA, Python, or R to programmatically read in the csv.

Git is needed for version control.
Environments need to be managed to ensure code executes as expected.

When?  
Reproducibility is incredibly important for analysis to ensure it can be properly QA’d. open analysis to ensure that it is actually useable
It is also important even for non open analysis, to ensure that results can be audited and veryify internally that 
Consistency
For an adhoc piece of analysis, it might be sufficient to write analysis code from scratch.
For repeated analysis, for example something like a statistical publication, it is important to ensure that the results of each analysis are consistent where expected. This means using the same data analysis functions, package versions, etc between analysis. The best way to achieve this is to store code that does the data analysis separately as ‘source code’, and for each publication of the analysis, code is written specific to that analysis, for example presentation or data sources change, but the source code performs the analysis which ensures it is consistent across publications.
There are times that the source will need to be added to. In this case it is important to have tests written to ensure it still produces the same results for all the previous publications it was used for.
In the case where the source code needs to be updated and is expected to produce different results, the version number is stepped, so that users (and tests) know which version to use to reproduce which publications.

## Openness
Openness is important for transparency, to build trust, allow external QA and collaboration. One approach would be to share data analysis code in a public Github repository which is linked to from the publication.
Readability and Best Practices
Within reason, at this point we should be able to reproduce a pipeline no matter what. However, realistically, if the code is not well written enough, it will not be reproduced, at least for the purpose of QA, or reuse for future. Modularisation and readability are therefore critical.

## User Research
Dfe research.

## Integration
To integrate pipelines with other projects, for example a website, or the GSS data 
