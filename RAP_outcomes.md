# Reproducible Analysis Approaches in Government

There are lots of different approaches and levels of sophistication to producing reproducible analysis in government. I thought it would be useful to lay this out to help people understand why you would use various approaches.

This article aims to address confusion around how and why a more sophisticated approach is taken.

I will refer to all data processing and analysis pipelines simply as pipelines.

## Automation
Automation is achieved by codifying pipelines. This can be done with legacy tools such as SPSS, VBA, SAS; or modern tools such as Python and R.

### Benefits
1. Automation removes the possibility of errors and mistakes that can be introduced by manual processes, for example copy and paste errors.  
1. Automation can increase efficiency by removing the, often time consuming, manual data processing. For example in Excel, or point and click functionality of tools like SPSS. This is efficiency is especially important in cases where the data processing might need to be run multiple times. For example, the underlying data changes, or a mistake was found during quality assurance of the outputs. The entire process might need to be repeated, and this could happen multiple times. Automation means that time spent rerunning the pipeline is trivial.  
1. It is also a necessary precursor to the below approaches for reproducibility and consistency.
1. Improves transparency and trust. If the data process is codified, by reading the data directly from it's source, and no data is 'hard-coded', then the code can often be shared as open source code, without disclosing any sensitive information in the underlying data. Whereas for example, a pipeline comprising of excel files usually cannot be shared as easily.  
For example, for a statistical publication, all underlying data can be stored in a CSV file(s). The automaton code reads the data directly from this CSV to populate all figures in the publication: summary tables, figures interspersed in text, self serve data tools, etc. This approach builds trust as the CSV is a single source of truth, the exact steps taken do process the data are shared, and eliminates any 'copy and pasting' errors.

### Comparison of tools
Whilst legacy tools are all mostly capable of reading and processing data, they have limitations. 
Modern tools have better support for:  
1. Reading a diverse range of data formats. VBA is primarily for working with Excel data and isn’t as easy to work with external data such as CSV, APIs, databases.  
1. Web scraping and APIs. SPSS and SAS have no support for web scraping or consuming data from APIs.
make table highlighting which tools have different functionality.
When automating, it is best practice to modularise code to simplify it, make it more easily reusable, easier to maintain, and quicker to debug. Most tools provide support for writing functions.

This approach still has drawbacks. Tools like SAS and SPSS have limited functionality which means part of the pipeline needs to be performed in Excel, which adds manual components to the pipeline.

Appropriate tools/languages for different functionality
Tool/Language|APIs|Databases|Web Scraping|manipulate excel files|create PDFS|create web pages
---|---|---|---
SPSS|||
SAS||*|
VBA|*||*
R|*|*|*
Python|*|*|*

## Reproducibility
Reproducibility is about ensuring any particular run of a pipline can be reproduced. If the pipeline has been automated, then the same person running the same code on the same machine might be able to rerun the pipeline to produce the same results. However, we often want for anybody, using any machine, to be able to exactly reproduce the results. This requires a few things:
1. The same version of the code is used. The best way to do this is to version control your code with github. This means the version of your code used for the run in question can be locked and recorded as a snapshot (commit), and the code can be easily shared with others via services like github.  
1. The same version of the software and it's dependencies are used. If the code is rerun at a later date, the user may have installed a more recent version of the software. With legacy tools this is generally not a problem as the software is a complete package. Open source tools like R and Python have different dependencies to run on different machines, and make use of user written packages. R has some limited tools to help with this, Python has mature, reliable tools which solve this.
1. The code itself is designed to run on multiple operating systems. For example, Windows uses different path notation than linux based operating systems like Mac OS.
1. The code is written to run directly from source data that other users will also have access to.
1. It is not completely necessary, but it is far better if the software is designed in a way that to use it is intuitive and requires minimal documentation, this forces developers to consider the usability of their code and not rely on increasingly complex instructions.  The documentation that there is, is stored in the code in either a README.md or docs files. Useful information can be stored in comments in the code but crtitical information should be made as readily availble to the user as possible, ideally in the README or docs. It is better to store instructions as part of the project as it is better for ensuring the instructions are mainted, updated, and distributed along with the code. The fact that the instructions are not tied to the process and tools, means that as tools get updated to new versions, or the process gets updated, the document becomes inaccurate and potentially unusable.
For example, users can check commits to see when docs where last updated. Instructions stored in shared drives have no version control, and require the user to know where they are saved, and have access to the shared drive.


Where raw data is stored  
Even if the most sophisticated approach is taken, if the location of the raw data cannot be readily found, the pipeline fails at the first hurdle. For example, if our data is excel files or csvs, that are stored on a drive or server somewhere, they might be moved, renamed, or moved to a new updated server, all of which will break previous file paths. The ideal situation is to aim to get data from the source, e.g. the database where it is stored and csvs etc are output from. The source database is far less likely to move location or be renamed, since it is part of would developers would refer to as a production environment, and is maintained by developers. Reproducibility is default in the world of developers and as such things typically remain constant since it is part of standard working practice (this is part of the reason why it can be hard to reform and innovate IT).

## Integration
If we have automated our pipeline, and made it reproducible, we have the possibility of integrating our code into other systems, for example:
using the same data cleaning functions in a data tool/dashboard.
Running on a platform like the GSS data tool, for example integrating a statistical publication with the GSS data project.
This requires using industry standard tools like Python, R is not a proper programming language.

A better approach is to automate the process entirely, for example using VBA, Python, or R to programmatically read in the csv.

Git is needed for version control.
Environments need to be managed to ensure code executes as expected. Taking account of difference between, for example, Windows and OSx.

When?  
Reproducibility is incredibly important for analysis to ensure it can be properly QA’d. open analysis to ensure that it is actually useable
It is also important even for non open analysis, to ensure that results can be audited and veryify internally that 

## Consistency
For an adhoc piece of analysis, it might be sufficient to write analysis code from scratch.
For repeated analysis, for example something like a statistical publication, it is important to ensure that the results of each analysis are consistent where expected. This means using the same data analysis functions, package versions, etc between analysis. The best way to achieve this is to store code that does the data analysis separately as ‘source code’, and for each publication of the analysis, code is written specific to that analysis, for example presentation or data sources change, but the source code performs the analysis which ensures it is consistent across publications.
There are times that the source will need to be added to. In this case it is important to have tests written to ensure it still produces the same results for all the previous publications it was used for.
In the case where the source code needs to be updated and is expected to produce different results, the version number is stepped, so that users (and tests) know which version to use to reproduce which publications.
There are also other aspects for which it is important to have consistency. For example the format of outputted data. For statistical publications, it does not cause major problems if number formats, column headings etc change unpredictably. But if the output data is to be integrated into another project it could be essential for the format to remain consistent, in which case this code should also be included in the source code and tests written.

An example of this is separating the 'source code' into an R package that is version controlled, and then called by each publication, ensuring consistency across publications. This is the approach taken by DfE.

## Openness
Openness is important for transparency, to build trust, allow external QA and collaboration. One approach would be to share data analysis code in a public Github repository which is linked to from the publication.
Readability and Best Practices
Within reason, at this point we should be able to reproduce a pipeline no matter what. However, realistically, if the code is not well written enough, it will not be reproduced, at least for the purpose of QA, or reuse for future. Modularisation and readability are therefore critical.

## User Research
Dfe research.

## Integration
To integrate pipelines with other projects, for example a website, or the GSS data 
