# Reproducible Analysis Approaches in Government

There are lots of different approaches and levels of sophistication to producing reproducible analysis in government. This article aims to address confusion around how and why a more sophisticated approach is taken.

I will refer to all data processing and analysis pipelines simply as pipelines.

## Automation
Automation is achieved by codifying pipelines. This can be done with legacy tools such as SPSS, VBA, SAS, or modern tools such as Python and R.

### Benefits
1. Automation removes the possibility of errors and mistakes that can be introduced by manual processes, for example copy and paste errors.  
1. Automation can increase efficiency by removing the need for time consuming manual data processing, for example in Excel, or point and click functionality of tools like SPSS. This is efficiency is especially important in cases where the data processing might need to be run multiple times. For example, the underlying data changes, or a mistake was found during quality assurance of the outputs. The entire process might need to be repeated, and this could happen multiple times. Automation means that time spent rerunning the pipeline is trivial.  
1. Improves transparency and trust. If the data process is codified, reading the data directly from it's source, and no data is 'hard-coded', then the code can often be shared as open source code, without disclosing any sensitive information in the underlying data. Whereas for example, a pipeline comprising of excel files usually cannot be shared as easily.  
For example, for a statistical publication, all underlying data can be stored in a CSV file(s). The automaton code reads the data directly from this CSV to populate all figures in the publication: summary tables, figures interspersed in text, self serve data tools, etc. This approach builds trust as the CSV is a single source of truth, the exact steps taken do process the data are shared, and eliminates any 'copy and pasting' errors.
1. It is also a necessary precursor to the below approaches for reproducibility and consistency.

### Comparison of tools
Whilst legacy tools are all mostly capable of reading and processing data, they have limitations. 
Modern tools have better support for:  
1. Reading a diverse range of data formats. For example, VBA is primarily for working with Excel data and isn’t as easy to work with external data such as CSV, APIs, databases. SPSS and SAS have no support for web scraping or consuming data from APIs.
1. Modularising code. When automating, it is best practice to modularise code to simplify it, make it more easily reusable, easier to maintain, and quicker to debug. Essentatially, writing code as well thought out piece of software, using best practices, rather than one long script or a collection of scripts. Modern tools like Python have much better support for modularising code and writing clean, realiable software.
1. Creating outputs. Modern tools have much better support for producing outputs such as charts, web pages, formatted spreadsheets, etc. Tools like SAS and SPSS have limited functionality which means part of the pipeline might still need to be performed in Excel, which adds manual components to the pipeline.

## Reproducibility
Reproducibility is about ensuring any particular run of a pipline can be reproduced. If the pipeline has been automated, then the same person running the same code on the same machine might be able to rerun the pipeline to produce the same results. However, we often also want anybody (with access to the source data), using any machine, to be able to exactly reproduce the results and outputs. For example, this allows analysis to be peer reviewed by anyone which access to the source data. Even for non-public analysis this is important, to ensure that results can be audited and veryified internally. This requires a few things:
1. The same version of the code is used. The best way to do this is to version control your code with github. This means the version of your code used for the run in question can be locked and recorded as a snapshot (commit), and the code can be easily shared with others via services like github. 
Simply save scripts as v1, v2 etc is not sufficient. There is no way to guarantee or check that any changes to v1 haven't been made since it was last run.
1. The same version of the software and it's dependencies are used. If the code is rerun at a later date, the user may have installed a more recent version of the software. With legacy tools this is generally not a problem as they function as standalone software without dependencies on other software. Open source tools like R and Python have different dependencies to run on different machines, and make use of user written packages. R has some limited tools to help with this, Python has mature, reliable tools which solve this.
1. The code itself is designed to run on multiple operating systems. For example, Windows uses backslash in paths whereas unix based operating systems like OS X use forwardslash.
1. The code is written to run directly from source data that other users will also have access to. Ensuring all parties are using the same data as a single source of truth and cleaning the data in the same way from the same point.
1. Ensuring they are sufficient and accurate instructions to run the pipeline. It is not completely necessary, but it is far better if the software is designed in a way that to use it is intuitive and requires minimal documentation, this forces developers to consider the usability of their code and not rely on increasingly complex wrtitten instructions. The documentation that there is, is stored in the code in either a README.md or docs files. Useful information can be stored in comments in the code but crtitical information should be made as readily availble to the user as possible, ideally in the README or docs. It is better to store instructions as part of the project as it is better for ensuring the instructions are mainted, updated, and distributed along with the code. If instructions are not tied to the process and tools and stored separately as a standalone document, then as code get updated to new versions, the instructions becomes inaccurate and potentially unusable.
For example, users can check commits to see when docs where last updated. Instructions stored in shared drives have no version control, and require the user to know where they are saved, and have access to the shared drive.

## Consistency
Consistency is about separating code for a pipeline into aspects that might be updated for each run of an analysis, from aspects that we want to remain constant for every run of a pipeline. For an adhoc piece of analysis, it might be sufficient to write analysis code from scratch. For repeated analysis, for example something like a statistical publication, it is important to ensure that the results of each analysis are consistent where expected. This means using the same functions for analysis and cleaning, package versions, etc, between analysis. The best way to achieve this is to store functions that do the data cleaning and analysis separately as ‘source code’, and for each publication of the analysis, code is written specific to that analysis, for example presentation or data sources change, but the source code performs the cleaning and analysis which ensures it is consistent across publications.
There are times that the source will need to be added to. In this case it is important to have tests written to ensure it still produces the same results for all the previous publications it was used for.
In the case where the source code needs to be updated and is expected to produce different results, the version number is incremented, so that users (and tests) know which version to use to reproduce which publications.
There are also other aspects for which it is important to have consistency. For example the format of outputted data. For excel files in statistical publications, it does not cause major problems if number formats, column headings, end up getting changed over time and therefore are changing unpredictably. But if the output data is to be integrated into another project it could be essential for the format to remain consistent, in which case this code should also be included in the source code and tests written.

Python has good support for storing reusable code in a package and using the desired version of the package. R also supports packages, but they are more complicated to develop, and R does not readily support using previous versions.

## Openness
Openness is important for transparency, to build trust, allow external QA and collaboration. One approach would be to share data analysis code in a public Github repository which is linked to from the publication.

## Integration
If we have automated our pipeline, and made it reproducible, consistent, and open, written in a well supported language like Python, we will be in a good position integrating our code into other systems, for example:
1. Hosting it on a cloud platform and automating runs.
1. Producing web based products from the outputs, for example a data tool.
1. Use the code to create an API to give users and other systems easy programatic access to the data.
 
## Source data
Even if the most sophisticated approach is taken, if the location of the raw data cannot be readily found, the pipeline fails at the first hurdle. For example, if our data is excel files or csvs, that are stored on a drive or server somewhere, they might be moved, renamed, or moved to a new updated server, all of which will break file paths pointing to the data. The ideal situation is to aim to get data from the source, e.g. the database where it is stored and csvs etc are output from. The source database is far less likely to move location or be renamed, and is more likely to be maintained  properly, using best practice (like making backups), by developers. For developers, reproducibility is critical to ensure services continue to work. It could be a good idea to make use of this to provide a solid foundation for a pipeline.
