# utl-in-the-spirit-of-John-Tukey-a-useful-ascii-plot-of-percentages-proc-plot
In the spirit of John Tukey a useful ascii plot of percentages proc plot 
    %let pgm=utl-in-the-spirit-of-John-Tukey-a-useful-ascii-plot-of-percentages-proc-plot;

    In the spirit of John Tukey a useful ascii plot of percentages proc plot

    github                                                                                                         
    https://tinyurl.com/2marypxe                                                                                   
    https://github.com/rogerjdeangelis/utl-in-the-spirit-of-John-Tukey-a-useful-ascii-plot-of-percentages-proc-plot

    stackoverflow                                                                                                                
    https://tinyurl.com/58ckyntn                                                                                                 
    https://stackoverflow.com/questions/79237480/add-the-percentage-of-different-types-of-observations-in-a-geom-count-plot-in-gg

    If you have a good scanner and
    you ca even add pencilled annotation.

    SOAPBOX ON

      I wish sas would enhance ascii graphics
        1. add a second right axis
        2 box adding top and right axis labels
        3 support inset of text

      FYI: The 1980s DMS editor is a great ascii graphics editor

    SOAPBOX OFF

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                             |                                |                                                         */
    /*                             |                                |                                                         */
    /*           INPUT             |    PROCESS                     |                   OUTPUT                                */
    /*           =====             |    =======                     |                                                         */
    /*                             |                                |                                                         */
    /*                             |                                |                                                         */
    /*                             |                                |                                                         */
    /*                             |                                |                         VAR1                            */
    /*    Obs    VAR1 VAR          | DATA SORTED FOR                |         A                B                C             */
    /*                             | DOCUMENTATION ONLY             |      ---+----------------+----------------+---          */
    /*      1     C    A           |                                |  VAR2|                                       |VAR2      */
    /*      2     C    B           |                  CALCULATE     |      |  OVERALL PERCENTAGE BY VARxVAR2       |          */
    /*      3     C    C           | OBS VAR1 VAR2    PERCENTS      |      |                                       |          */
    /*      4     B    A           |                                |      |  COUNT(VAR1xVAR2)/OVERALL COUNT       |          */
    /*      5     C    C           |  1   A    A     1/12 = 8.3%    |      |                                       |          */
    /*      6     B    C           |                                |      |                                       |          */
    /*      7     B    A           |  2   B    A     3/12 = 25%     |      |  VAR1  VAR2                           |          */
    /*      8     B    A           |  3   B    A                    |    C +                    * 16.7%    16.7% * + C        */
    /*      9     C    A           |  4   B    A                    |      |   C     A                             |          */
    /*     10     A    A           |                                |      |   C     B                             |          */
    /*     11     B    C           |  5   B    B     1/12 = 8.3%    |      |   C     C                             |          */
    /*     12     B    B           |                                |      |   B     A                             |          */
    /*                             |  6   B    C                    |      |   C     C                             |          */
    /*                             |  7   B    C     2.12 = 16.7%   |      |   B     C                             |          */
    /*                             | ...                            |    B +   B     A          * 8.3%      8.3% * + B        */
    /*                             |                                |      |   B     A                             |          */
    /*                             | SAS                            |      |   C     A                             |          */
    /*                             | ===                            |      |   A     A                             |          */
    /*                             | select                         |      |   B     C                             |          */
    /*                             |   var1                         |      |   B     B                             |          */
    /*                             |  ,var2                         |      |                                       |          */
    /*                             |  ,cats(                        |    A +  * 8.3%            * 25.0%    16.7% * + A        */
    /*                             |     put(100*count(var1!!var2)  |      |                                       |          */
    /*                             |       /(select                 |      ---+----------------+----------------+---          */
    /*                             |             count(*)           |         A                B                C             */
    /*                             |         from sd1.have),4.1)    |                        VAR1                             */
    /*                             |         ,'%') as anno          |                                                         */
    /*                             | from                           |                                                         */
    /*                             |    sd1.have                    |                                                         */
    /*                             | group                          |                                                         */
    /*                             |    by var1, var2               |                                                         */
    /*                             |                                |                                                         */
    /*                             | options ls=64 ps=32;           |                                                         */
    /*                             | proc plot data=want            |                                                         */
    /*                             |  (rename=var2=var21234567890); |                                                         */
    /*                             |  plot var21234567890*var1='*'  |                                                         */
    /*                             |   $anno/box;                   |                                                         */
    /*                             | run;quit;                      |                                                         */
    /*                             |                                |                                                         */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options
      validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
    input Var1$ Var2$;
    cards4;
    C    A
    C    B
    C    C
    B    A
    C    C
    B    C
    B    A
    B    A
    C    A
    A    A
    B    C
    B    B
    ;;;;
    run;quit;

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    proc sql;
      create
         table want as
      select
         var1
        ,var2
        ,cats(
           put(100*count(var1!!var2)
             /(select
                   count(*)
               from sd1.have),4.1)
               ,'%') as anno
      from
         sd1.have
      group
         by var1, var2
    ;quit;

    /*---- lengthen variable name to better control horizonta width ---*/
    options ls=64 ps=32;
    proc plot data=want
     (rename=var2=var21234567890);
     plot var21234567890*var1='*'
      $anno/box;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*    ---+----------------+----------------+---                                                                           */
    /*    |                                       |                                                                           */
    /*    |                                       |                                                                           */
    /*  A +                   * 16.7%    16.7% *  +                                                                           */
    /*    |                                       |                                                                           */
    /*    |                                       |                                                                           */
    /*    |                                       |                                                                           */
    /*    |                                       |                                                                           */
    /*    |                                       |                                                                           */
    /*    |                                       |                                                                           */
    /*  B +                   * 8.3%      8.3% *  +                                                                           */
    /*    |                                       |                                                                           */
    /*    |                                       |                                                                           */
    /*    |                                       |                                                                           */
    /*    |                                       |                                                                           */
    /*    |                                       |                                                                           */
    /*    |                                       |                                                                           */
    /*  C +  * 8.3%           * 25.0%    16.7% *  +                                                                           */
    /*    |                                       |                                                                           */
    /*    ---+----------------+----------------+---                                                                           */
    /*       A                B                C                                                                              */
    /*                      VAR1                                                                                              */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
