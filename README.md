# utl_average_of_pairs_of_variables_with_minimum_differences_by_row
Average of pairs of variables with minimum differences by row.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Average of pairs of variables with minimum differences by row

    github
    https://tinyurl.com/y86cccst
    https://github.com/rogerjdeangelis/utl_average_of_pairs_of_variables_with_minimum_differences_by_row

    This is a bit tricky?
    I have my doubts that this will scale?

    A wallpaper solution
    I was unable to get some of th eposted solutions to work with more then three variables.

    see
    https://tinyurl.com/ycjxvpvl
    https://communities.sas.com/t5/Base-SAS-Programming/averaging-two-of-three-values/m-p/483643


    INPUT
    =====

      %let nvars=3;  ** thes defines the number od variables.
      We will get 3 choose 2 paries 3!/(2! * 1!) = 6/2 = 3

      WORK.HAV1ST total obs=3

       WEIGHT1    WEIGHT2    WEIGHT3

          7          3          1
          .          5          2
          9          5          4

      RULES  FIRST ROW
      ----------------
        Form the absolute value of the three possible differences.

           abs(WEIGHT1-WEIGHT2)  =  7 - 3 = 4
           abs(WEIGHT1-WEIGHT3)  =  7 - 1 = 6
           abs(WEIGHT2-WEIGHT3)  =  3 - 1 = 2

           compute the mean of the pair with minimum diference

           mean(WEIGHT2-WEIGHT3) = mean(3,1) = 2

      TRICKY PART
      -----------
           Compute the min of differences = 2
           Note the min function ignores missing values, which asures
           that no min can be missing, unless all weights are missing.

           Select the pair(s) of variables that yeild the minimum.
           If there are  multiple equal minimums use the last pair
           in the sequences of pairs defined by

           do i=1 to n-1; do j=i+1 to n

     EXAMPLE OUTPUT (USING OPS DATA)
     -------------------------------

      WANT total obs=4

       Obs     AVG

        1     53.00
        2     47.75
        3     68.75
        4     48.00


    PROCESS
    =======

     data want;

         * get all possible pairs;
         if _n_=0 then do;
           %let rc=%sysfunc(dosubl('
            proc sql;
              select l.t ,r.t into :is separated by " ",:js separated by " "
              from sashelp.yr111(obs=&nvars) as l, sashelp.yr111(obs=&nvars) as r
              where l.t < r.t
            ;quit;
            '))
            %array(da,values=&is);
            %array(db,values=&js);
         end;

         set have;
         %do_over(da db,phrase=%str(dif?da?db = abs(weight?da - weight?db);))
         %do_over(da db,phrase=%str(if DIF?da?db=min(of DIF:) then AVG=mean(WEIGHT?da,WEIGHT?db);))
         keep avg;

      run;quit;

     The code above generates this

     * my debug macro output;
     options mfile mprint source2;
     data want;
     set have;
     dif12 = abs(weight1 - weight2);
     dif13 = abs(weight1 - weight3);
     dif14 = abs(weight1 - weight4);
     dif15 = abs(weight1 - weight5);
     dif16 = abs(weight1 - weight6);
     dif17 = abs(weight1 - weight7);
     dif23 = abs(weight2 - weight3);
     dif24 = abs(weight2 - weight4);
     dif25 = abs(weight2 - weight5);
     dif26 = abs(weight2 - weight6);
     dif27 = abs(weight2 - weight7);
     dif34 = abs(weight3 - weight4);
     dif35 = abs(weight3 - weight5);
     dif36 = abs(weight3 - weight6);
     dif37 = abs(weight3 - weight7);
     dif45 = abs(weight4 - weight5);
     dif46 = abs(weight4 - weight6);
     dif47 = abs(weight4 - weight7);
     dif56 = abs(weight5 - weight6);
     dif57 = abs(weight5 - weight7);
     dif67 = abs(weight6 - weight7);
     if DIF12=min(of DIF:) then AVG=mean(WEIGHT1,WEIGHT2);
     if DIF13=min(of DIF:) then AVG=mean(WEIGHT1,WEIGHT3);
     if DIF14=min(of DIF:) then AVG=mean(WEIGHT1,WEIGHT4);
     if DIF15=min(of DIF:) then AVG=mean(WEIGHT1,WEIGHT5);
     if DIF16=min(of DIF:) then AVG=mean(WEIGHT1,WEIGHT6);
     if DIF17=min(of DIF:) then AVG=mean(WEIGHT1,WEIGHT7);
     if DIF23=min(of DIF:) then AVG=mean(WEIGHT2,WEIGHT3);
     if DIF24=min(of DIF:) then AVG=mean(WEIGHT2,WEIGHT4);
     if DIF25=min(of DIF:) then AVG=mean(WEIGHT2,WEIGHT5);
     if DIF26=min(of DIF:) then AVG=mean(WEIGHT2,WEIGHT6);
     if DIF27=min(of DIF:) then AVG=mean(WEIGHT2,WEIGHT7);
     if DIF34=min(of DIF:) then AVG=mean(WEIGHT3,WEIGHT4);
     if DIF35=min(of DIF:) then AVG=mean(WEIGHT3,WEIGHT5);
     if DIF36=min(of DIF:) then AVG=mean(WEIGHT3,WEIGHT6);
     if DIF37=min(of DIF:) then AVG=mean(WEIGHT3,WEIGHT7);
     if DIF45=min(of DIF:) then AVG=mean(WEIGHT4,WEIGHT5);
     if DIF46=min(of DIF:) then AVG=mean(WEIGHT4,WEIGHT6);
     if DIF47=min(of DIF:) then AVG=mean(WEIGHT4,WEIGHT7);
     if DIF56=min(of DIF:) then AVG=mean(WEIGHT5,WEIGHT6);
     if DIF57=min(of DIF:) then AVG=mean(WEIGHT5,WEIGHT7);
     if DIF67=min(of DIF:) then AVG=mean(WEIGHT6,WEIGHT7);
     keep avg;
     run;
     quit;


    OUTPUT
    ======

     %let nvars=3;

     Using first row of ops data

     WEIGHT1    WEIGHT2    WEIGHT3

      53.0       54.7        53

      WANT total obs=4

       Obs     AVG

        1     53.00  (53-53)=0 mean(53,53)=53
        2     47.75
        3     68.75
        4     48.00

     Using nvars=7

     WORK.WANT total obs=7

      Obs     AVG

       1      3.5  ** agrees with hand calculation
       2      9.5
       3      5.0
       4      9.0
       5     10.0
       6     12.5
       7     12.5

    Check row 1

    WEIGHT1    WEIGHT2    WEIGHT3    WEIGHT4    WEIGHT5    WEIGHT6    WEIGHT7

       19          .          7         22          3          4          .

       19-.  19
       19-7  12
       19-22 3
       19-3  16
       19-4  15
       19-.  19

       .-7    7
       .-22  22
       .-3    3
       .-4    4
       .-.    .

       7-22  15
       7-3    4
       7-4    3
       7-.    7

       22-3  19
       22-4  18
       22-.  22

       3-4    1   *** here is the min do mean(3,4) = 3.5
       3-.    3

       4-.    4

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    %let nvars=7;

    data hav1st;
      input obs weight1 weight2 weight3 ;
    cards4;
    1 53 54.7 53
    2 48 47.5  .
    3 67.4 68.5 69
    4 48 48 .
    ;;;;
    run;quit;

    data _null_ ;

      if &nvars ne 3 then do;
         rc=dosubl('
          data have;
             call streaminit (7) ;
             array w weight1-weight&nvars ;
             do _n_ = 1 to &nvars ;
               do over w ;
               w = int(24*rand ("uniform")) ;
               if rand ("uniform") < .3 then w = . ;
             end ;
             output ;
             end ;
          run;quit;
          ');
      end;
      else do;
          rc=dosubl('
           data have;
             set hav1st;
           run;quit;
         ');
      end;

    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    see process;

