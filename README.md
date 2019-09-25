# utl-rename-all-variables-into-subscripted-variable-like-y1-y10
Dosubl rename all variables into subscripted variable like-y1-y10 

    Rename all variables into subscripted variables like-y1-y10                                                                           
                                                                                                                                          
    https://tinyurl.com/yxnhfd9o                                                                                                          
    https://github.com/rogerjdeangelis/utl-rename-all-variables-into-subscripted-variable-like-y1-y10                                     
                                                                                                                                          
    sas forum                                                                                                                             
    https://tinyurl.com/y37gvdr4                                                                                                          
    https://communities.sas.com/t5/New-SAS-User/Dynamically-rename-a-bunch-of-vriables-into-new-variables-Y1-Y2/m-p/591328                
                                                                                                                                          
    macros                                                                                                                                
    https://tinyurl.com/y9nfugth                                                                                                          
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories                                            
                                                                                                                                          
    Note: %utlnopts turs off macro generated code in the log                                                                              
          %utlopts turns it back on                                                                                                       
                                                                                                                                          
                                                                                                                                          
      Note: %utlnopts turs off macro generated code in the log                                                                            
            %utlopts turns it back on                                                                                                     
                                                                                                                                          
      Three solutions                                                                                                                     
                                                                                                                                          
           a. Array and do_over macros with hardcoded number of variables                                                                 
                                                                                                                                          
              data have;                                                                                                                  
                set sashelp.class;                                                                                                        
              run;quit;                                                                                                                   
                                                                                                                                          
              %utlnopts;                                                                                                                  
              %array(old,values=%varlist(have));                                                                                          
              %array(new,values=y1-y5);                                                                                                   
                                                                                                                                          
              proc datasets lib=work;                                                                                                     
                modify have;                                                                                                              
                rename %do_over(old new,phrase=%str(?old = ?new));                                                                        
              run;quit;                                                                                                                   
              %utlopts;                                                                                                                   
                                                                                                                                          
           b. Array and do_over without hardcoded number of variable                                                                      
                                                                                                                                          
              data have;                                                                                                                  
                set sashelp.class;                                                                                                        
              run;quit;                                                                                                                   
                                                                                                                                          
              %array(old,values=%varlist(have));                                                                                          
              %array(new,values=y1-y&oldn);       /* note y&oldn = Y5 */                                                                  
                                                                                                                                          
              proc datasets lib=work;                                                                                                     
                modify have;                                                                                                              
                rename %do_over(old new,phrase=%str(?old = ?new));                                                                        
              run;quit;                                                                                                                   
                                                                                                                                          
           c. DOSUBL (dee below)                                                                                                          
                                                                                                                                          
              Based on solution by                                                                                                        
              Paul Dorfman                                                                                                                
              sashole@bellsouth.net                                                                                                       
                                                                                                                                          
              * just in case it exists;                                                                                                   
              proc datasets lib=work mt=cat;                                                                                              
                delete tmp;                                                                                                               
              run;quit;                                                                                                                   
                                                                                                                                          
              data have;                                                                                                                  
                set sashelp.class;                                                                                                        
              run;quit;                                                                                                                   
                                                                                                                                          
              data log;                                                                                                                   
                                                                                                                                          
                 pgm="Renaming all sashelp.class to y1-yn";                                                                               
                                                                                                                                          
                 rc=dosubl(%tslit(                                                                                                        
                   filename wrk catalog 'work.tmp.catams' ;                                                                               
                   data _null_;                                                                                                           
                     length _v $8;                                                                                                        
                     file wrk;                                                                                                            
                      put                                                                                                                 
                        'proc datasets noprint; modify have;                                                                              
                          rename';                                                                                                        
                             do  _n_=0 by 1 until (_v = "") ;                                                                             
                               call vnext (_v) ;                                                                                          
                               if _v notin: ("_", "","_V") then put _v "=Y" _n_;                                                          
                             end ;                                                                                                        
                      put ';run;quit;';                                                                                                   
                      stop;                                                                                                               
                      set have;                                                                                                           
                      run;quit;                                                                                                           
                      %let ccgen=&syserr;                                                                                                 
                      %inc wrk;                                                                                                           
                      %let ccexec=&syserr;                                                                                                
                 ));                                                                                                                      
                                                                                                                                          
                 if symgetn("ccgen")=0 then status_gen = "code gen ok "; else status_gen="code gen err";                                  
                 if symgetn("ccexec")=0 then status_exec = "code exec ok "; else status_exec="code exec err";                             
                                                                                                                                          
               run;quit;                                                                                                                  
                                                                                                                                          
    *_                   _                                                                                                                
    (_)_ __  _ __  _   _| |_                                                                                                              
    | | '_ \| '_ \| | | | __|                                                                                                             
    | | | | | |_) | |_| | |_                                                                                                              
    |_|_| |_| .__/ \__,_|\__|                                                                                                             
            |_|                                                                                                                           
    ;                                                                                                                                     
                                                                                                                                          
    WORK.HAVE total obs=19                                                                                                                
                                                                                                                                          
     NAME       SEX    AGE    HEIGHT    WEIGHT                                                                                            
                                                                                                                                          
     Alfred      M      14     69.0      112.5                                                                                            
     Alice       F      13     56.5       84.0                                                                                            
     Barbara     F      13     65.3       98.0                                                                                            
     Carol       F      14     62.8      102.5                                                                                            
     Henry       M      14     63.5      102.5                                                                                            
     James       M      12     57.3       83.0                                                                                            
    ...                                                                                                                                   
                                                                                                                                          
    *            _               _                                                                                                        
      ___  _   _| |_ _ __  _   _| |_                                                                                                      
     / _ \| | | | __| '_ \| | | | __|                                                                                                     
    | (_) | |_| | |_| |_) | |_| | |_                                                                                                      
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                                     
                    |_|                                                                                                                   
    ;                                                                                                                                     
                                                                                                                                          
    WORK.LOG total obs=1                                                                                                                  
                                                                                                                                          
                     PGM                    RC    STATUS_GEN     STATUS_EXEC                                                              
                                                                                                                                          
     Renaming all sashelp.class to y1-yn     0    code gen ok    code exec ok                                                             
                                                                                                                                          
                                                                                                                                          
                                                                                                                                          
                                                                                                                                          
    WORK.HAVE total obs=19                                                                                                                
                                                                                                                                          
     Y1         Y2    Y3     Y4       Y5                                                                                                  
                                                                                                                                          
     Alfred     M     14    69.0    112.5                                                                                                 
     Alice      F     13    56.5     84.0                                                                                                 
     Barbara    F     13    65.3     98.0                                                                                                 
     Carol      F     14    62.8    102.5                                                                                                 
     Henry      M     14    63.5    102.5                                                                                                 
     James      M     12    57.3     83.0                                                                                                 
    ...                                                                                                                                   
                                                                                                                                          
    *                                                                                                                                     
     _ __  _ __ ___   ___ ___  ___ ___                                                                                                    
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                                                   
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                                   
    | .__/|_|  \___/ \___\___||___/___/                                                                                                   
    |_|                                                                                                                                   
    ;                                                                                                                                     
                                                                                                                                          
     * just in case it exists;                                                                                                            
     proc datasets lib=work mt=cat;                                                                                                       
       delete tmp;                                                                                                                        
     run;quit;                                                                                                                            
                                                                                                                                          
     data have;                                                                                                                           
       set sashelp.class;                                                                                                                 
     run;quit;                                                                                                                            
                                                                                                                                          
    * cleaner than 'call execute', easier to maitain?;                                                                                    
     data log;                                                                                                                            
                                                                                                                                          
        pgm="Renaming all sashelp.class to y1-yn";                                                                                        
                                                                                                                                          
        rc=dosubl(%tslit(                                                                                                                 
          filename wrk catalog 'work.tmp.catams' ;                                                                                        
          data _null_;                                                                                                                    
            length _v $8;                                                                                                                 
            file wrk;                                                                                                                     
             put                                                                                                                          
               'proc datasets noprint; modify have;                                                                                       
                 rename';                                                                                                                 
                    do  _n_=0 by 1 until (_v = "") ;                                                                                      
                      call vnext (_v) ;                                                                                                   
                      if _v notin: ("_", "","_V") then put _v "=Y" _n_;                                                                   
                    end ;                                                                                                                 
             put ';run;quit;';                                                                                                            
             stop;                                                                                                                        
             set have;                                                                                                                    
             run;quit;                                                                                                                    
             %let ccgen=&syserr;                                                                                                          
             %inc wrk;                                                                                                                    
             %let ccexec=&syserr;                                                                                                         
        ));                                                                                                                               
                                                                                                                                          
        if symgetn("ccgen")=0 then status_gen = "code gen ok "; else status_gen="code gen err";                                           
        if symgetn("ccexec")=0 then status_exec = "code exec ok "; else status_exec="code exec err";                                      
                                                                                                                                          
      run;quit;                                                                                                                           
                                                                                                                                          
