# utl_generate_all_combinations_of_gene_mutations_within_and_organism
Generate all combinations of gene mutations within and organism. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Generate all combinations of gene mutations within and organism

    github
    https://goo.gl/97p87V
    https://github.com/rogerjdeangelis/utl_generate_all_combinations_of_gene_mutations_within_and_organism

    Same results in WPS and SAS

    I could not understand or get the R program to work.

    https://stackoverflow.com/questions/48709076/r-combining-values-from-columns-into-rows

    INPUT
    =====

     ALGORITHM

        1.For OG1 the op asks us to examine all combinations of
            PB with PF and PI = 2 *3 * 3 or 18 combinations
        2. For OG2 we have PBA,PFA and PLA for 3*2*2 =12 combinations

      WORK.HAVE total obs=15

      GROUP    GENE_ID   |  RULES ( two of the 18 possible combintions)
                         |
       OG1      PB_1     |  GNE1ST    GNE2ND    GNE3RD
       OG1      PB_2     |   PB_1      PF_1      PI_1
                         |   PB_2      PF1       PI_1
       OG1      PF_1     |
       OG1      PF_2     |
       OG1      PF_3     |
                         |
       OG1      PI_1     |
       OG1      PI_2     |
       OG1      PI_3     |
                         |
       OG2      PBa_1    |
       OG2      PBa_2    |
       OG2      PBa_3    |
       OG2      PFa_1    |
       OG2      PFa_2    |
       OG2      PIa_1    |
       OG2      PIa_2    |

      EXAMPLE FOR OG1
      ===============

       GROUP    GNE1ST    GNE2ND    GNE3RD

        OG1      PB_1      PF_1      PI_1
        OG1      PB_1      PF_1      PI_2
        OG1      PB_1      PF_1      PI_3

        OG1      PB_1      PF_2      PI_1
        OG1      PB_1      PF_2      PI_2
        OG1      PB_1      PF_2      PI_3

        OG1      PB_1      PF_3      PI_1
        OG1      PB_1      PF_3      PI_2
        OG1      PB_1      PF_3      PI_3

        OG1      PB_2      PF_1      PI_1
        OG1      PB_2      PF_1      PI_2
        OG1      PB_2      PF_1      PI_3

        OG1      PB_2      PF_2      PI_1
        OG1      PB_2      PF_2      PI_2
        OG1      PB_2      PF_2      PI_3

        OG1      PB_2      PF_3      PI_1
        OG1      PB_2      PF_3      PI_2



    PROCESS (All the code)
    ======================

       * make it a view ;
       * Simple rollup of counts for each of the gene types;
       data havRol/view=havRol;
         length gene_id $3 cnts $24 genes $480;
         retain cnt 0 cnts genes;
         set have;
         by group gene_id notsorted;
         cnt=cnt+1;
         if last.gene_id then do;
            cnts=catx("@",cnts,put(cnt,2.));
            genes=catx("@",genes,gene_id);
            cnt=0;
         end;
         if last.group then do;
            output;
            cnts="";
            genes="";
         end;
         keep group genes cnts;
       run;quit;

       /*  Simple rollup of counts for each of the gene types

          Obs    GROUP    CNTS        GENES

            1     OG1     2@3@3    PB_@PF_@PI_
            2     OG2     3@2@2    PBa@PFa@PIa
       */

       * unroll;
       * just use the counts for three loops to form all combinations;
       data havVrt;

         set havRol;
           do i=1 to input(scan(cnts,1,"@"),2.);
             gne1st=cats(scan(genes,1,"@"),put(i,1.));
             do j=1 to input(scan(cnts,2,"@"),2.);
               gne2nd=cats(scan(genes,2,"@"),put(j,1.));
               do k=1 to input(scan(cnts,3,"@"),2.);
                 gne3rd=cats(scan(genes,3,"@"),put(k,1.));
                 output;
               end;
             end;
           end;
           keep group gne:
       ;
       run;quit;

    OUTPUT
    ======

       /*

        WORK.HAVVRT total obs=30

        GROUP    GNE1ST    GNE2ND    GNE3RD

         OG1      PB_1      PF_1      PI_1
         OG1      PB_1      PF_1      PI_2
         OG1      PB_1      PF_1      PI_3
         OG1      PB_1      PF_2      PI_1
       ....

       */

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;


    data have;
     retain group;
     input gene_ID$ group$;
    cards4;
     PB_1 OG1
     PB_2 OG1
     PF_1 OG1
     PF_2 OG1
     PF_3 OG1
     PI_1 OG1
     PI_2 OG1
     PI_3 OG1
     PBa_1 OG2
     PBa_2 OG2
     PBa_3 OG2
     PFa_1 OG2
     PFa_2 OG2
     PIa_1 OG2
     PIa_2 OG2
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    *SAS;
    * make it a view ;
    data havRol/view=havRol;
      length gene_id $3 cnts $24 genes $480;
      retain cnt 0 cnts genes;
      set have;
      by group gene_id notsorted;
      cnt=cnt+1;
      if last.gene_id then do;
         cnts=catx("@",cnts,put(cnt,2.));
         genes=catx("@",genes,gene_id);
         cnt=0;
      end;
      if last.group then do;
         output;
         cnts="";
         genes="";
      end;
      keep group genes cnts;
    run;quit;

    /*  Simple rollup of counts for each of the gene types

       Obs    GROUP    CNTS        GENES

         1     OG1     2@3@3    PB_@PF_@PI_
         2     OG2     3@2@2    PBa@PFa@PIa
    */

    * just use the counts for three loops to form all combinations;
    data havVrt;

      set havRol;
        do i=1 to input(scan(cnts,1,"@"),2.);
          gne1st=cats(scan(genes,1,"@"),put(i,1.));
          do j=1 to input(scan(cnts,2,"@"),2.);
            gne2nd=cats(scan(genes,2,"@"),put(j,1.));
            do k=1 to input(scan(cnts,3,"@"),2.);
              gne3rd=cats(scan(genes,3,"@"),put(k,1.));
              output;
            end;
          end;
        end;
        keep group gne:
    ;
    run;quit;

    /*
     WORK.HAVVRT  total obs=30

       GROUP    GNE1ST    GNE2ND    GNE3RD

        OG1      PB_1      PF_1      PI_1
        OG1      PB_1      PF_1      PI_2
        OG1      PB_1      PF_1      PI_3
        OG1      PB_1      PF_2      PI_1
        OG1      PB_1      PF_2      PI_2
        OG1      PB_1      PF_2      PI_3
        OG1      PB_1      PF_3      PI_1
        OG1      PB_1      PF_3      PI_2
        OG1      PB_1      PF_3      PI_3
        OG1      PB_2      PF_1      PI_1
        OG1      PB_2      PF_1      PI_2
        OG1      PB_2      PF_1      PI_3
        OG1      PB_2      PF_2      PI_1
        OG1      PB_2      PF_2      PI_2
        OG1      PB_2      PF_2      PI_3
        OG1      PB_2      PF_3      PI_1
        OG1      PB_2      PF_3      PI_2
        OG1      PB_2      PF_3      PI_3

        OG2      PBa1      PFa1      PIa1
        OG2      PBa1      PFa1      PIa2
        OG2      PBa1      PFa2      PIa1
        OG2      PBa1      PFa2      PIa2
        OG2      PBa2      PFa1      PIa1
        OG2      PBa2      PFa1      PIa2
        OG2      PBa2      PFa2      PIa1
        OG2      PBa2      PFa2      PIa2
        OG2      PBa3      PFa1      PIa1
        OG2      PBa3      PFa1      PIa2
        OG2      PBa3      PFa2      PIa1
        OG2      PBa3      PFa2      PIa2
     */


    * WPS;

    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";

    data havRol/view=havRol;
      length gene_id $3 cnts $24 genes $480;
      retain cnt 0 cnts genes;
      set wrk.have;
      by group gene_id notsorted;
      cnt=cnt+1;
      if last.gene_id then do;
         cnts=catx("@",cnts,put(cnt,2.));
         genes=catx("@",genes,gene_id);
         cnt=0;
      end;
      if last.group then do;
         output;
         cnts="";
         genes="";
      end;
      keep group genes cnts;
    run;quit;

    * just use the counts for three loops to form all combinations;
    data wrk.havVrtWps;

      set havRol;
        do i=1 to input(scan(cnts,1,"@"),2.);
          gne1st=cats(scan(genes,1,"@"),put(i,1.));
          do j=1 to input(scan(cnts,2,"@"),2.);
            gne2nd=cats(scan(genes,2,"@"),put(j,1.));
            do k=1 to input(scan(cnts,3,"@"),2.);
              gne3rd=cats(scan(genes,3,"@"),put(k,1.));
              output;
            end;
          end;
        end;
        keep group gne:
    ;
    run;quit;
    ');

