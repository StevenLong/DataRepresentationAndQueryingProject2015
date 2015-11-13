#API For Accessing And Organising Higher Education Statistics
## Data Representation And Querying Project 2015
###-- By Steven Long 
###-- G00317230

##Introduction
  This project provides the design and documentation for an API which allows easy access and organisation of the dataset "Enrolments (full-time, part-time and remote) by Level, Field of Study (ISCED) and Gender" which covers the 2014/2015 academic year and can be found at http://www.hea.ie/node/1557. 

  It should be noted that the dataset used here is used for example, and this API can be applied to the dataset from any other academic year much to the same effect.

##Dataset Overview
  This dataset contains data for all full-time, part-time and remote enrolments for Irish Universities, Colleges and Institutes of Technology, organised by gender of the applicant and the field of study to which they have enrolled. The dataset contains total number of enrolments for all Universities, Colleges and ITs, as well as totals for each individual University, College and IT. All of these totals are available as a male/female breakdown as well. The dataset was received in .xlsx (Microsoft Excel Open XML Document) format, and was downloaded from (http://www.hea.ie/node/1557). The dataset contains 3 tables, one for Universities, one for Colleges, and one for Institutes of Technology. Each row of the dataset represents a field to which students can enrol (for example, Honours Bachelor Degrees), or represents a total of a type of field (for example, total undergraduates), or grand total. Three columns are dedicated to each institute, one for male enrolments, one for female enrolments, and one for total enrolments. The last three columns are dedicated to the totals of the other columns, one for total male enrolments, one for total female enrolments, and one as a grand total for that row.

###Data types
  The 3 tables contain roughly the same types of data. The data is broken down into enrolment numbers for each institute, and is then further broken down into male and female numbers. They can be represented as such:
  - *Institute*: the institute to which the data belongs.
    - ***Field of Study***: The field of study to which these enrolments belong.
      - **Male**: the number of enrolments by males for this field of study.
      - **Female**: the number of enrolments by females for this field of study.
      - **Total**: the total number of enrolments for this field of study in this institution .
  
  The tables also contain totals for each institute regardless of field of study. These can be represented as such:
  - *Institute*: The institute to which the data belongs.
     - **Male**: the total number of enrolments to this institute by males.
     - **Female**: the total number of enrolments to this institute by females.
     - **Total**: the total number of enrolments to this institute.
  
  The tables also contain total enrolments to all institutes contained in that table. These can be represented as such:
  - *Field of Study*: The field of study to which these enrolments belong.
   - **Total Males**: The total number of enrolments by males to this field of study.
   - **Total Females**: The total number of enrolments by females to this field of study.
   - **Total Enrolments**: The total number of enrolments to this field of study.

  The tables also contain the total number of enrolments for all institutes of this type. This can be represented as such:
  - *Type of Institute*: The type of institute to which this data belongs; one of University, College, or Institute of Technology.
    - **Total Males**: The overall total number of male enrolments to this type of institute.
    - **Total Females**: The overall total number of female enrolments to this type of institute.
    - **Total Enrolments**: The overall total number of enrolments to this type of institute.

  Finally, the totals in each table are subtotalled by the type of fields they belong to. This can be represented as such:
  - *Total Type*: The field to which the subtotal belongs.
   - **Total Male**: The number of male enrolments for this field.
   - **Total Female**: The number of female enrolments for this field.
   - **Total Enrolments**: The number of enrolments to this field type.

##API Design
  The data can be retrieved via a set of URLs which, when accessed using the GET method, will return the relevant data in JSON format. In order to retrieve data the user must access the relevant URL. The URLs are formatted as follows:
  
 - All of the URLs start *http://heastats.com/*
 - The path after the above URL determines the data returned
 - Using the URL *http://heastats.com/[institute]*, where *[institute]* is replaced with the name of the institute for which data is desired, will return the data from every row and column related to that institute
 - Using the URL *http://heastats.com/[institute]/[field]*, where *[institute]* is replaced with the name of the institute and *[field]* is replaced with the name of the field for which information is desired, will return all columns pertaining to that field in that institute.
 - Using the URL *http://heastats.com/[ins_type]/[total_type]*, where *[ins_type]* is replaced with the type of institute and *[total_type]* is replaced with the desired total, will return all rows and columns pertaining to the desired total type for that institute type

  
  The following are some working examples of the API:
  
  Using the URL: *http://heastats.com/its/hbd*
will return the enrolment numbers for Honours Bachelor Degrees in institutes of technology. The response to this would look like:`

        ```json
        [{"institute": "Athlone IT", "male": 873, "female": 679, "total": 1322}, {"institute": "Cork IT", "male": 2445, "female": 1762, "total": 4207}, {...}, ...]
        ```

  The above data represents:
  
  
  Type | Name | Description |
  -----|------|-------------|
  String | institute |  the name of the institute |
  Integer | male | the number of male enrolments for this institute under the requested field (in this case Honours Bachelor Degrees) |
  Integer | female | the number of female enrolments for this institute under the requested field (in this case Honours Bachelor Degrees) |
  Integer | total | the number of enrolments for this institute under the requested field (in this case Honours Bachelor Degrees) |
  
  Using the URL: *http://heastats.com/gmit/totals*
will return the enrolment number totals and subtotals for the selected institute (in this case GMIT). The response to this would look like:

          ```json
          [{"Type": "FETAC", "Total Male": 193, "Total Female": 18, "Total Enrolments": 211}, {"Type": "Undergraduate Total", "Total Male": 3640, ...}, {...}, ...]
          ```

  The above data represents:
  
  Type | Name | Description |
  -----|------|-------------|
  String | Type | The type of field which to the enrolment totals and subtotals belong |
  Integer | Total Male | The total number of male enrolments for this field |
  Integer | Total Female | The total number of female enrolments for this field |
  Integer | Total Enrolments | The total number of enrolments for this field |
  
  Using the URL: *http://heastats.com/its/totals* 
will return enrolment number totals and subtotals for the selected type of institute(in this case institutes of technology). The response to this would look like:

         ```json
         [{"Type": "FETAC", "Male": 2609, "Female:" 217, "Total": 2826}, {...}, ..., {"Type": "Total", "Male": 52606, "Female": 38407, "Total": 91013}]
         ```

  The above data represents:

  Type | Name | Description |
  -----|------|-------------|
  String | Type | The type of total or subtotal; one of FETAC, Undergraduate, Postgraduate, or Total |
  Integer | Male | The number of enrolments by males for this total or subtotal |
  Integer | Female | The number of enrolments by females for this total or subtotal |
  Integer | Total | The total number of enrolments. |

##Users
  
  The type of users this API could be useful to is a broad and varied group. One of the primary uses for this API is to help determine the proper allocation of funding and resources for both institutes and for the various fields those institutes provide. It can also be used to determine overall interest in the individual fields, allowing institutes to schedule more of less of said fields as needed. It can also be used, in conjunction with course completion data, to determine success rates of courses, as well as the number of newly qualified individuals that will likely be entering the workforce. Lastly, because the data is grouped by male and female, the API could be useful to those who are analysing the higher education enrolment numbers of these demographics.
