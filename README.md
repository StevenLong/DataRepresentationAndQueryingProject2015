#API For Accessing And Organising Higher Education Statistics
## Data Representation And Querying Project 2015
#By Steven Long

##Introduction
  This project provides the design and documentation for the dataset "Enrolments (full-time, part-time and remote) by Level, Field of Study (ISCED) and Gender" which covers the 2014/2015 academic year and can be found at http://www.hea.ie/node/1557.

##Dataset Overview
  This dataset contains data for all full-time, part-time and remote enrolments for Irish Unversities, Colleges and Institutes of Technology, organised by gender of the applicant and the field of study to which they have enrolled. The dataset contains total number of enrolments for all Universities, Colleges and ITs, as well as totals for each individual University, College and IT. All of these totals are available as a male/female breakdown as well. The dataset was recieved in .xlsx (Microsoft Excel Open XML Document) format, and was downloaded from (http://www.hea.ie/node/1557).

#Data types
  The dataset contains 3 tables, one for Universities, one for Colleges, and one for Institutes of Technology. These tables contain roughly the same types of data. The data is broken down into enrolment numbers for each institute, and is then further broken down into male and female numbers. They can be represented as such:
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
  
  The tables also contain total enrolments to all institutes contained in that table. These can br represented as such:
  - *Type of Institute*: The type of institute to which this data belongs; one of University, College, or Institute of Technology.
    - ***Field of Study***: The field of study to which these enrolments belong.
      - **Total Males**: The total number of enrolments by males to this field of study.
      - **Total Females**: The total number of enrolments by females to this field of study.
      - **Total Enrolments**: The total number of enrolments to this field of study.

  Finally, the tables also contain the total number of enrolments for all institutes of this type. This can be represented as such:
  - *Type of Institute*: The type of institute to which this data belongs; one of University, College, or Institute of Technology.
    - **Total Males**: The overall total number of male enrolments to this type of institute.
    - **Total Females**: The overall total number of female enrolments to this type of institute.
    - **Total Enrolments**: The overall total number of enrolments to this type of institute.

##API Design
  The data can be retrieved via a set of URLs which, when accessed using the GET method, will return the relevant data in JSON format. The following are some examples of this:
  
  Using the URL: *http://heastats.com/its/hbd*
will return the enrolment numbers for Honours Bachelor Degrees in institutes of technology. The response to this would look like:`

        ```json
        [{"institute": "Athlone IT", "male": 873, "female": 679, "total": 1322}, {"institute": "Cork IT", "male": 2445, "female": 1762, "total": 4207}, {...}, ...]
        ```

  The above data represents:
  - **institute**: the name of the institute.
    - *male*: the number of male enrolments for this institute under the requested field (in this case Honours Bachelor Degrees).
    - *females*: the number of female enrolments for this institute under the requested field (in this case Honours Bachelor Degrees).
    - *totals*: the number of enrolments for this institute under the requested field (in this case Honours Bachelor Degrees).



##Users
