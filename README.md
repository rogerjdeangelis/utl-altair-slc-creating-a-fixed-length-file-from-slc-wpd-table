# utl-altair-slc-creating-a-fixed-length-file-from-slc-wpd-table
Altair slc creating a fixed length file from slc wpd table
    %let pgm=utl-altair-slc-creating-a-fixed-length-file-from-slc-wpd-table;

    %stop_submission;

    RE: Altair slc creating a fixed length file from slc wpd table

    Too long to pst in listserv, see github

    github
    https://github.com/rogerjdeangelis/utl-altair-slc-creating-a-fixed-length-file-from-slc-wpd-table

    community.altair.com
    https://community.altair.com/discussion/19487?tab=accepted

    Macro flatfile in (and in this repo)
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories

    May not work to special formats like dates, commax.x dollarx.x, best ro stick to numeric and character.
    Excel tends to do conversions for you.

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /*********************************************************************************************/
    /*  CONVERT COMMA DELIMITED FILE TO FIXED FORMAT INPUT                                       */
    /*                                      |                                                    */
    /*               INPUT                  |                    OUTPUT                          */
    /*    -------------------------------   |    ---------------------------------------------   */
    /*    data class;                       |     data table  ;                                  */
    /*       infile cards4 delimiter=',';   |      input NAME   $char8. +1                       */
    /*       informat                       |            SEX    $char1. +1                       */
    /*         NAME $8.                     |            AGE    best10. +1                       */
    /*         SEX $1.                      |            HEIGHT best10. +1                       */
    /*         AGE 8.                       |            WEIGHT best10.;                         */
    /*         HEIGHT 8.                    |     cards4;                                        */
    /*         WEIGHT 8.                    |     Alfred   M         14         69      112.5    */
    /*    ;                                 |     Alice    F         13       56.5         84    */
    /*    input NAME SEX AGE HEIGHT WEIGHT; |     Barbara  F         13       65.3         98    */
    /*    cards4;                           |     Carol    F         14       62.8      102.5    */
    /*    Alfred,M,14,69,112.5              |     Henry    M         14       63.5      102.5    */
    /*    Alice,F,13,56.5,84                |     James    M         12       57.3         83    */
    /*    Barbara,F,13,65.3,98              |     Jane     F         12       59.8       84.5    */
    /*    Carol,F,14,62.8,102.5             |     Janet    F         15       62.5      112.5    */
    /*    Henry,M,14,63.5,102.5             |     Jeffrey  M         13       62.5         84    */
    /*    James,M,12,57.3,83                |     John     M         12         59       99.5    */
    /*    Jane,F,12,59.8,84.5               |     Joyce    F         11       51.3       50.5    */
    /*    Janet,F,15,62.5,112.5             |     Judy     F         14       64.3         90    */
    /*    Jeffrey,M,13,62.5,84              |     Louise   F         12       56.3         77    */
    /*    John,M,12,59,99.5,                |     Mary     F         15       66.5        112    */
    /*    Joyce,F,11,51.3,50.5              |     Philip   M         16         72        150    */
    /*    Judy,F,14,64.3,90,                |     Robert   M         12       64.8        128    */
    /*    Louise,F,12,56.3,77               |     Ronald   M         15         67        133    */
    /*    Mary,F,15,66.5,112                |     Thomas   M         11       57.5         85    */
    /*    Philip,M,16,72,150                |     William  M         15       66.5        112    */
    /*    Robert,M,12,64.8,128              |     ;;;;                                           */
    /*    Ronald,M,15,67,133                |     run;quit;                                      */
    /*    Thomas,M,11,57.5,85               |                                                    */
    /*    William,M,15,66.5,112             |                                                    */
    /*    ;;;;                              |                                                    */
    /*    run;quit;                         |                                                    */
    /*                                      |                                                    */
    /*********************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    data class;
       infile cards4 delimiter=',';
       informat
         NAME $8.
         SEX $1.
         AGE 8.
         HEIGHT 8.
         WEIGHT 8.
    ;
    input NAME SEX AGE HEIGHT WEIGHT;
    cards4;
    Alfred,M,14,69,112.5
    Alice,F,13,56.5,84
    Barbara,F,13,65.3,98
    Carol,F,14,62.8,102.5
    Henry,M,14,63.5,102.5
    James,M,12,57.3,83
    Jane,F,12,59.8,84.5
    Janet,F,15,62.5,112.5
    Jeffrey,M,13,62.5,84
    John,M,12,59,99.5,
    Joyce,F,11,51.3,50.5
    Judy,F,14,64.3,90,
    Louise,F,12,56.3,77
    Mary,F,15,66.5,112
    Philip,M,16,72,150
    Robert,M,12,64.8,128
    Ronald,M,15,67,133
    Thomas,M,11,57.5,85
    William,M,15,66.5,112
    ;;;;
    run;quit;

    /*----
    WORK.CLASS (CLASS.WPD)

     NAME       SEX    AGE    HEIGHT    WEIGHT

     Alfred      M      14     69.0      112.5
     Alice       F      13     56.5       84.0
     Barbara     F      13     65.3       98.0
     Carol       F      14     62.8      102.5
     Henry       M      14     63.5      102.5
     James       M      12     57.3       83.0
     Jane        F      12     59.8       84.5
     Janet       F      15     62.5      112.5
     Jeffrey     M      13     62.5       84.0
     John        M      12     59.0       99.5
     Joyce       F      11     51.3       50.5
     Judy        F      14     64.3       90.0
     Louise      F      12     56.3       77.0
     Mary        F      15     66.5      112.0
     Philip      M      16     72.0      150.0
     Robert      M      12     64.8      128.0
     Ronald      M      15     67.0      133.0
     Thomas      M      11     57.5       85.0
     William     M      15     66.5      112.0
    ----*/

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    %utl_flatfile(
          lib=work
         ,sasdsn=class
         ,file=d:/sas/inpfix.sas
         );

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    data table  ;
     input NAME   $char8. +1
           SEX    $char1. +1
           AGE    best10. +1
           HEIGHT best10. +1
           WEIGHT best10.;
    cards4;
    Alfred   M         14         69      112.5
    Alice    F         13       56.5         84
    Barbara  F         13       65.3         98
    Carol    F         14       62.8      102.5
    Henry    M         14       63.5      102.5
    James    M         12       57.3         83
    Jane     F         12       59.8       84.5
    Janet    F         15       62.5      112.5
    Jeffrey  M         13       62.5         84
    John     M         12         59       99.5
    Joyce    F         11       51.3       50.5
    Judy     F         14       64.3         90
    Louise   F         12       56.3         77
    Mary     F         15       66.5        112
    Philip   M         16         72        150
    Robert   M         12       64.8        128
    Ronald   M         15         67        133
    Thomas   M         11       57.5         85
    William  M         15       66.5        112
    ;;;;
    run;quit;

    RUNNING THE FIXED LENGTH CODE

     TABLE total obs=19 10NOV2025:11:09:42

      NAME       SEX    AGE    HEIGHT    WEIGHT

      Alfred      M      14     69.0      112.5
      Alice       F      13     56.5       84.0
      Barbara     F      13     65.3       98.0
      Carol       F      14     62.8      102.5
      Henry       M      14     63.5      102.5
      James       M      12     57.3       83.0
      Jane        F      12     59.8       84.5
      Janet       F      15     62.5      112.5
      Jeffrey     M      13     62.5       84.0
      John        M      12     59.0       99.5
      Joyce       F      11     51.3       50.5
      Judy        F      14     64.3       90.0
      Louise      F      12     56.3       77.0
      Mary        F      15     66.5      112.0
      Philip      M      16     72.0      150.0
      Robert      M      12     64.8      128.0
      Ronald      M      15     67.0      133.0
      Thomas      M      11     57.5       85.0
      William     M      15     66.5      112.0

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
