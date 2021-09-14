# utl-creating-tables-and-macro-variable-lists-with-variable-names-from-any-table
Creating tables and macro variable lists with variable names from any table     
    %let pgm=utl-creating-tables-and-macro-variable-lists-with-variable-names-from-any-table;

    Creating tables and macro variable lists with variable names from any table

    GitHub
    https://tinyurl.com/mnnc44sw
    https://github.com/rogerjdeangelis/utl-creating-tables-and-macro-variable-lists-with-variable-names-from-any-table

          Table with variable names

            1. proc transpose
            2. proc contents
            3. proc datasets
            4. SQL dictionaries
            5. utl_gather

          Macro list of variable names

            6, macro list of variables
            7, macro array of variable names var1-var5 and varn=5

          Useful ways to create table like another table

            8. If _n_=0
            9. SQL like
           10. SQL describe

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    proc datasets lib=work kill;
    run;quit;

    data class;
      set sashelp.class;
    run;quit;

     _     _
    / |   | |_ _ __ __ _ _ __  ___ _ __   ___  ___  ___
    | |   | __| `__/ _` | `_ \/ __| `_ \ / _ \/ __|/ _ \
    | |_  | |_| | | (_| | | | \__ \ |_) | (_) \__ \  __/
    |_(_)  \__|_|  \__,_|_| |_|___/ .__/ \___/|___/\___|
                                  |_|

    proc transpose data=class(obs=1) out=xpoTranspose(drop=col1);
      var _all_;
    run;quit;

    Up to 40 obs WORK.XPOTRANSPOSE total obs=5

    Obs    _NAME_

     1     NAME
     2     SEX
     3     AGE
     4     HEIGHT
     5     WEIGHT

    /*___                      _             _
    |___ \      ___ ___  _ __ | |_ ___ _ __ | |_ ___
      __) |    / __/ _ \| `_ \| __/ _ \ `_ \| __/ __|
     / __/ _  | (_| (_) | | | | ||  __/ | | | |_\__ \
    |_____(_)  \___\___/|_| |_|\__\___|_| |_|\__|___/

    */

    proc contents data=class out=xpocon (keep=name);
    run;quit;

    Up to 40 obs from XPOCON total obs=5

    Obs    NAME

     1     AGE
     2     HEIGHT
     3     NAME
     4     SEX
     5     WEIGHT

    /*____        _       _                 _
    |___ /     __| | __ _| |_ __ _ ___  ___| |_ ___
      |_ \    / _` |/ _` | __/ _` / __|/ _ \ __/ __|
     ___) |  | (_| | (_| | || (_| \__ \  __/ |_\__ \
    |____(_)  \__,_|\__,_|\__\__,_|___/\___|\__|___/

    */
    proc datasets lib=work nolist nodetails;
       contents data=class out=xpoDatasets(keep=name);
    run;quit;

    Up to 40 obs from XPODATASETS total obs=5

    Obs    NAME

     1     AGE
     2     HEIGHT
     3     NAME
     4     SEX
     5     WEIGHT

    /*  _                _       _ _      _
    | || |     ___  __ _| |   __| (_) ___| |_ ___
    | || |_   / __|/ _` | |  / _` | |/ __| __/ __|
    |__   _|  \__ \ (_| | | | (_| | | (__| |_\__ \
       |_|(_) |___/\__, |_|  \__,_|_|\___|\__|___/
                      |_|
    */


    proc sql;
      create
         table sqlDic as
      select
         name
      from
         sashelp.vcolumn
      where
         libname="WORK"
         and memname="CLASS"
    ;quit;

    Up to 40 obs WORK.SQLDIC total obs=5

    Obs    NAME

     1     NAME
     2     SEX
     3     AGE
     4     HEIGHT
     5     WEIGHT

    /*___                 _   _
    | ___|     __ _  __ _| |_| |__   ___ _ __
    |___ \    / _` |/ _` | __| `_ \ / _ \ `__|
     ___) |  | (_| | (_| | |_| | | |  __/ |
    |____(_)  \__, |\__,_|\__|_| |_|\___|_|
              |___/
    */

    %utl_gather(sashelp.class(obs=1), var, Val, Exclude, xpoAlea, ValFormat=$5., WithFormats=Y);

    Up to 40 obs from XPOALEA total obs=5

    Obs    VAR       VAL      _COLFORMAT    _COLTYP

     1     NAME      Alfre     $8.             C
     2     SEX       M         $1.             C
     3     AGE       14        BEST12.         N
     4     HEIGHT    69        BEST12.         N
     5     WEIGHT    112.5     BEST12.         N

    /*__                      _ _     _
     / /_    __   ____ _ _ __| (_)___| |_
    | `_ \   \ \ / / _` | `__| | / __| __|
    | (_) |   \ V / (_| | |  | | \__ \ |_
     \___(_)   \_/ \__,_|_|  |_|_|___/\__|

    */

    %let varlst=%varlist(class);

    %put &=varlst;


    VARLST=NAME SEX AGE HEIGHT WEIGHT

    /*____
    |___  |  _ __ ___   __ _  ___ _ __ ___     __ _ _ __ _ __ __ _ _   _
       / /  | `_ ` _ \ / _` |/ __| `__/ _ \   / _` | `__| `__/ _` | | | |
      / /_  | | | | | | (_| | (__| | | (_) | | (_| | |  | | | (_| | |_| |
     /_/(_) |_| |_| |_|\__,_|\___|_|  \___/   \__,_|_|  |_|  \__,_|\__, |
                                                                   |___/
    */

    %array(vars,values=%varlist(class));

    data varAry;
      %do_over(vars,phrase=%str(name="?     ";output;));
    run;quit;

    Up to 40 obs WORK.VARARY total obs=5

    Obs    NAME

     1     NAME
     2     SEX
     3     AGE
     4     HEIGHT
     5     WEIGHT


    /*___      _  __                       ___
     ( _ )    (_)/ _|       _ __    _____ / _ \
     / _ \    | | |_       | `_ \  |_____| | | |
    | (_) |   | |  _|      | | | | |_____| |_| |
     \___(_)  |_|_|    ____|_| |_|____    \___/
                      |_____|   |_____|
    */

    data ifn;
      if _n_=0 then set class;
    run;quit;

    Up to 40 obs from IFN total obs=1

    Obs    NAME    SEX    AGE    HEIGHT    WEIGHT

     1                     .        .         .

     Variables in Creation Order

    #    Variable    Type    Len

    1    NAME        Char      8
    2    SEX         Char      1
    3    AGE         Num       8
    4    HEIGHT      Num       8
    5    WEIGHT      Num       8

    /*___               _   _ _ _
     / _ \    ___  __ _| | | (_) | _____
    | (_) |  / __|/ _` | | | | | |/ / _ \
     \__, |  \__ \ (_| | | | | |   <  __/
       /_(_) |___/\__, |_| |_|_|_|\_\___|
                     |_|
    */

    proc sql;
      create
        table clone like class
    ;quit;

     Variables in Creation Order

    #    Variable    Type    Len

    1    NAME        Char      8
    2    SEX         Char      1
    3    AGE         Num       8
    4    HEIGHT      Num       8
    5    WEIGHT      Num       8

    /*  ___               _       _                     _ _
    / |/ _ \    ___  __ _| |   __| | ___  ___  ___ _ __(_) |__   ___
    | | | | |  / __|/ _` | |  / _` |/ _ \/ __|/ __| `__| | `_ \ / _ \
    | | |_| |  \__ \ (_| | | | (_| |  __/\__ \ (__| |  | | |_) |  __/
    |_|\___(_) |___/\__, |_|  \__,_|\___||___/\___|_|  |_|_.__/ \___|
                       |_|
    */

    proc sql;
      describe table class;
    run;quit;

    create table WORK.CLASS( bufsize=65536 )
      (
       NAME char(8),
       SEX char(1),
       AGE num,
       HEIGHT num,
       WEIGHT num
      );

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
