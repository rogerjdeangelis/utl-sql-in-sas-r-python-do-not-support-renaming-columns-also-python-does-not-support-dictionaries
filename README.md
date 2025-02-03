# utl-sql-in-sas-r-python-do-not-support-renaming-columns-also-python-does-not-support-dictionaries
SQL in sas r python do not support renaming columns also python does not support dictionaries 
    %let pgm=utl-sql-in-sas-r-python-do-not-support-renaming-columns-also-python-does-not-support-dictionaries;

    %stop_submission;

    SQL in sas r python do not support renaming columns also python does not support dictionaries

    SOAPBOX ON

    Note: I am interested in remaing columns in r sqldf and python
    pdsql without creating a table or adding and removing columns.

    It is more important to know what a language does not support then what the language does support.

    SAS does NOT support renaming columns using sql using alter table, however sas does
    support alter table for most other modifications

    This may be do triggers, constraints and idexes.
    Note sas does support, adding columns, dropping columns, modifying column attributes, adding and removing constraint.

    R sqldf, but not python pdsql, can read limited column meta data but r and python cannot change meta data.

    SOAPBOX OFF


    stackoverflow
    https://tinyurl.com/yc7nv3wx
    https://stackoverflow.com/questions/79404352/rename-multiple-variables-at-once-using-dplyr


       SOLUTIONS (use base language functions and code generation usingsqlarrays for mutiple renames))

        RENAMING FUNCTIONS
          1 proc datasets
          2 r rename base
          3 python rename pandas language

        COLUMN DICTIONARY
          4 sas column dictionary
          5 r sqldf dictionary
          6 python fails


    related repos

    here is a in memory sq;ite3 solution
    https://github.com/rogerjdeangelis/utl-sql-column-dictionary-in-sas-r-and-python-multi-language

    https://github.com/rogerjdeangelis/utl-wps-meta-data-dictionaries
    https://github.com/rogerjdeangelis/utl-create-a-sas-table-with-meta-data-and-no-observations
    https://github.com/rogerjdeangelis/utl-dosubl-using-meta-data-with-column-names-and-labels-to-create-mutiple-proc-reports
    https://github.com/rogerjdeangelis/utl-extracting-sas-meta-data-using-sas-macro-fcmp-and-dosubl
    https://github.com/rogerjdeangelis/utl-join-two-ables-based-on-predefined-meta-data-rules
    https://github.com/rogerjdeangelis/utl-load-and-extract-ms-excel-document-properties-metadata
    https://github.com/rogerjdeangelis/utl-macros-to-get-all-variable-and-dataset-meta-data-dictionary-tables
    https://github.com/rogerjdeangelis/utl-meta-analysis-of-a-single-proportion-in-R-with-forest-and-funnel-plots
    https://github.com/rogerjdeangelis/utl-perl-write-and-read-meta-data-saved-in-the-windows-file-properties-details-panel
    https://github.com/rogerjdeangelis/utl-wps-meta-data-dictionaries
    https://github.com/rogerjdeangelis/utl_meta_analysis_funnel_plot_odds_ratio_vs_standard_error
    https://github.com/rogerjdeangelis/utl_using_meta_data_add_column_average_height_to_each_table_in_a_sas_data_library
    https://github.com/rogerjdeangelis/utl-macros-to-get-all-variable-and-dataset-meta-data-dictionary-tables


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* RENAMING COLUMNS                                                                                                       */
    /*------------------------------------------------------------------------------------------------------------------------*/
    /*                         |                                             |                                                */
    /*         SAS             |                 R                           |              Python                            */
    /*                         |                                             |                                                */
    /*  proc datasets lib=lib; | Base R language                             | pandas language                                */
    /*    modify dataset;      | colnames(have)<-c("X1","X2");               | df=df.rename(columns={'xa': 'x1', 'xb': 'x2'}) */
    /*    rename               |                                             |                                                */
    /*        xa= x1           | Also dynamic dplyr language                 |                                                */
    /*        xb =x2           |                                             |                                                */
    /*    ..                   | dplyr language (rename with)                |                                                */
    /*  run;quit;              | df |> rename_with(~paste0("x", 1:ncol(df))) |                                                */
    /*                         |                                             |                                                */
    /*                         |                                             |                                                */
    /*------------------------------------------------------------------------------------------------------------------------*/
    /*                                                                                                                        */
    /*ACCESS COLUMN DICTIONARY                                                                                                */
    /*------------------------------------------------------------------------------------------------------------------------*/
    /*                         |                                             |                                                */
    /* SAS                     |  R                                          |  PYTHON                                        */
    /*                         |                                             |                                                */
    /* select                  |  select                                     |  Unfortumately PDSQL does not support          */
    /*   libname               |     name                                    |  the column dictionary                         */
    /*  ,memname               |    ,type                                    |                                                */
    /*  ,name                  |    ,pk                                      |  pragma_table_info                             */
    /*  ,type                  |  from                                       |                                                */
    /* from                    |    pragma_table_info('have')                |  ERROR                                         */
    /*   dictionary.columns    |                                             |                                                */
    /* where                   |                                             |  Empty DataFrame                               */
    /*   libname="SD1" and     |                                             |  Columns: [name, type, pk]                     */
    /*   memname="HAVE"        |                                             |  Index: []                                     */
    /*                         |                                             |                                                */
    /* OUTPUT                  | OUTPUT                                      |  See this for an in memory sqlite3 solution    */
    /* ======                  | ======                                      |  https://tinyurl.com/yeymr2x2                  */
    /*                    I    |                                             |                                                */
    /*  L   M             D    |                                             |                                                */
    /*  I   E             X    |                                             |                                                */
    /*  B   M             U    |                                             |                                                */
    /*  N   N    N   T    S    |   N      T                                  |                                                */
    /*  A   A    A   Y    A    |   A      Y                                  |                                                */
    /*  M   M    M   P    G    |   M      P      P                           |                                                */
    /*  E   E    E   E    E    |   E      E      K                           |                                                */
    /*                         |                                             |                                                */
    /* SD1 HAVE  XA char       |   XA    TEXT    0                           |                                                */
    /* SD1 HAVE  XB char       |   XB    TEXT    0                           |                                                */
    /*                         |                                             |                                                */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
     xa="A";
     xb="B";
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  XA    XB                                                                                                              */
    /*                                                                                                                        */
    /*  A     B                                                                                                               */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                              _       _                 _
    / |  _ __  _ __ ___   ___    __| | __ _| |_ __ _ ___  ___| |_ ___
    | | | `_ \| `__/ _ \ / __|  / _` |/ _` | __/ _` / __|/ _ \ __/ __|
    | | | |_) | | | (_) | (__  | (_| | (_| | || (_| \__ \  __/ |_\__ \
    |_| | .__/|_|  \___/ \___|  \__,_|\__,_|\__\__,_|___/\___|\__|___/
        |_|   _ _   _                 _
    __      _(_) |_| |__    ___  __ _| |   __ _ _ __ _ __ __ _ _   _ ___
    \ \ /\ / / | __| `_ \  / __|/ _` | |  / _` | `__| `__/ _` | | | / __|
     \ V  V /| | |_| | | | \__ \ (_| | | | (_| | |  | | | (_| | |_| \__ \
      \_/\_/ |_|\__|_| |_| |___/\__, |_|  \__,_|_|  |_|  \__,_|\__, |___/
                                   |_|                         |___/
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
     xa="A";
     xb="B";
    run;quit;

    %array(_col,values=%utl_varlist(sd1.have));
    %array(_seq,values=1-&_coln);

    data _null_;
      %do_over(_col _seq,phrase=%str(put "?_col = X?_seq";))
    run;quit;

    /*---- cut and paste from log -----*/
    options validvarname=upcase;

    proc datasets lib=sd1 nodetails nolist;
      modify have;
      rename  %do_over(_col _seq,phrase=%str(?_col = X?_seq))
    ;run;quit;

    proc print data=sd1.have;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*   X1    X2                                                                                                             */
    /*                                                                                                                        */
    /*   A     B                                                                                                              */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*         __
    __      __/ /__     __ _ _ __ _ __ __ _ _   _ ___
    \ \ /\ / / / _ \   / _` | `__| `__/ _` | | | / __|
     \ V  V / / (_) | | (_| | |  | | | (_| | |_| \__ \
      \_/\_/_/ \___/   \__,_|_|  |_|  \__,_|\__, |___/
                                            |___/
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
     xa="A";
     xb="B";
    run;quit;

    %array(_col,values=%utl_varlist(sd1.have));
    %array(_seq,values=1-&_coln);

    data _null_;
      %do_over(_col _seq,phrase=%str(put "?_col = X?_seq";))
    run;quit;

    /*---- cut and paste from log -----*/

    /*---- cut and paste from log -----*/
    proc datasets lib=sd1 nodetails nolist;
      modify have;
      rename
        XA = X1
        XB = X2 ;
    ;run;quit;

    proc print data=sd1.have;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*   X1    X2                                                                                                             */
    /*                                                                                                                        */
    /*   A     B                                                                                                              */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___           _
    |___ \   _ __  | |__   __ _ ___  ___
      __) | | `__| | `_ \ / _` / __|/ _ \
     / __/  | |    | |_) | (_| \__ \  __/
    |_____| |_|    |_.__/ \__,_|___/\___|

    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
     xa="A";
     xb="B";
    run;quit;

    %array(_col,values=%utl_varlist(sd1.have));
    %array(_seq,values=1-&_coln);

    data _null_;
      %do_over(_col _seq,phrase=%str(put "'X?_seq',";));
    run;quit;

    /*---- cut and paste into the r script ----*/

    proc datasets lib=sd1 nolist nodetails;
     delete want;
    run;quit;

    %utl_rbeginx;
    parmcards4;
    library(haven)
    source("c:/oto/fn_tosas9x.R")
    have<-read_sas("d:/sd1/have.sas7bdat")
    print(have)
    colnames(have)<-c('X1','X2')
    have;
    fn_tosas9x(
          inp    = have
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want heading=vertical;
    run;quit;

    /**************************************************************************************************************************/
    /*           |                                                                                                            */
    /* R         |  SAS                                                                                                       */
    /*           |                                                                                                            */
    /*    X1 X2  |  ROWNAMES    X1    X2                                                                                      */
    /*           |                                                                                                            */
    /*  1  A  B  |      1       A     B                                                                                       */
    /*           |                                                                                                            */
    /**************************************************************************************************************************/

    /*____               _   _                                         _            _
    |___ /   _ __  _   _| |_| |__   ___  _ __    _ __   __ _ _ __   __| | __ _ ___ | | __ _ _ __   __ _ _   _  __ _  __ _  ___
      |_ \  | `_ \| | | | __| `_ \ / _ \| `_ \  | `_ \ / _` | `_ \ / _` |/ _` / __|| |/ _` | `_ \ / _` | | | |/ _` |/ _` |/ _ \
     ___) | | |_) | |_| | |_| | | | (_) | | | | | |_) | (_| | | | | (_| | (_| \__ \| | (_| | | | | (_| | |_| | (_| | (_| |  __/
    |____/  | .__/ \__, |\__|_| |_|\___/|_| |_| | .__/ \__,_|_| |_|\__,_|\__,_|___/|_|\__,_|_| |_|\__, |\__,_|\__,_|\__, |\___|
            |_|    |___/                        |_|                                               |___/             |___/

    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
     xa="A";
     xb="B";
    run;quit;

    %array(_col,values=%utl_varlist(sd1.have));
    %array(_seq,values=1-&_coln);

    data _null_;
      %do_over(_col _seq,phrase=%str(put "'?_col': 'x?_seq'";))
    run;quit;

    /*---- cut and paste from log -----*/
    options validvarname=upcase;

    proc datasets lib=sd1 nolist nodetails;
     delete pywant;
    run;quit;

    %utl_pybeginx;
    parmcards4;
    import pandas as pd
    exec(open('c:/oto/fn_python.py').read());
    have,meta = ps.read_sas7bdat('d:/sd1/have.sas7bdat');
    have=have.rename(columns={'XA': 'X1', 'XB': 'X2'})
    print(have);
    fn_tosas9x(have,outlib='d:/sd1/',outdsn='pywant',timeest=3);
    ;;;;
    %utl_pyendx;

    proc print data=sd1.pywant;
    run;quit;

    /**************************************************************************************************************************/
    /*           |                                                                                                            */
    /* PYTHON    |  SAS                                                                                                       */
    /*           |                                                                                                            */
    /*    X1 X2  |  X1    X2                                                                                                  */
    /*           |                                                                                                            */
    /*  0  A  B  |  A     B                                                                                                   */
    /*           |                                                                                                            */
    /**************************************************************************************************************************/

    /*  _                         _ _      _   _
    | || |    ___  __ _ ___    __| (_) ___| |_(_) ___  _ __   __ _ _ __ _   _
    | || |_  / __|/ _` / __|  / _` | |/ __| __| |/ _ \| `_ \ / _` | `__| | | |
    |__   _| \__ \ (_| \__ \ | (_| | | (__| |_| | (_) | | | | (_| | |  | |_| |
       |_|   |___/\__,_|___/  \__,_|_|\___|\__|_|\___/|_| |_|\__,_|_|   \__, |
                                                                        |___/
    */

    proc sql;
      create
        table cols as
      select
        libname
       ,memname
       ,name
       ,type
       ,idxusage
      from
        dictionary.columns
      where
        libname="SD1" and
        memname="HAVE"
    ;quit;

    proc print data=cols /*heading=vertical*/;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  LIBNAME    MEMNAME    NAME    TYPE    IDXUSAGE                                                                        */
    /*                                                                                                                        */
    /*    SD1       HAVE       XA     char                                                                                    */
    /*    SD1       HAVE       XB     char                                                                                    */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                   _     _  __      _ _      _   _
    | ___|  _ __  ___  __ _| | __| |/ _|  __| (_) ___| |_(_) ___  _ __   __ _ _ __ _   _
    |___ \ | `__|/ __|/ _` | |/ _` | |_  / _` | |/ __| __| |/ _ \| `_ \ / _` | `__| | | |
     ___) || |   \__ \ (_| | | (_| |  _|| (_| | | (__| |_| | (_) | | | | (_| | |  | |_| |
    |____/ |_|   |___/\__, |_|\__,_|_|   \__,_|_|\___|\__|_|\___/|_| |_|\__,_|_|   \__, |
                         |_|                                                       |___/
    */

    proc datasets lib=sd1 nolist nodetails;
     delete want;
    run;quit;

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    source("c:/oto/fn_tosas9x.R")
    have<-read_sas("d:/sd1/have.sas7bdat")
    print(have)
    want<-sqldf("
       select
          name
         ,type
         ,pk
       from
         pragma_table_info('have')
       ")
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want;
    run;quit;

    /*__                 _   _                    __       _ _
     / /_    _ __  _   _| |_| |__   ___  _ __    / _| __ _(_) |___
    | `_ \  | `_ \| | | | __| `_ \ / _ \| `_ \  | |_ / _` | | / __|
    | (_) | | |_) | |_| | |_| | | | (_) | | | | |  _| (_| | | \__ \
     \___/  | .__/ \__, |\__|_| |_|\___/|_| |_| |_|  \__,_|_|_|___/
            |_|    |___/
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
     xa="A";
     xb="B";
    run;quit;

    proc datasets lib=sd1 nolist nodetails;
     delete pywant;
    run;quit;

    %utl_pybeginx;
    parmcards4;
    exec(open('c:/oto/fn_python.py').read());
    have,meta = ps.read_sas7bdat('d:/sd1/have.sas7bdat');
    print(have);
    want=pdsql('''select name,type,pk from pragma_table_info("have")''')
    print(want)
    ;;;;
    %utl_pyendx;

    proc print data=sd1.pywant;
    run;quit;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
