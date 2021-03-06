# utl_split_a_dataset_by_sets_of_variables_using_sql
utl_split_a_dataset_by_sets_of_variables_using_sql. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverfl SAS community.
    Split a dataset by sets of variables using SQL

    Same results in WPS and SAS

    github
    https://github.com/rogerjdeangelis/utl_split_a_dataset_by_sets_of_variables_using_sql

    see for do_over macro
    https://goo.gl/EUYyaB
    https://github.com/rogerjdeangelis/utl_sql_looping_or_using_arrays_in_sql_do_over_macro/blob/master/utl_sql_looping_or_using_arrays_in_sql_do_over


    stackoverflow
    https://stackoverflow.com/questions/48969348/sas-proc-sql-select-columns-belonging-together

    INPUT
    =====

       RULES (USE SQL and create two datasets C1 and C2

        1.  Create dataset C1 with varaibles _C10_--_C19_
        2.  Create dataset C2 with varaibles _C20_--_C29_


     WORK.HAVE  total obs=1

     Obs _C10_ _C11_ _C12_ _C13_ _C14_ _C15_ _C16_ _C17_ _C18_ _C19_

      1  37851 52717  6400 35160 29928  8803 45861 11191 34847 13532

     Obs _C20_ _C21_ _C22_ _C23_  _C24_ _C25_ _C26_ _C27_ _C28_ _C29_

      1  42999 38913  5202 32790 100150 35176 43656 61507  9979 37850


    PROCESS
    =======

      proc sql;
         create
            table c1 as
         select
            %array(Cs,values=1-9)
            %do_over(cs,phrase=_C1?_,between=comma)
         from
            have
         ;
         create
            table c2 as
         select
            %do_over(cs,phrase=_C2?_,between=comma)
         from
            have
         ;
      quit;


    OUTPUT
    ======

     WORK.C1 total obs=1

      Obs _C11_ _C12_ _C13_ _C14_ _C15_ _C16_ _C17_ _C18_ _C19_

       1  52717  6400 35160 29928  8803 45861 11191 34847 13532


     WORK.C1 total obs=1

      Obs _C21_ _C22_ _C23_  _C24_ _C25_ _C26_ _C27_ _C28_ _C29_

       1  38913  5202 32790 100150 35176 43656 61507  9979 37850

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    proc report data=sashelp.cars nowd missing out=have(keep=_C10_--_C29_);
    cols make, weight;
    define make / across;
    define weight / sum;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;


    * SAS;
    proc sql;
       create
          table c1 as
       select
          %array(Cs,values=1-9)
          %do_over(cs,phrase=_C1?_,between=comma)
       from
          have
       ;
       create
          table c2 as
       select
          %do_over(cs,phrase=_C2?_,between=comma)
       from
          have
       ;
    quit;


    * WPS;
    %utl_submit_wps64('
    libname wrk "%sysfunc(pathname(work))";

     proc sql;
       create
          table c1 as
       select
          %array(Cs,values=1-9)
          %do_over(cs,phrase=_C1?_,between=comma)
       from
          wrk.have
       ;
       create
          table c2 as
       select
          %do_over(cs,phrase=_C2?_,between=comma)
       from
          wrk.have
       ;
     quit;

    ');

