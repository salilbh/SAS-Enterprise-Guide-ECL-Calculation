# SAS-Enterprise-Guide-ECL-Calculation
Code used for development of ECL Calculation for BFSI (Raw Code)

/* -- Assign Library --*/

LIBNAME ECLSEP "F:\SAS Development\ECL_Sep17";
LIBNAME ECLMJ "F:\SAS Development\ECL_MarJun18";


LIBNAME SASMAR18 "F:\SAS Development\ECL_MarJun18\Input\RMD\31MAR2018LandA\";
LIBNAME SASJUN18 "F:\SAS Development\ECL_MarJun18\Input\RMD\30JUN2018LandA\";




/* -- Macros used in the code --*/


%Macro Import_Files (Data_Name, File_Location, Sheet, Range);


PROC IMPORT OUT= ECLMJ.&Data_Name
DATAFILE= &File_Location
DBMS=EXCEL REPLACE;
SHEET=&Sheet;
RANGE=&Range;
GETNAMES=YES;
RUN;


%Mend;


%Macro Sort_Data (Data_Name, by, Output);

Proc sort Data=&Data_Name Out=&Output;
by &by;
Run;

%Mend;


%Macro Sort_Data_NDK (Data_Name, by, Output);

Proc sort Data=&Data_Name Out=&Output nodupkey;
by &by;
Run;

%Mend;


%Macro Export_Data (Data, Outfile);

proc export 
  data=&Data
  dbms=xlsx 
  outfile=&Outfile
  replace;
run; 

%Mend;


%Macro Import_txt (Data_Name, File_Location);


PROC IMPORT OUT= ECLMJ.&Data_Name
DATAFILE= &File_Location
DBMS=DLM REPLACE;
Delimiter="09"x;
getnames=no; 
RUN;


%Mend;


%Macro Import_csv (Data_Name, File_Location);


proc import datafile=&File_Location 
out=ECLMJ.&Data_Name dbms=csv replace; 
getnames=yes; 
run;

%Mend;


%Macro Import_txtTAB_Transactions (Data_Name, File_Location);




/* -- Import Data --*/

DATA &Data_Name;
INFILE  &File_Location
            LRECL = 256
        DELIMITER = "09"X  /* DELIMITER = ","*/
        DSD
        FIRSTOBS = 2
        MISSOVER;
ATTRIB FORACID LENGTH = $16 INFORMAT = $16.;
ATTRIB TRAN_DATE LENGTH = 8 FORMAT = DDMMYY10. INFORMAT = DDMMYY10.;
ATTRIB TRAN_ID LENGTH = $9 INFORMAT = $9.;
ATTRIB TRAN_TYPE LENGTH = $1 INFORMAT = $1.;
ATTRIB TRAN_AMT LENGTH = 8 INFORMAT = BEST12.;
ATTRIB ACID LENGTH = $11 INFORMAT = $11.;
ATTRIB DEBIT LENGTH = 8 INFORMAT = BEST12.;
ATTRIB CREDIT LENGTH = 8 INFORMAT = BEST12.;
ATTRIB TRAN_PARTICULAR LENGTH = $100 INFORMAT = $100.;
ATTRIB INSTRMNT_TYPE LENGTH = $10 INFORMAT = $10.;
ATTRIB TRAN_RMKS LENGTH = $20 INFORMAT = $20.;
ATTRIB INSTRMNT_NUM LENGTH = 8 INFORMAT = BEST12.;
ATTRIB PSTD_DATE LENGTH = 8 FORMAT = DATETIME20. INFORMAT = ANYDTDTM40.;

INPUT FORACID TRAN_DATE TRAN_ID TRAN_TYPE TRAN_AMT ACID DEBIT CREDIT TRAN_PARTICULAR INSTRMNT_TYPE TRAN_RMKS INSTRMNT_NUM PSTD_DATE;
RUN;

%Mend;




%Macro Import_txtQuote_Transactions (Data_Name, File_Location);

DATA &Data_Name;
INFILE  &File_Location
            LRECL = 256
        DELIMITER = ","
        DSD
        FIRSTOBS = 2
        MISSOVER;
ATTRIB FORACID LENGTH = $16 INFORMAT = $16.;
ATTRIB TRAN_DATE LENGTH = 8 FORMAT = DDMMYY10. INFORMAT = DDMMYY10.;
ATTRIB TRAN_ID LENGTH = $9 INFORMAT = $9.;
ATTRIB TRAN_TYPE LENGTH = $1 INFORMAT = $1.;
ATTRIB TRAN_AMT LENGTH = 8 INFORMAT = BEST12.;
ATTRIB ACID LENGTH = $11 INFORMAT = $11.;
ATTRIB DEBIT LENGTH = 8 INFORMAT = BEST12.;
ATTRIB CREDIT LENGTH = 8 INFORMAT = BEST12.;
ATTRIB TRAN_PARTICULAR LENGTH = $100 INFORMAT = $100.;
ATTRIB INSTRMNT_TYPE LENGTH = $10 INFORMAT = $10.;
ATTRIB TRAN_RMKS LENGTH = $20 INFORMAT = $20.;
ATTRIB INSTRMNT_NUM LENGTH = 8 INFORMAT = BEST12.;
ATTRIB PSTD_DATE LENGTH = 8 FORMAT = DATETIME20. INFORMAT = ANYDTDTM40.;

INPUT FORACID TRAN_DATE TRAN_ID TRAN_TYPE TRAN_AMT ACID DEBIT CREDIT TRAN_PARTICULAR INSTRMNT_TYPE TRAN_RMKS INSTRMNT_NUM PSTD_DATE;
RUN;

%Mend;



%Import_Files (Data_Name=MISDATA_Mar181, File_Location="F:\SAS Development\ECL_MarJun18\Input\MIS\loan_dum_mar_1.xlsx", Sheet="Sheet1", Range="A1:BC900000");
%Import_Files (Data_Name=MISDATA_Mar182, File_Location="F:\SAS Development\ECL_MarJun18\Input\MIS\loan_dum_mar_2.xlsx", Sheet="Sheet1", Range="A1:BC758899");
%Import_Files (Data_Name=MISDATA_Jun181, File_Location="F:\SAS Development\ECL_MarJun18\Input\MIS\loan_dum_jun_1.xlsx", Sheet="Sheet1", Range="A1:BC900000");
%Import_Files (Data_Name=MISDATA_Jun182, File_Location="F:\SAS Development\ECL_MarJun18\Input\MIS\loan_dum_jun_2.xlsx", Sheet="Sheet1", Range="A1:BC811781");


%Import_Files (Data_Name=Bills_ACID2, File_Location="F:\SAS Development\ECL_MarJun18\Input\MIS\Bills_ACID.xlsx", Sheet="Sheet1", Range="A1:B1076");










%Import_Files (Data_Name=Pool_MJ2018, File_Location="F:\SAS Development\ECL_MarJun18\Input\Credito\Pool_MJ2018.xlsx", Sheet="Sheet1", Range="A1:C137");
%Import_Files (Data_Name=SMA_MJ2018, File_Location="F:\SAS Development\ECL_MarJun18\Input\MS&C\SMA_MJ2018.xlsx", Sheet="hc-02", Range="A1:C194891");
%Import_Files (Data_Name=Restructure_MJ2018, File_Location="F:\SAS Development\ECL_MarJun18\Input\MS&C\Restructure_MJ2018.xlsx", Sheet="Sheet1", Range="A1:C12932");
%Import_Files (Data_Name=MSME_MJ2018, File_Location="F:\SAS Development\ECL_MarJun18\Input\MSME\MSME_MJ2018.xlsx", Sheet="Sheet1", Range="A1:D522211");
%Import_Files (Data_Name=NPA_MJ2018, File_Location="F:\SAS Development\ECL_MarJun18\Input\R&R\NPA_MJ2018.xlsx", Sheet="AC2", Range="A1:Q249287");

%Import_Files (Data_Name=MOC_UNDRAWN_MJ2018, File_Location="F:\SAS Development\ECL_MarJun18\Input\RMD\MOC_UNDRAWNMJ2018.xlsx", Sheet="Sheet1", Range="A1:D1562");



%Import_Files (Data_Name=IRating_MJ2018, File_Location="F:\SAS Development\ECL_MarJun18\Input\RMD\InternalRating_MJ2018.xlsx", Sheet="Sheet1", Range="A1:Z33172");

%Import_Files (Data_Name=CorpPD_MJ2018, File_Location="F:\SAS Development\ECL_MarJun18\Input\RMD\InternalRating_MJ2018.xlsx", Sheet="Sheet2", Range="A1:C66");

%Import_Files (Data_Name=RetailPD_MJ2018, File_Location="F:\SAS Development\ECL_MarJun18\Input\RMD\InternalRating_MJ2018.xlsx", Sheet="Sheet3", Range="A1:D39");



%Import_Files (Data_Name=MOC_Undrawn_TL, File_Location="D:\ECL_MJ2018\MOC_Undrawn_TL.xlsx", Sheet="Sheet1", Range="A1:C34");
%Import_Files (Data_Name=MOC_Undrawn_WC, File_Location="D:\ECL_MJ2018\MOC_Undrawn_WC.xlsx", Sheet="MOC", Range="A1:C201");

%Import_Files (Data_Name=Restructure_MJ2018, File_Location="D:\ECL_MJ2018\Restructure.xlsx", Sheet="ABC", Range="A1:B7078");



%Import_Files (Data_Name=Bcode_Desc, File_Location="D:\ECL_MJ2018\Bcode_Desc.xlsx", Sheet="Sheet1", Range="A1:B20");
%Import_Files (Data_Name=BClass_Rectification, File_Location="D:\ECL_MJ2018\BClass_Rectification.xlsx", Sheet="Sheet1", Range="A1:U37");

%Import_Files (Data_Name=NPA_Balance_ROI, File_Location="D:\ECL_MJ2018\NPA Balance ROI.xlsx", Sheet="Sheet1", Range="A1:C200970");


%Import_Files (Data_Name=Closed_NPA, File_Location="D:\ECL_MJ2018\Closed NPA Date.xlsx", Sheet="Sheet1", Range="A1:C200970");

%Import_txt (Data_Name=CashRecovery, File_Location="D:\ECL_MJ2018\rmdcash_2010_2018.txt");


%Import_Files (Data_Name=BadDebt_Writeoff, File_Location="D:\ECL_MJ2018\sal_1.xlsx", Sheet="Sheet3", Range="A1:H133545");
%Import_txt (Data_Name=MOI_Writeoff, File_Location="D:\ECL_MJ2018\sal3.txt");



Data ECLMJ.MOI_Writeoff;
Set ECLMJ.MOI_Writeoff (rename= (VAR1=SOL_ID VAR2=DR_FORACID VAR3=DR_AMT VAR4=CR_FORACID VAR5=TRAN_ID VAR6=TRAN_DATE VAR7=CR_AMT VAR8=TRAN_PARTICULAR));
Run;


Data ECLMJ.LGD_ACSTATEMENTS1;
Set ECLMJ.TRAN_NPA_LGD_2009 ECLMJ.TRAN_NPA_LGD_2010 ECLMJ.TRAN_NPA_LGD_2011 ECLMJ.TRAN_NPA_LGD_2012 ECLMJ.TRAN_NPA_LGD_2013 ECLMJ.TRAN_NPA_LGD_2014 ECLMJ.TRAN_NPA_LGD_2015 ECLMJ.TRAN_NPA_LGD_2016 ECLMJ.TRAN_NPA_LGD_2017;
Run;


/* Transaction Details List 1 Import and rectification*/

%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS1, File_Location='D:\ECL_MJ2018\TRAN_NPA_LGD_2009.txt');
%Import_txtQuote_Transactions (Data_Name=LGD_ACSTATEMENTS2, File_Location='D:\ECL_MJ2018\TRAN_NPA_LGD_2010.txt');
%Import_txtQuote_Transactions (Data_Name=LGD_ACSTATEMENTS3, File_Location='D:\ECL_MJ2018\TRAN_NPA_LGD_2011.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS4, File_Location='D:\ECL_MJ2018\TRAN_NPA_LGD_2012.txt');
%Import_txtQuote_Transactions (Data_Name=LGD_ACSTATEMENTS5, File_Location='D:\ECL_MJ2018\TRAN_NPA_LGD_2013.txt');
%Import_txtQuote_Transactions (Data_Name=LGD_ACSTATEMENTS6, File_Location='D:\ECL_MJ2018\TRAN_NPA_LGD_2014.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS7, File_Location='D:\ECL_MJ2018\TRAN_NPA_LGD_2015.txt');
%Import_txtQuote_Transactions (Data_Name=LGD_ACSTATEMENTS8, File_Location='D:\ECL_MJ2018\TRAN_NPA_LGD_2016.txt');

%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS9, File_Location='D:\ECL_MJ2018\TRAN_NPA_LGD_Jan_Mar17.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS10, File_Location='D:\ECL_MJ2018\TRAN_NPA_LGD_APR17.txt');
Data LGD_ACSTATEMENTS10;
set LGD_ACSTATEMENTS10;
if Tran_Date = '30Mar2017'd then Delete;
if tran_Date = '31Mar2017'd then Delete;
Run;
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS11, File_Location='D:\ECL_MJ2018\TRAN_NPA_LGD_JUN17.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS12, File_Location='D:\ECL_MJ2018\TRAN_NPA_LGD_JUL-DEC17.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS13, File_Location='D:\ECL_MJ2018\TRAN_JAN20117.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS14, File_Location='D:\ECL_MJ2018\TRAN_JUL20117.txt');


Data LGD_ACSTATEMENTS_List1;
Set LGD_ACSTATEMENTS1 LGD_ACSTATEMENTS2 LGD_ACSTATEMENTS3 LGD_ACSTATEMENTS4 LGD_ACSTATEMENTS5 LGD_ACSTATEMENTS6 LGD_ACSTATEMENTS7 LGD_ACSTATEMENTS8 LGD_ACSTATEMENTS9 LGD_ACSTATEMENTS10 LGD_ACSTATEMENTS11 LGD_ACSTATEMENTS12 LGD_ACSTATEMENTS13 LGD_ACSTATEMENTS14;
Run;
Data LGD_ACSTATEMENTS_List1;
Set LGD_ACSTATEMENTS_List1 (rename= FORACID=ACCOUNTID);
If ACCOUNTID ne "" then FORACID = ACCOUNTID*1;
Run;
Data ECLMJ.LGD_ACSTATEMENTS_List1;
Set LGD_ACSTATEMENTS_List1;
If FORACID = . then Delete;
Run;
/*-------------------------END Transaction List 1 Creation---------------------------*/


/* Transaction Details List 2 Import and rectification*/
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS15, File_Location='D:\ECL_MJ2018\TRAN_2009_2010.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS16, File_Location='D:\ECL_MJ2018\TRAN_2011_2012.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS17, File_Location='D:\ECL_MJ2018\TRAN_2013_2014.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS18, File_Location='D:\ECL_MJ2018\TRAN_2015_2016.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS19, File_Location='D:\ECL_MJ2018\TRAN_FEB_MAR20116.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS20, File_Location='D:\ECL_MJ2018\TRAN_APR_JUN20116.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS21, File_Location='D:\ECL_MJ2018\TRAN_JUL_SEP20116.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS22, File_Location='D:\ECL_MJ2018\TRAN_OCT_DEC2016.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS23, File_Location='D:\ECL_MJ2018\TRAN_JAN_JUN2017.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS24, File_Location='D:\ECL_MJ2018\TRAN_JUL_SEP2017.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS25, File_Location='D:\ECL_MJ2018\TRAN_OCT_DEC2017.txt');
%Import_txtTAB_Transactions (Data_Name=LGD_ACSTATEMENTS26, File_Location='D:\ECL_MJ2018\TRAN_JAN_FEB2018.txt');


Data LGD_ACSTATEMENTS_List2;
Set LGD_ACSTATEMENTS15 LGD_ACSTATEMENTS16 LGD_ACSTATEMENTS17 LGD_ACSTATEMENTS18 LGD_ACSTATEMENTS19 LGD_ACSTATEMENTS20 LGD_ACSTATEMENTS21 LGD_ACSTATEMENTS22 LGD_ACSTATEMENTS23 LGD_ACSTATEMENTS24 LGD_ACSTATEMENTS25 LGD_ACSTATEMENTS26;
Run;

Data LGD_ACSTATEMENTS_List2;
Set LGD_ACSTATEMENTS_List2 (rename= FORACID=ACCOUNTID);
If ACCOUNTID ne "" then FORACID = ACCOUNTID*1;
Run;
Data ECLMJ.LGD_ACSTATEMENTS_List2;
Set LGD_ACSTATEMENTS_List2;
If FORACID = . then Delete;
Run;

/*-------------------------END Transaction List 2 Creation---------------------------*/


/* ------------------------- Setting up Combined Transaction Data---------------------------*/

Data ECLMJ.LGD_ACSTATEMENTS;
Set ECLMJ.LGD_ACSTATEMENTS_List1 ECLMJ.LGD_ACSTATEMENTS_List2;
Run;

/* --END-------------------- Setting up Combined Transaction Data---------------------------*/




/*Check month wise all dates in data*/

Data abc (keep=ACID Tran_Date);
Set LGD_ACSTATEMENTS_List2;
Run;

%Sort_Data_NDK (Data_Name=abc, by=tran_date, Output=xyz);

/*END - Check month wise all dates in data*/


/*Check Duplicate entries */

Data LGD_ACSTATEMENTS_chk;
Set LGD_ACSTATEMENTS;
where FORACID = "602306211000005";
Run;

/*------*/




/* --------------- Create Intermediate Tables - SAS Standardised Approach Data Tables ------------*/

LIBNAME SASMAR18 "F:\SAS Development\ECL_MarJun18\Input\RMD\31MAR2018LandA\";
LIBNAME SASJUN18 "F:\SAS Development\ECL_MarJun18\Input\RMD\30JUN2018LandA\";


Data ECLMJ.SAS_RWAMJ2018;
set SASMAR18.REGULATORY_CAPITAL_DETAIL_LA SASJUN18.regulatory_capital_detail_la;
Run;

Data SAS_RWAMJ2018;
set ECLMJ.SAS_RWAMJ2018;
if Account_ID ne "" then ACID = substr(Account_ID,1,15);
Run;

Data SAS_RWAMJ2018;
set SAS_RWAMJ2018;
if ACID*1 = ACID then ACID_NUM = ACID*1;
Run;



Data ECLMJ.BillsM18;
set ECLMJ.tb_fb_mar18 ECLMJ.tb_ib_mar18;
Run;

Data ECLMJ.BillsM18;
format Reporting_Date date9.;
Set ECLMJ.BillsM18;
Reporting_Date = '31Mar2018'd;
Run;

Data ECLMJ.BillsJ18;
set eclmj.tb_fb_jun18 eclmj.tb_ib_jun18;
Run;

Data ECLMJ.BillsJ18;
format Reporting_Date date9.;
Set ECLMJ.BillsJ18;
Reporting_Date = '30JUN2018'd;
Run;

Data ECLMJ.Bills_ACID (drop=ORIGINAL_ACCOUNT_NUMBER);
Set ECLMJ.BillsM18 ECLMJ.BillsJ18;
if ORIGINAL_ACCOUNT_NUMBER*1=ORIGINAL_ACCOUNT_NUMBER then ACID_NUM = ORIGINAL_ACCOUNT_NUMBER*1;
Run;

Data ECLMJ.Bills_ACID (Rename=ACCOUNTID=ACID);
set ECLMJ.Bills_ACID;
Run;


%Sort_Data (Data_Name=SAS_RWAMJ2018, by=Reporting_Date ACID, Output=SAS_RWAMJ2018);
%Sort_Data (Data_Name=ECLMJ.Bills_ACID, by=Reporting_Date ACID, Output=ECLMJ.Bills_ACID);

Data SAS_RWAMJ2018;
Merge SAS_RWAMJ2018 ECLMJ.Bills_ACID;
by Reporting_Date ACID;
Run;

Data ECLMJ.SAS_RWAMJ2018;
set SAS_RWAMJ2018;
if file_name = "CARDS" then ACID_NUM = 0;
Run;

Data a1 (keep=ACID ACID_NUM);
set ECLMJ.SAS_RWAMJ2018;
where file_name ="BILLS";
Run;
Data a1 (rename=ACID_NUM=ACID_NUM_Bill);
set a1;
if ACID_NUM = . then Delete;
Run;

%Sort_Data_NDK (Data_Name=a1, by=ACID, Output=a1);
%Sort_Data (Data_Name=ECLMJ.SAS_RWAMJ2018, by=ACID, Output=ECLMJ.SAS_RWAMJ2018);

Data SAS_RWAMJ2018;
Merge ECLMJ.SAS_RWAMJ2018 a1;
by ACID;
Run;

Data SAS_RWAMJ2018;
set SAS_RWAMJ2018;
if ACID_NUM = . and ACID_NUM_Bill ne . then ACID_NUM = ACID_NUM_Bill;
Run;

Data ECLMJ.SAS_RWAMJ2018 (drop=ACID_NUM_Bill);
set SAS_RWAMJ2018;
Run;


Data ECLMJ.SAS_BCode (keep= Reporting_Date ACID_NUM Constitution rename=Reporting_Date=StatementDate);
Set ECLMJ.SAS_RWAMJ2018;
if Constitution = "BS-1D" then Delete;
If Constitution = "BS-1E" then Delete;
If Constitution = "BS-1G" then Delete;
If ACID_NUM = . then Delete;
If Reporting_Date = . then Delete;
Run;
%Sort_Data_NDK (Data_Name=ECLMJ.SAS_BCode, by=StatementDate ACID_NUM, Output=ECLMJ.SAS_BCode);


Data ECLMJ.GUARANTEED (Keep=Reporting_Date ACID_NUM Constitution);
Set ECLMJ.SAS_RWAMJ2018;
where Constitution in ("BS-1E", "BS-1G");
Run;
Data ECLMJ.GUARANTEED (Rename=Reporting_Date=StatementDate);
Set ECLMJ.GUARANTEED;
/*informat Guarantor $CHAR28. Guarantee Best15.;*/
if Constitution = "BS-1E" then Guarantor="Exposure Guaranteed By CGFTS";
if Constitution = "BS-1E" then Guarantee=0.70;
if Constitution = "BS-1G" then Guarantor="Exposure Guaranteed By ECGC";
if Constitution = "BS-1G" then Guarantee=0.75;
/*format acid_num Best15.
Guarantor $CHAR28. Guarantee Best15.;*/
Run;


/* RWA CRM Etc */
proc contents data= ECLSEP.FBACID_RWA  short; 
        run;


Data SAS_RWAMJ2018;
Set ECLMJ.SAS_RWAMJ2018;
where File_name ne "LCBG";
Run;

%Sort_Data (Data_Name=ECLMJ.MOC_UNDRAWN_MJ2018, by=Reporting_Date ACID_NUM, Output=MOC_UNDRAWN_MJ2018);
%Sort_Data (Data_Name=SAS_RWAMJ2018, by=Reporting_Date ACID_NUM, Output=SAS_RWAMJ2018);

Data SAS_RWAMJ2018;
Merge SAS_RWAMJ2018 MOC_UNDRAWN_MJ2018;
By Reporting_Date ACID_NUM;
Run;


Data SAS_RWAMJ2018; Set SAS_RWAMJ2018;
If New_undrawn ne . then UNDRAWN_AMT = New_undrawn;
If New_Risk_Weight ne . then rwa_undrawn = New_Risk_Weight;
Run;

Data ECLMJ.SAS_RWAMJ2018_PostMOC;
Set SAS_RWAMJ2018;
Run;

PROC SQL;
  CREATE TABLE ECLMJ.FBACID_RWA AS
    SELECT Reporting_Date as StatementDate,
			ACID_NUM AS ACID_NUM,
			Count(ACID_NUM) As Count,
			Sum(Outstanding3) AS Balance_Outstanding,
           Sum(rwa_Outstanding) AS Sum_RWA,
		   Sum(rwa_undrawn) AS Sum_RWA_Undrawn,
		   Sum(post_crm_collateral_var) As CRM,

		   Sum(undrawn_amt) As Total_Undrawn,
		   Sum((undrawn_amt*CCF_undrawn/100)-FCH_Undrawn) As CRM_Undrawn

    FROM   ECLMJ.SAS_RWAMJ2018_PostMOC
  GROUP BY Reporting_Date, ACID_NUM;
QUIT;

Data ECLMJ.FBACID_RWA;
Set ECLMJ.FBACID_RWA;
if statementdate = . then delete;
Run;


/*------------------------------------------------------------------------------------*/
/*------------------------------------------------------------------------------------*/




/*-----------Setting Up Base Data---------*/

%Import_Files (Data_Name=MISDATA_Mar181, File_Location="F:\SAS Development\ECL_MarJun18\Input\MIS\loan_dum_mar_1.xlsx", Sheet="Sheet1", Range="A1:BC900000");
%Import_Files (Data_Name=MISDATA_Mar182, File_Location="F:\SAS Development\ECL_MarJun18\Input\MIS\loan_dum_mar_2.xlsx", Sheet="Sheet1", Range="A1:BC758899");
%Import_Files (Data_Name=MISDATA_Jun181, File_Location="F:\SAS Development\ECL_MarJun18\Input\MIS\loan_dum_jun_1.xlsx", Sheet="Sheet1", Range="A1:BC900000");
%Import_Files (Data_Name=MISDATA_Jun182, File_Location="F:\SAS Development\ECL_MarJun18\Input\MIS\loan_dum_jun_2.xlsx", Sheet="Sheet1", Range="A1:BC811781");

Data ECLMJ.MIS_MJ2018;
set ECLMJ.MISDATA_JUN181 ECLMJ.MISDATA_JUN182 ECLMJ.misdata_mar181 ECLMJ.misdata_mar182;
Run;

Data MIS_MJ2018;
Set ECLMJ.MIS_MJ2018;
if FORACID ne . then ACID_NUM =FORACID;
Run;

Data NPA_MJ2018 (keep=StatementDate ACID_NUM NPA_DATE PWO GROSS_NPA GROSS_PROV NPA_Class);
Set ECLMJ.NPA_MJ2018 (rename=(ACID=ACID_NUM Statement_Date=StatementDate));
Run;

%Sort_Data (Data_Name=MIS_MJ2018, by=StatementDate ACID_NUM, Output=MIS_MJ2018);
%Sort_Data (Data_Name=NPA_MJ2018, by=StatementDate ACID_NUM, Output=NPA_MJ2018);
Data MIS_MJ2018;
Merge MIS_MJ2018 NPA_MJ2018;
by StatementDate ACID_NUM;
if Statementdate = '31MAR2018'd and ACID_NUM = 608609041000001 then Balance_GL = 32128719;
if Statementdate = '31MAR2018'd and ACID_NUM = 608609041000001 then FORACID = 608609041000001;
if Statementdate = '31MAR2018'd and ACID_NUM = 0 then Balance_GL = Gross_NPA;
if Statementdate = '30JUN2018'd and ACID_NUM = 0 then Balance_GL = Gross_NPA;
if ACID_Num = 0 then FORACID = 0;
Run;
/*-----------------------------------------*/

Data Abc;
Set MIS_MJ2018;
where Foracid = 0;
Run;

Data Abc;
set Abc;
if Foracid = 0 and StatementDate='31Mar2018'd then BALANCE_GL = -682296740.49;
if Foracid = 0 and StatementDate='30JUN2018'd then BALANCE_GL = 534713077.55;
if Foracid = 0 then NPA_DATE = .;
if Foracid = 0 then ACID_NUM = 1;
if Foracid = 0 then NPA_Class = .;
if Foracid = 0 then PWO = .;
if Foracid = 0 then GROSS_NPA = .;
if Foracid = 0 then GROSS_PROV = .;
if Foracid = 0 then Foracid = 1;
Run;


Data MIS_MJ2018;
Set MIS_MJ2018 Abc;
Run;




/*-----------------------------------------*/
Data MSME_MJ2018 (Drop=FORACID);
Set ECLMJ.MSME_MJ2018 (rename= (ACID=ACID_NUM statement_Date=Statementdate));
Run;
%Sort_Data (Data_Name=MSME_MJ2018, by=StatementDate ACID_NUM, Output=MSME_MJ2018);
%Sort_Data (Data_Name=MIS_MJ2018, by=StatementDate ACID_NUM, Output=MIS_MJ2018);
Data MIS_MJ2018;
Merge MIS_MJ2018 MSME_MJ2018;
by StatementDate ACID_NUM;
Run;



Data SMA_MJ2018;
Set ECLMJ.SMA_MJ2018(rename= (ACID=ACID_NUM statement_Date=Statementdate));
Run;
%Sort_Data (Data_Name=SMA_MJ2018, by=StatementDate ACID_NUM, Output=SMA_MJ2018);
%Sort_Data (Data_Name=MIS_MJ2018, by=StatementDate ACID_NUM, Output=MIS_MJ2018);
Data MIS_MJ2018;
Merge MIS_MJ2018 SMA_MJ2018;
by StatementDate ACID_NUM;
if FORACID = . then Delete;
Run;




Proc sql;
create table CUSTBAL as
select StatementDate, CUST_ID, Sum(BALANCE_GL) as CUST_Balance from MIS_MJ2018
Group by StatementDate, CUST_ID;
Quit;

Data CUSTBAL;
Set CUSTBAL;
if CUST_ID = . then Delete;
Run;
%Sort_Data (Data_Name=CUSTBAL, by=StatementDate CUST_ID, Output=CUSTBAL);
%Sort_Data (Data_Name=MIS_MJ2018, by=StatementDate CUST_ID, Output=MIS_MJ2018);

Data MIS_MJ2018;
Merge MIS_MJ2018 CUSTBAL;
by StatementDate CUST_ID;
Run;




%Sort_Data (Data_Name=MIS_MJ2018, by=StatementDate ACID_NUM, Output=MIS_MJ2018);

Data sas_bcode(Rename=Constitution=Class);
Set ECLMJ.SAS_BCODE;
Run;
%Sort_Data (Data_Name=SAS_BCode, by=StatementDate ACID_NUM, Output=SAS_BCode);
Data MIS_MJ2018;
Merge MIS_MJ2018 SAS_BCode;
by StatementDate ACID_Num;
Run;

Data MIS_MJ2018;
Set MIS_MJ2018;
if FORACID = . then Delete;
Run;



Data RESTRUCTURE_MJ2018 (Rename=(ACID=ACID_NUM statement_Date=Statementdate));
Set ECLMJ.Restructure_mj2018;
Run;

%Sort_Data (Data_Name=RESTRUCTURE_MJ2018, by=StatementDate ACID_NUM, Output=RESTRUCTURE_MJ2018);

Data MIS_MJ2018;
Merge MIS_MJ2018 RESTRUCTURE_MJ2018;
by StatementDate ACID_NUM;
Run;

Data MIS_MJ2018;
Set MIS_MJ2018;
if Foracid = . then Delete;
Run;



Data POOL_MJ2018(Rename=(ACID=ACID_NUM statement_Date=Statementdate));
set eclmj.pool_mj2018;
Run;
%Sort_Data (Data_Name=POOL_MJ2018, by=StatementDate ACID_NUM, Output=POOL_MJ2018);

Data MIS_MJ2018;
Merge MIS_MJ2018 POOL_MJ2018;
by StatementDate ACID_NUM;
Run;



%Sort_Data_NDK (Data_Name=ECLMJ.GUARANTEED, by=StatementDate ACID_NUM, Output=GUARANTEED);

Data MIS_MJ2018;
Merge MIS_MJ2018 GUARANTEED;
by StatementDate ACID_NUM;
if FORACID = . then Delete;
Run;

/*--------------------Save-------------------*/
Data ECLMJ.MIS_MJ2018_2;
set MIS_MJ2018;
Run;
/*--------------------Save-------------------*/


Data MIS_MJ2018;
set ECLMJ.MIS_MJ2018_2;
Run;

Data MIS_MJ2018;
Merge MIS_MJ2018 ECLMJ.FBACID_RWA;
by statementdate ACID_NUM;
Run;

Data MIS_MJ2018;
set MIS_MJ2018;
if FORACID = . then Delete;
Run;


Data MIS_MJ2018;
Set MIS_MJ2018;
If Balance_Outstanding = . then Balance_Outstanding = Balance_GL;
If Sum_RWA = . then Sum_RWA = 0;
If Sum_RWA_Undrawn = . then Sum_RWA_Undrawn = 0;
If CRM = . then CRM = 0;
If Total_Undrawn = . then Total_Undrawn = 0;
If CRM_Undrawn = . then CRM_Undrawn = 0;
Run;

Data MIS_MJ2018;
Set MIS_MJ2018;
if Class = "" and CUST_Balance > 50000000 then Class = "BS-07";
if Class = "" and CUST_Balance < 50000001 then Class = "BS-08";
Run;


Data IRATING_MJ2018 (rename=Customer_ID=Cust_ID);
Set ECLMJ.IRATING_MJ2018(Keep=Customer_ID I2018);
Run;

%Sort_Data (Data_Name=IRATING_MJ2018, by=Cust_ID, Output=IRATING_MJ2018);
%Sort_Data (Data_Name=MIS_MJ2018, by=Cust_ID, Output=MIS_MJ2018);

Data MIS_MJ2018;
Merge MIS_MJ2018 IRATING_MJ2018;
by Cust_ID;
Run;

Data MIS_MJ2018;
Set MIS_MJ2018;
if FORACID = . then delete;
Run;

Data ECLMJ.MIS_MJ2018_Base; Set MIS_MJ2018; Run;


%Import_Files (Data_Name=MISDATA_Mar181, File_Location="F:\SAS Development\ECL_MarJun18\Input\MIS\loan_dum_mar_1.xlsx", Sheet="Sheet1", Range="A1:BC900000");
%Import_Files (Data_Name=MISDATA_Mar182, File_Location="F:\SAS Development\ECL_MarJun18\Input\MIS\loan_dum_mar_2.xlsx", Sheet="Sheet1", Range="A1:BC758899");
%Import_Files (Data_Name=MISDATA_Jun181, File_Location="F:\SAS Development\ECL_MarJun18\Input\MIS\loan_dum_jun_1.xlsx", Sheet="Sheet1", Range="A1:BC900000");
%Import_Files (Data_Name=MISDATA_Jun182, File_Location="F:\SAS Development\ECL_MarJun18\Input\MIS\loan_dum_jun_2.xlsx", Sheet="Sheet1", Range="A1:BC811781");

Data ECLMJ.MIS_MJ2018;
set ECLMJ.MISDATA_JUN181 ECLMJ.MISDATA_JUN182 ECLMJ.misdata_mar181 ECLMJ.misdata_mar182;
Run;

Data MIS_MJ2018;
Set ECLMJ.MIS_MJ2018;
if FORACID ne . then ACID_NUM =FORACID;
Run;

Data NPA_MJ2018 (keep=StatementDate ACID_NUM NPA_DATE PWO GROSS_NPA GROSS_PROV NPA_Class);
Set ECLMJ.NPA_MJ2018 (rename=(ACID=ACID_NUM Statement_Date=StatementDate));
Run;

%Sort_Data (Data_Name=MIS_MJ2018, by=StatementDate ACID_NUM, Output=MIS_MJ2018);
%Sort_Data (Data_Name=NPA_MJ2018, by=StatementDate ACID_NUM, Output=NPA_MJ2018);
Data MIS_MJ2018;
Merge MIS_MJ2018 NPA_MJ2018;
by StatementDate ACID_NUM;
if Statementdate = '31MAR2018'd and ACID_NUM = 608609041000001 then Balance_GL = 32128719;
if Statementdate = '31MAR2018'd and ACID_NUM = 608609041000001 then FORACID = 608609041000001;
if Statementdate = '31MAR2018'd and ACID_NUM = 0 then Balance_GL = Gross_NPA;
if Statementdate = '30JUN2018'd and ACID_NUM = 0 then Balance_GL = Gross_NPA;
if ACID_Num = 0 then FORACID = 0;
Run;
/*-----------------------------------------*/

Data Abc;
Set MIS_MJ2018;
where Foracid = 0;
Run;

Data Abc;
set Abc;
if Foracid = 0 and StatementDate='31Mar2018'd then BALANCE_GL = -682296740.49;
if Foracid = 0 and StatementDate='30JUN2018'd then BALANCE_GL = 534713077.55;
if Foracid = 0 then NPA_DATE = .;
if Foracid = 0 then ACID_NUM = 1;
if Foracid = 0 then NPA_Class = .;
if Foracid = 0 then PWO = .;
if Foracid = 0 then GROSS_NPA = .;
if Foracid = 0 then GROSS_PROV = .;
if Foracid = 0 then Foracid = 1;
Run;


Data MIS_MJ2018;
Set MIS_MJ2018 Abc;
Run;




/*-----------------------------------------*/
Data MSME_MJ2018 (Drop=FORACID);
Set ECLMJ.MSME_MJ2018 (rename= (ACID=ACID_NUM statement_Date=Statementdate));
Run;
%Sort_Data (Data_Name=MSME_MJ2018, by=StatementDate ACID_NUM, Output=MSME_MJ2018);
%Sort_Data (Data_Name=MIS_MJ2018, by=StatementDate ACID_NUM, Output=MIS_MJ2018);
Data MIS_MJ2018;
Merge MIS_MJ2018 MSME_MJ2018;
by StatementDate ACID_NUM;
Run;



Data SMA_MJ2018;
Set ECLMJ.SMA_MJ2018(rename= (ACID=ACID_NUM statement_Date=Statementdate));
Run;
%Sort_Data (Data_Name=SMA_MJ2018, by=StatementDate ACID_NUM, Output=SMA_MJ2018);
%Sort_Data (Data_Name=MIS_MJ2018, by=StatementDate ACID_NUM, Output=MIS_MJ2018);
Data MIS_MJ2018;
Merge MIS_MJ2018 SMA_MJ2018;
by StatementDate ACID_NUM;
if FORACID = . then Delete;
Run;




Proc sql;
create table CUSTBAL as
select StatementDate, CUST_ID, Sum(BALANCE_GL) as CUST_Balance from MIS_MJ2018
Group by StatementDate, CUST_ID;
Quit;

Data CUSTBAL;
Set CUSTBAL;
if CUST_ID = . then Delete;
Run;
%Sort_Data (Data_Name=CUSTBAL, by=StatementDate CUST_ID, Output=CUSTBAL);
%Sort_Data (Data_Name=MIS_MJ2018, by=StatementDate CUST_ID, Output=MIS_MJ2018);

Data MIS_MJ2018;
Merge MIS_MJ2018 CUSTBAL;
by StatementDate CUST_ID;
Run;




%Sort_Data (Data_Name=MIS_MJ2018, by=StatementDate ACID_NUM, Output=MIS_MJ2018);

Data sas_bcode(Rename=Constitution=Class);
Set ECLMJ.SAS_BCODE;
Run;
%Sort_Data (Data_Name=SAS_BCode, by=StatementDate ACID_NUM, Output=SAS_BCode);
Data MIS_MJ2018;
Merge MIS_MJ2018 SAS_BCode;
by StatementDate ACID_Num;
Run;

Data MIS_MJ2018;
Set MIS_MJ2018;
if FORACID = . then Delete;
Run;



Data RESTRUCTURE_MJ2018 (Rename=(ACID=ACID_NUM statement_Date=Statementdate));
Set ECLMJ.Restructure_mj2018;
Run;

%Sort_Data (Data_Name=RESTRUCTURE_MJ2018, by=StatementDate ACID_NUM, Output=RESTRUCTURE_MJ2018);

Data MIS_MJ2018;
Merge MIS_MJ2018 RESTRUCTURE_MJ2018;
by StatementDate ACID_NUM;
Run;

Data MIS_MJ2018;
Set MIS_MJ2018;
if Foracid = . then Delete;
Run;



Data POOL_MJ2018(Rename=(ACID=ACID_NUM statement_Date=Statementdate));
set eclmj.pool_mj2018;
Run;
%Sort_Data (Data_Name=POOL_MJ2018, by=StatementDate ACID_NUM, Output=POOL_MJ2018);

Data MIS_MJ2018;
Merge MIS_MJ2018 POOL_MJ2018;
by StatementDate ACID_NUM;
Run;



%Sort_Data_NDK (Data_Name=ECLMJ.GUARANTEED, by=StatementDate ACID_NUM, Output=GUARANTEED);

Data MIS_MJ2018;
Merge MIS_MJ2018 GUARANTEED;
by StatementDate ACID_NUM;
if FORACID = . then Delete;
Run;

/*--------------------Save-------------------*/
Data ECLMJ.MIS_MJ2018_2;
set MIS_MJ2018;
Run;
/*--------------------Save-------------------*/


Data MIS_MJ2018;
set ECLMJ.MIS_MJ2018_2;
Run;

Data MIS_MJ2018;
Merge MIS_MJ2018 ECLMJ.FBACID_RWA;
by statementdate ACID_NUM;
Run;

Data MIS_MJ2018;
set MIS_MJ2018;
if FORACID = . then Delete;
Run;


Data MIS_MJ2018;
Set MIS_MJ2018;
If Balance_Outstanding = . then Balance_Outstanding = Balance_GL;
If Sum_RWA = . then Sum_RWA = 0;
If Sum_RWA_Undrawn = . then Sum_RWA_Undrawn = 0;
If CRM = . then CRM = 0;
If Total_Undrawn = . then Total_Undrawn = 0;
If CRM_Undrawn = . then CRM_Undrawn = 0;
Run;

Data MIS_MJ2018;
Set MIS_MJ2018;
if Class = "" and CUST_Balance > 50000000 then Class = "BS-07";
if Class = "" and CUST_Balance < 50000001 then Class = "BS-08";
Run;


Data IRATING_MJ2018 (rename=Customer_ID=Cust_ID);
Set ECLMJ.IRATING_MJ2018(Keep=Customer_ID I2018);
Run;

%Sort_Data (Data_Name=IRATING_MJ2018, by=Cust_ID, Output=IRATING_MJ2018);
%Sort_Data (Data_Name=MIS_MJ2018, by=Cust_ID, Output=MIS_MJ2018);

Data MIS_MJ2018;
Merge MIS_MJ2018 IRATING_MJ2018;
by Cust_ID;
Run;

Data MIS_MJ2018;
Set MIS_MJ2018;
if FORACID = . then delete;
Run;

Data ECLMJ.MIS_MJ2018_Base; Set MIS_MJ2018; Run;


/*------------------- ----------------------------------*/


/*------------------- Classification (PD)----------------------------------*/

Data MIS_MJ2018_C; Set ECLMJ.MIS_MJ2018_Base; Run;


data MIS_MJ2018_C;
format Classification $20.;
set MIS_MJ2018_C;
if Class = "BS-03" then Classification = "Corporate and PSE";
if Class = "BS-07" then Classification = "Corporate and PSE";
if Class = "BS-07U" then Classification = "Corporate and PSE";
if Class = "BS-1F" then Classification = "Corporate and PSE";
run;


data MIS_MJ2018_C;
set MIS_MJ2018_C;
if classification="" and Schm_code in() then Classification='AGRI';
run;

data MIS_MJ2018_C;
set MIS_MJ2018_C;
if classification="" and Schm_code in() then Classification='AUTO';
run;

data MIS_MJ2018_C;
set MIS_MJ2018_C;
if classification="" and Schm_code in() then Classification='EDU';
run;

data MIS_MJ2018_C;
set MIS_MJ2018_C;
if classification="" and Schm_code in() then Classification='HL';
run;

data MIS_MJ2018_C;
set MIS_MJ2018_C;
if classification="" and Schm_code in() then Classification='PL';
run;

data MIS_MJ2018_C;
set MIS_MJ2018_C;
if classification="" and Schm_code in() then Classification='Other Retail';
run;

data MIS_MJ2018_C;
set MIS_MJ2018_C;
if CLASS_POOL = "Pool" then Classification='Other Retail';
run;


data MIS_MJ2018_C;
set MIS_MJ2018_C;
if classification="" and CUST_Balance>50000000 and Schm_code in() then Classification='Corporate and PSE';
run;
data MIS_MJ2018_C;
set MIS_MJ2018_C;
if classification="" and CUST_Balance<50000001 and Schm_code in() then Classification='AUTO';
run;

data MIS_MJ2018_C;
set MIS_MJ2018_C;
if classification="" and CUST_Balance>50000000 and Schm_code in() then Classification='Corporate and PSE';
run;
data MIS_MJ2018_C;
set MIS_MJ2018_C;
if classification="" and CUST_Balance<50000001 and Schm_code in() then Classification='Other Retail';
run;


data MIS_MJ2018_C;
set MIS_MJ2018_C;
if Class_MSME = "MSME" then Classification = "MSME";
run;


data MIS_MJ2018_C;
set MIS_MJ2018_C;
if classification="" and Schm_code in() then Classification='Other Retail';
run;

data MIS_MJ2018_C;
set MIS_MJ2018_C;
if classification="" and Schm_code in() then Classification='Other Retail';
run;

data MIS_MJ2018_C;
set MIS_MJ2018_C;
if classification="" and ACID_NUM = 0 then Classification="Corporate and PSE";
run;





data MIS_MJ2018_C;
set MIS_MJ2018_C;
if classification="" and CUST_Balance>50000000 and Schm_code in() then Classification='Corporate and PSE';
run;
data MIS_MJ2018_C;
set MIS_MJ2018_C;
if classification="" and CUST_Balance<50000001 and Schm_code in() then Classification='Other Retail';
run;
data MIS_MJ2018_C;
set MIS_MJ2018_C;
if classification="" and Schm_code in() then Classification='AGRI';
run;
data MIS_MJ2018_C;
set MIS_MJ2018_C;
if FORACID=608609041000001 then Classification='Other Retail';
run;


data MIS_MJ2018_C;
format NPA_Class $20.;
format NPA_SubClass $20.;
set MIS_MJ2018_C;
if FORACID ne . then NPA_SubClass = NPA_Class;
if NPA_SubClass = "" then NPA_SubClass = "SA";
if NPA_Class ne . then NPA_Class = "NPA";
if NPA_Class = . then NPA_Class = "SA";
run;

Data ECLMJ.MIS_MJ2018_C; Set MIS_MJ2018_C; Run;


/*------------------- Sub Classification (LGD)----------------------------------*/
Data MIS_MJ2018_CS; Set ECLMJ.MIS_MJ2018_C; Run;



data MIS_MJ2018_CS;
set MIS_MJ2018_CS;
if class="BS-1A" then Basel_Classification="Expsoure On And Exposure Guaranteed by Central Govt. (CG)";
if class="BS-1B" then Basel_Classification="Exposure on State Govt.";
if class="BS-1C" then Basel_Classification="Exposure Guaranteed By Reserve Bank of India (RBI)";
if class="BS-1F" then Basel_Classification="Expsoure Guaranteed By State Govt.";
if Guarantor="Exposure Guaranteed By CGFTSI" then Basel_Classification="Exposure Guaranteed By CGFTSI";
if Guarantor="Exposure Guaranteed By ECGC" then Basel_Classification="Exposure Guaranteed By ECGC";
run;


Data MIS_MJ2018_CS;
format Stage $9.;
Set MIS_MJ2018_CS;
If (Classification = "Corporate and PSE") AND (Class_SMA = '') then Stage = "Stage1";
If (Classification = "Corporate and PSE") AND (Class_SMA = "SMA0") then Stage = "Stage1";
If (Classification = "Corporate and PSE") AND (Class_SMA = "SMA1") then Stage = "Stage1";
If (Classification = "Corporate and PSE") AND (Class_SMA = "SMA2") then Stage = "Stage2";
If (Classification = "MSME") AND (Class_SMA = '') then Stage = "Stage1";
If (Classification = "MSME") AND (Class_SMA = "SMA0") then Stage = "Stage1";
If (Classification = "MSME") AND (Class_SMA = "SMA1") then Stage = "Stage1";
If (Classification = "MSME") AND (Class_SMA = "SMA2") then Stage = "Stage2";
If ((Classification = "EDU") or (Classification = "AGRI") or (Classification = "HL") or (Classification = "PL")or (Classification = "AUTO")or (Classification = "Other Retail")) AND (Class_SMA = '') then Stage = "Stage1";
If ((Classification = "EDU") or (Classification = "AGRI") or (Classification = "HL") or (Classification = "PL")or (Classification = "AUTO")or (Classification = "Other Retail")) AND (Class_SMA = 'SMA0') then Stage = "Stage1";
If ((Classification = "EDU") or (Classification = "AGRI") or (Classification = "HL") or (Classification = "PL")or (Classification = "AUTO")or (Classification = "Other Retail")) AND (Class_SMA = 'SMA1') then Stage = "Stage2";
If ((Classification = "EDU") or (Classification = "AGRI") or (Classification = "HL") or (Classification = "PL")or (Classification = "AUTO")or (Classification = "Other Retail")) AND (Class_SMA = 'SMA2') then Stage = "Stage2";
Run;

Data MIS_MJ2018_CS;
Set MIS_MJ2018_CS;
If (Restructure = "Restructure") or (Class_NPA = "NPA") or (GROSS_NPA ne .) then Stage = "Stage3";
Run;



Data MIS_MJ2018_CS;
format Stage $15.;
Set MIS_MJ2018_CS;
if Stage in ('Stage1', 'Stage2') then Stage_Class = Stage;
if stage = "Stage3" and NPA_Class = "SA" then Stage_Class = "Stage3 - SA";
if stage = "Stage3" and NPA_SubClass = "SSA" then Stage_Class = "Stage3 - SSA";
if stage = "Stage3" and NPA_SubClass = "DB1" then Stage_Class = "Stage3 - DB1";
if stage = "Stage3" and NPA_SubClass = "DB2" then Stage_Class = "Stage3 - DB2";
if stage = "Stage3" and NPA_SubClass = "DB3" then Stage_Class = "Stage3 - DB3";
if stage = "Stage3" and NPA_SubClass = "DB4" then Stage_Class = "Stage3 - DB4";
if stage = "Stage3" and NPA_SubClass = "LOS" then Stage_Class = "Stage3 - LOS";
Run;

Data ECLMJ.MIS_MJ2018_CS; Set MIS_MJ2018_CS; Run;


/*------------------- Populate PD----------------------------------*/

Data MIS_MJ2018_CSPD; Set ECLMJ.MIS_MJ2018_CS; Run;



Data IRATING_MJ2018 (rename=Customer_ID=Cust_ID);
Set ECLMJ.IRATING_MJ2018(Keep=Customer_ID I2018);
Run;

%Sort_Data (Data_Name=IRATING_MJ2018, by=Cust_ID, Output=IRATING_MJ2018);
%Sort_Data (Data_Name=MIS_MJ2018_CSPD, by=Cust_ID, Output=MIS_MJ2018_CSPD);

Data MIS_MJ2018_CSPD;
Merge MIS_MJ2018_CSPD IRATING_MJ2018;
by Cust_ID;
Run;

Data MIS_MJ2018_CSPD;
Set MIS_MJ2018_CSPD;
if FORACID = . then delete;
Run;







Data MIS_MJ2018_CSPD;
format Final_IRating $3.;
Set MIS_MJ2018_CSPD;
IF I2018 = "VB10" then I2018 = "";
If I2018 ne "" then Final_IRating = I2018;
If (Classification = "Corporate and PSE") And Final_IRating = '' then Final_IRating = "Unr";
If (Classification = "MSME") And Final_IRating = '' then Final_IRating = "Unr";
Run;

Data MIS_MJ2018_CSPD;
Set MIS_MJ2018_CSPD;
If GROSS_NPA ne . then Final_IRating = "NPA";
If (Gross_NPA = .) and (Final_IRating = "NPA") then Final_IRating = "VB8";
Run;



Data Corp_PD (keep=Statement_Date Final_IRating PD_Corp_MSME rename=Statement_Date=StatementDate);
Set ECLMJ.CORPPD_MJ2018 (Rename= (Int_Rating=Final_IRating));
where Statement_Date in ('31MAR2018'd, '30JUN2018'd);
Run;

%Sort_Data (Data_Name=MIS_MJ2018_CSPD, by=StatementDate Final_IRating, Output=MIS_MJ2018_CSPD);
%Sort_Data (Data_Name=Corp_PD, by=StatementDate Final_IRating, Output=Corp_PD);

Data MIS_MJ2018_CSPD;
Merge MIS_MJ2018_CSPD Corp_PD;
by StatementDate Final_IRating;
Run;

Data RETAILPD_MJ2018 (keep= 'Statement Date'n Classification PD_Retail rename='Statement Date'n=Statementdate);
Set ECLMJ.RETAILPD_MJ2018 (Rename= Pool_Code=Classification);
where 'Statement Date'n in ('31MAR2018'd, '30JUN2018'd);
Run;

%Sort_Data (Data_Name=MIS_MJ2018_CSPD, by=StatementDate Classification, Output=MIS_MJ2018_CSPD);
%Sort_Data (Data_Name=RETAILPD_MJ2018, by=StatementDate Classification, Output=RETAILPD_MJ2018);

Data MIS_MJ2018_CSPD;
Merge MIS_MJ2018_CSPD RETAILPD_MJ2018;
by StatementDate Classification;
Run;

Data MIS_MJ2018_CSPD;
format Final_PD BESTX9.7;
Set MIS_MJ2018_CSPD;
If (Classification =  "Corporate and PSE") or (Classification =  "MSME") then Final_PD = PD_Corp_MSME;
If (Classification =  "EDU") or (Classification =  "HL") or (Classification =  "PL") or (Classification =  "AUTO") or (Classification =  "Other Retail") or (Classification =  "AGRI") then Final_PD = PD_Retail;
Run;

Data MIS_MJ2018_CSPD;
format NPA_SubClass_Final $20.;
SEt MIS_MJ2018_CSPD;
if NPA_SubClass_Final = NPA_SubClass;
Run;

Data MIS_MJ2018_CSPD;
SEt MIS_MJ2018_CSPD;
if NPA_SubClass_Final = "" then NPA_SubClass_Final = "SA";
Run;

Data MIS_MJ2018_CSPD;
SEt MIS_MJ2018_CSPD;
If NPA_SubClass = "SA" then NPA_Class = "SA";
Run;

Data MIS_MJ2018_CSPD;
SEt MIS_MJ2018_CSPD;
if NPA_SubClass ne "SA" then NPA_Class = "NPA";
Run;

Data MIS_MJ2018_CSPD;
Set MIS_MJ2018_CSPD;
if Classification in ("Corporate and PSE", "MSME") and Final_IRating in ("VB1", "VB2") then Final_PD = 0.0003;
Run;

Data MIS_MJ2018_CSPD;
SEt MIS_MJ2018_CSPD;
If NPA_Class = "NPA" or Gross_NPA ne . then Final_PD = 1;
If Stage = "Stage3" then Final_PD = 1;
Run;

Data MIS_MJ2018_CSPD;
set MIS_MJ2018_CSPD;
if Gross_NPA = . and NPA_SubClass ne "SA" then NPA_Class = "SA";
Run;


Data ECLMJ.MIS_MJ2018_CSPD; Set MIS_MJ2018_CSPD; Run;





/*------------------- Populate LGD----------------------------------*/

Data MIS_MJ2018_CSPDLGD; Set ECLMJ.MIS_MJ2018_CSPD; Run;

Data MIS_MJ2018_CSPDLGD;
format LGD BESTX9.7;
Set MIS_MJ2018_CSPDLGD;
LGD = 0.456253438297207;
Run;

Data MIS_MJ2018_CSPDLGD;
Set MIS_MJ2018_CSPDLGD;
if NPA_SubClass in ("LOS", "DB4") then LGD = 1;
Run;




Data ECLMJ.MIS_MJ2018_CSPDLGD; Set MIS_MJ2018_CSPDLGD; Run;


/*------------------- Define & Populate EAD----------------------------------*/

Data MIS_MJ2018_CSPDLGDEAD; Set ECLMJ.MIS_MJ2018_CSPDLGD; Run;

Data MIS_MJ2018_CSPDLGDEAD;
Set MIS_MJ2018_CSPDLGDEAD;
Original_CRM = CRM;
Original_CRM_Undrawn = CRM_Undrawn;
Run;

Data MIS_MJ2018_CSPDLGDEAD;
Set MIS_MJ2018_CSPDLGDEAD;
If NPA_Class = "NPA" then CRM = 0;
Balance = sum(BALANCE_GL, -PWO);
Run;


Data MIS_MJ2018_CSPDLGDEAD;
Set MIS_MJ2018_CSPDLGDEAD;
if CRM > Balance and Balance => 0 then CRM = Balance;
if CRM_Undrawn > (Total_Undrawn*0.20) and (Total_Undrawn*0.20) =>0 then CRM_Undrawn = (Total_Undrawn*0.20);
if CRM_Undrawn < 0 then CRM_Undrawn = 0;
if CRM < 0 then CRM = 0;
Run;


Data MIS_MJ2018_CSPDLGDEAD;
Set MIS_MJ2018_CSPDLGDEAD;
EAD_FB = sum(Balance, -CRM);
EAD_Undrawn = sum((Total_Undrawn*0.20), -CRM_Undrawn);
Run;

Data MIS_MJ2018_CSPDLGDEAD;
Set MIS_MJ2018_CSPDLGDEAD;
EAD = sum(EAD_FB, EAD_Undrawn);
Run;

Data MIS_MJ2018_CSPDLGDEAD;
Set MIS_MJ2018_CSPDLGDEAD;
If EAD_FB < 0 then EAD_FB = 0;
if EAD_Undrawn < 0 then EAD_Undrawn = 0;
if EAD < 0 then EAD = 0;
Run;

Data ECLMJ.MIS_MJ2018_CSPDLGDEAD; Set MIS_MJ2018_CSPDLGDEAD; Run;






/*------------------- Date Difference m----------------------------------*/

Data MIS_MJ2018_CSPDLGDEADM; Set ECLMJ.MIS_MJ2018_CSPDLGDEAD; Run;

Data MIS_MJ2018_CSPDLGDEADM;
Set MIS_MJ2018_CSPDLGDEADM;
Date_Diff_Dt = datdif(statementdate, lim_exp_date, 'act/act');
Date_diff_Yrs = yrdif(statementdate, lim_exp_date, 'act/act');
Date_Diff_Dt_Corrected = datdif(statementdate, lim_exp_date, 'act/act');
Date_diff_Yrs_Corrected = yrdif(statementdate, lim_exp_date, 'act/act');
run;


Proc sql;
create table SCHM_AvgRMaturity as
select StatementDate, SCHM_CODE, sum(Date_diff_Yrs_Corrected)/count(Date_diff_Yrs_Corrected) as SCHM_Avg_YM, sum(Date_Diff_Dt_Corrected)/count(Date_Diff_Dt_Corrected) as SCHM_Avg_DM
from MIS_MJ2018_CSPDLGDEADM
where Date_diff_Yrs_Corrected >0 and GROSS_NPA = .
Group by StatementDate, SCHM_CODE;
Quit;


%Sort_Data (Data_Name=MIS_MJ2018_CSPDLGDEADM, by=StatementDate SCHM_CODE, Output=MIS_MJ2018_CSPDLGDEADM);
%Sort_Data (Data_Name=SCHM_AvgRMaturity, by=StatementDate SCHM_CODE, Output=SCHM_AvgRMaturity);




Data MIS_MJ2018_CSPDLGDEADM;
Merge MIS_MJ2018_CSPDLGDEADM SCHM_AvgRMaturity;
By StatementDate SCHM_CODE;
Run;



Data MIS_MJ2018_CSPDLGDEADM;
Set MIS_MJ2018_CSPDLGDEADM;
If Date_diff_Yrs_Corrected < 0 and GROSS_NPA = . then Date_diff_Yrs_Corrected = SCHM_Avg_YM;
If Date_diff_Yrs_Corrected < 0 and GROSS_NPA = . then Date_Diff_Dt_Corrected = SCHM_Avg_DM;
Run;




/*Based on Behaviour study of demand loans i.e. 50% under bucket 6 months to 1 year*/
Data MIS_MJ2018_CSPDLGDEADM;
Set MIS_MJ2018_CSPDLGDEADM;
If GROSS_NPA = . and SCHM_CODE in() then Date_diff_Yrs_Corrected = 0.50;
If GROSS_NPA = . and SCHM_CODE in() then Date_Diff_Dt_Corrected = 182;
Run;


/*27 accounts Rs. 1.27 crs balance LIM_EXP_DATE 30DEC1899 changed to 1 year*/
Data MIS_MJ2018_CSPDLGDEADM;
Set MIS_MJ2018_CSPDLGDEADM;
If GROSS_NPA = . and Stage ne "Stage1" and Date_diff_Yrs_Corrected = . and Date_diff_Yrs < 0 then Date_diff_Yrs_Corrected = 1;
If GROSS_NPA = . and Stage ne "Stage1" and Date_diff_Yrs_Corrected = . and Date_diff_Yrs < 0 then Date_Diff_Dt_Corrected = 365;
Run;




Data ECLMJ.MIS_MJ2018_CSPDLGDEADM; Set  MIS_MJ2018_CSPDLGDEADM; Run;





/*------------------- Calculation of ECL Revised----------------------------------*/

/* LGD for Loss Assets to be made 100% and Weighted Average LGD to be extracted in report*/


Data MIS_MJ2018_CSPDLGDEADM_ECL; Set ECLMJ.MIS_MJ2018_CSPDLGDEADM; Run;




Data MIS_MJ2018_CSPDLGDEADM_ECL;
format ECL_FB BESTX12.2;
format ECL_Undrawn BESTX12.2;
format ECL BESTX12.2;
Set MIS_MJ2018_CSPDLGDEADM_ECL;
If (Classification = "Corporate and PSE") AND (Stage = "Stage1") then ECL_FB = EAD_FB*Final_PD*LGD;
If (Classification = "Corporate and PSE") AND (Stage = "Stage1") then ECL_Undrawn = EAD_Undrawn*Final_PD*LGD;

If (Classification = "Corporate and PSE") AND (Stage = "Stage2") then ECL_FB = EAD_FB*(1-(1-Final_PD)**(Date_diff_Yrs_Corrected))*LGD;
If (Classification = "Corporate and PSE") AND (Stage = "Stage2") then ECL_Undrawn = EAD_Undrawn*(1-(1-Final_PD)**(Date_diff_Yrs_Corrected))*LGD;

If (Classification = "Corporate and PSE") AND (Stage = "Stage3") then ECL_FB = EAD_FB*Final_PD*LGD;
If (Classification = "Corporate and PSE") AND (Stage = "Stage3") then ECL_Undrawn = EAD_Undrawn*Final_PD*LGD;

If (Classification = "MSME") AND (Stage = "Stage1") then ECL_FB = EAD_FB*Final_PD*LGD;
If (Classification = "MSME") AND (Stage = "Stage1") then ECL_Undrawn = EAD_Undrawn*Final_PD*LGD;

If (Classification = "MSME") AND (Stage = "Stage2") then ECL_FB = EAD_FB*(1-(1-Final_PD)**(Date_diff_Yrs_Corrected))*LGD;
If (Classification = "MSME") AND (Stage = "Stage2") then ECL_Undrawn = EAD_Undrawn*(1-(1-Final_PD)**(Date_diff_Yrs_Corrected))*LGD;

If (Classification = "MSME") AND (Stage = "Stage3") then ECL_FB = EAD_FB*Final_PD*LGD;
If (Classification = "MSME") AND (Stage = "Stage3") then ECL_Undrawn = EAD_Undrawn*Final_PD*LGD;

If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage1") then ECL_FB = EAD_FB*Final_PD*LGD;
If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage1") then ECL_Undrawn = EAD_Undrawn*Final_PD*LGD;

If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage2") then ECL_FB = EAD_FB*(1-(1-Final_PD)**(Date_diff_Yrs_Corrected))*LGD;
If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage2") then ECL_Undrawn = EAD_Undrawn*(1-(1-Final_PD)**(Date_diff_Yrs_Corrected))*LGD;

If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage3") then ECL_FB = EAD_FB*Final_PD*LGD;
If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage3") then ECL_Undrawn = EAD_Undrawn*Final_PD*LGD;




If (Classification = "Corporate and PSE") AND (Stage = "Stage1") then ECL = EAD*Final_PD*LGD;
If (Classification = "Corporate and PSE") AND (Stage = "Stage2") then ECL = EAD*(1-(1-Final_PD)**(Date_diff_Yrs_Corrected))*LGD;
If (Classification = "Corporate and PSE") AND (Stage = "Stage3") then ECL = EAD*Final_PD*LGD;
If (Classification = "MSME") AND (Stage = "Stage1") then ECL = EAD*Final_PD*LGD;
If (Classification = "MSME") AND (Stage = "Stage2") then ECL = EAD*(1-(1-Final_PD)**(Date_diff_Yrs_Corrected))*LGD;
If (Classification = "MSME") AND (Stage = "Stage3") then ECL = EAD*Final_PD*LGD;
If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL")or (Classification = "AUTO")or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage1") then ECL = EAD*Final_PD*LGD;
If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL")or (Classification = "AUTO")or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage2") then ECL = EAD*(1-(1-Final_PD)**(Date_diff_Yrs_Corrected))*LGD;
If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL")or (Classification = "AUTO")or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage3") then ECL = EAD*Final_PD*LGD;
Run;

Data MIS_MJ2018_CSPDLGDEADM_ECL;
Set MIS_MJ2018_CSPDLGDEADM_ECL;
If Classification = "Corporate and PSE" or Classification = "MSME" then Main_Classification = "Corporate/PSE/MSME";
If Classification = "AUTO" or Classification = "EDU" or Classification = "HL" or Classification = "Other Retail" or Classification = "PL"  or (Classification = "AGRI") then Main_Classification = "Retail";
Run;
/*
Data MIS_MJ2018_CSPDLGDEADM_ECL;
set MIS_MJ2018_CSPDLGDEADM_ECL;

If (Classification = "Corporate and PSE") AND (Stage = "Stage1") then ECL_FB_PD = EAD_FB*Final_PD;
If (Classification = "Corporate and PSE") AND (Stage = "Stage1") then ECL_FB_LGD = EAD_FB*LGD;
If (Classification = "Corporate and PSE") AND (Stage = "Stage1") then ECL_Undrawn_PD = EAD_Undrawn*Final_PD;
If (Classification = "Corporate and PSE") AND (Stage = "Stage1") then ECL_Undrawn_LGD = EAD_Undrawn*LGD;

If (Classification = "Corporate and PSE") AND (Stage = "Stage2") then ECL_FB_PD = EAD_FB*(1-(1-Final_PD)**(Date_diff_Yrs_Corrected));
If (Classification = "Corporate and PSE") AND (Stage = "Stage2") then ECL_FB_LGD = EAD_FB*LGD;
If (Classification = "Corporate and PSE") AND (Stage = "Stage2") then ECL_Undrawn_PD = EAD_Undrawn*(1-(1-Final_PD)**(Date_diff_Yrs_Corrected));
If (Classification = "Corporate and PSE") AND (Stage = "Stage2") then ECL_Undrawn_LGD = EAD_Undrawn*LGD;

If (Classification = "Corporate and PSE") AND (Stage = "Stage3") then ECL_FB_PD = EAD_FB*Final_PD;
If (Classification = "Corporate and PSE") AND (Stage = "Stage3") then ECL_FB_LGD = EAD_FB*LGD;
If (Classification = "Corporate and PSE") AND (Stage = "Stage3") then ECL_Undrawn_PD = EAD_Undrawn*Final_PD;
If (Classification = "Corporate and PSE") AND (Stage = "Stage3") then ECL_Undrawn_LGD = EAD_Undrawn*LGD;

If (Classification = "MSME") AND (Stage = "Stage1") then ECL_FB_PD = EAD_FB*Final_PD;
If (Classification = "MSME") AND (Stage = "Stage1") then ECL_FB_LGD = EAD_FB*LGD;
If (Classification = "MSME") AND (Stage = "Stage1") then ECL_Undrawn_PD = EAD_Undrawn*Final_PD;
If (Classification = "MSME") AND (Stage = "Stage1") then ECL_Undrawn_LGD = EAD_Undrawn*LGD;

If (Classification = "MSME") AND (Stage = "Stage2") then ECL_FB_PD = EAD_FB*(1-(1-Final_PD)**(Date_diff_Yrs_Corrected));
If (Classification = "MSME") AND (Stage = "Stage2") then ECL_FB_LGD = EAD_FB*LGD;
If (Classification = "MSME") AND (Stage = "Stage2") then ECL_Undrawn_PD = EAD_Undrawn*(1-(1-Final_PD)**(Date_diff_Yrs_Corrected));
If (Classification = "MSME") AND (Stage = "Stage2") then ECL_Undrawn_LGD = EAD_Undrawn*LGD;

If (Classification = "MSME") AND (Stage = "Stage3") then ECL_FB_PD = EAD_FB*Final_PD;
If (Classification = "MSME") AND (Stage = "Stage3") then ECL_FB_LGD = EAD_FB*LGD;
If (Classification = "MSME") AND (Stage = "Stage3") then ECL_Undrawn_PD = EAD_Undrawn*Final_PD;
If (Classification = "MSME") AND (Stage = "Stage3") then ECL_Undrawn_LGD = EAD_Undrawn*LGD;

If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage1") then ECL_FB_PD = EAD_FB*Final_PD;
If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage1") then ECL_FB_LGD = EAD_FB*LGD;
If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage1") then ECL_Undrawn_PD = EAD_Undrawn*Final_PD;
If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage1") then ECL_Undrawn_LGD = EAD_Undrawn*LGD;

If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage2") then ECL_FB_PD = EAD_FB*(1-(1-Final_PD)**(Date_diff_Yrs_Corrected));
If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage2") then ECL_FB_LGD = EAD_FB*LGD;
If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage2") then ECL_Undrawn_PD = EAD_Undrawn*(1-(1-Final_PD)**(Date_diff_Yrs_Corrected));
If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage2") then ECL_Undrawn_LGD = EAD_Undrawn*LGD;

If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage3") then ECL_FB_PD = EAD_FB*Final_PD;
If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage3") then ECL_FB_LGD = EAD_FB*LGD;
If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage3") then ECL_Undrawn_PD = EAD_FB*Final_PD;
If ((Classification = "EDU") or (Classification = "HL") or (Classification = "PL") or (Classification = "AUTO") or (Classification = "Other Retail") or (Classification = "AGRI")) AND (Stage = "Stage3") then ECL_Undrawn_LGD = EAD_FB*LGD;


Run;
*/


Data MIS_MJ2018_CSPDLGDEADM_ECL;
set MIS_MJ2018_CSPDLGDEADM_ECL;
if FORACID = 1 then NPA_Class = "SA";
if FORACID = 1 then NPA_SubClass = "SA";
if FORACID = 1 then Main_Classification = "Corporate/PSE/MSME";
if FORACID = 1 then classification = "Corporate and PSE";
if FORACID = 1 then Stage = "Stage1";
if FORACID = 1 then Stage_Class = "Stage1 - SA";
Run;

Data ECLMJ.MIS_MJ2018_CSPDLGDEADM_ECL_R; Set MIS_MJ2018_CSPDLGDEADM_ECL; Run;








/*------------------- Report 2 -NPA, PD & Stage Classification----------------------------------*/


TITLE;
TITLE1 "Summary Tables";
FOOTNOTE;
FOOTNOTE1 "Generated by the SAS System (&_SASSERVERNAME, &SYSSCPL) on %TRIM(%QSYSFUNC(DATE(), NLDATE20.)) at %TRIM(%SYSFUNC(TIME(), TIMEAMPM12.))";
FOOTNOTE2 "SAS Library Name 'ECLMJ' and final dataset name 'MISDATA_SEP17_ECL_FINAL' ";
/* -------------------------------------------------------------------
   Code generated by SAS Task

   Generated on: Tuesday, January 16, 2018 at 3:07:49 PM
   By task: Summary Tables

   Input Data: SASApp:ECLC.CBS_DATA_STD5
   Server:  SASApp
   ------------------------------------------------------------------- */

%_eg_conditional_dropds(WORK.STABSummaryTables);

/* -------------------------------------------------------------------
   Run the tabulate procedure
   ------------------------------------------------------------------- */
PROC TABULATE
DATA=ECLMJ.MIS_MJ2018_CSPDLGDEADM_ECL_R

	OUT=ECLMJ.Report2_R(LABEL="Summary Tables  for ECLMJ.MIS_MJ2018_CSPDLGDEADM_ECL")	;
	
	VAR BALANCE_GL BALANCE CRM EAD_FB EAD_Undrawn CRM_Undrawn EAD ECL_FB ECL_Undrawn Total_Undrawn ECL;
	CLASS StatementDate /	ORDER=UNFORMATTED MISSING;
	CLASS NPA_Class /	ORDER=UNFORMATTED MISSING;
	CLASS Main_Classification /	ORDER=UNFORMATTED MISSING;
	CLASS classification /	ORDER=UNFORMATTED MISSING;
	CLASS Stage /	ORDER=UNFORMATTED MISSING;
	TABLE 
		/* ROW Statement */
		NPA_Class *Main_Classification *classification *Stage 
		all = 'Total'  ,
		/* COLUMN Statement */
		StatementDate  *(BALANCE_GL * Sum={LABEL="Sum"} BALANCE * Sum={LABEL="Sum"} CRM * Sum={LABEL="Sum"} EAD_FB * Sum={LABEL="Sum"} ECL_FB * Sum={LABEL="Sum"} Total_Undrawn * Sum={LABEL="Sum"} CRM_Undrawn * Sum={LABEL="Sum"} EAD_Undrawn * Sum={LABEL="Sum"} ECL_Undrawn * Sum={LABEL="Sum"} EAD * Sum={LABEL="Sum"} ECL * Sum={LABEL="Sum"}) ;
	;

RUN;
/* -------------------------------------------------------------------
   End of task code
   ------------------------------------------------------------------- */
RUN; QUIT;
TITLE; FOOTNOTE;




/*------------------- Report 3 -NPA, PD & Stage Classification NPA Asset Class----------------------------------*/


TITLE;
TITLE1 "Summary Tables";
FOOTNOTE;
FOOTNOTE1 "Generated by the SAS System (&_SASSERVERNAME, &SYSSCPL) on %TRIM(%QSYSFUNC(DATE(), NLDATE20.)) at %TRIM(%SYSFUNC(TIME(), TIMEAMPM12.))";
FOOTNOTE2 "SAS Library Name 'ECLMJ' and final dataset name 'MISDATA_SEP17_ECL_FINAL' ";
/* -------------------------------------------------------------------
   Code generated by SAS Task

   Generated on: Tuesday, January 16, 2018 at 3:07:49 PM
   By task: Summary Tables

   Input Data: SASApp:ECLC.CBS_DATA_STD5
   Server:  SASApp
   ------------------------------------------------------------------- */

%_eg_conditional_dropds(WORK.STABSummaryTables);

/* -------------------------------------------------------------------
   Run the tabulate procedure
   ------------------------------------------------------------------- */
PROC TABULATE
DATA=ECLMJ.MIS_MJ2018_CSPDLGDEADM_ECL_R

	OUT=ECLMJ.Report3_R(LABEL="Summary Tables  for ECLMJ.MIS_MJ2018_CSPDLGDEADM_ECL")	;
	
	VAR BALANCE_GL BALANCE CRM EAD_FB EAD_Undrawn CRM_Undrawn EAD ECL_FB ECL_Undrawn Total_Undrawn ECL;
	CLASS StatementDate /	ORDER=UNFORMATTED MISSING;
	CLASS NPA_Class /	ORDER=UNFORMATTED MISSING;
	CLASS NPA_SubClass /	ORDER=UNFORMATTED MISSING;
	CLASS Stage /	ORDER=UNFORMATTED MISSING;
	TABLE 
		/* ROW Statement */
		NPA_Class *NPA_SubClass *Stage 
		all = 'Total'  ,
		/* COLUMN Statement */
		StatementDate  *(BALANCE_GL * Sum={LABEL="Sum"} BALANCE * Sum={LABEL="Sum"} CRM * Sum={LABEL="Sum"} EAD_FB * Sum={LABEL="Sum"} ECL_FB * Sum={LABEL="Sum"} Total_Undrawn * Sum={LABEL="Sum"} CRM_Undrawn * Sum={LABEL="Sum"} EAD_Undrawn * Sum={LABEL="Sum"} ECL_Undrawn * Sum={LABEL="Sum"} EAD * Sum={LABEL="Sum"} ECL * Sum={LABEL="Sum"}) ;
	;

RUN;
/* -------------------------------------------------------------------
   End of task code
   ------------------------------------------------------------------- */
RUN; QUIT;
TITLE; FOOTNOTE;





/*------------------- Report 4 -NPA, PD & Stage Classification NPA Asset Class PRov----------------------------------*/


Data NPA_MJ2018 (keep=StatementDate ACID_NUM Secured_Amt);
Set ECLMJ.NPA_MJ2018 (rename=(ACID=ACID_NUM Statement_Date=StatementDate));
Run;

%Sort_Data (Data_Name=ECLMJ.MIS_MJ2018_CSPDLGDEADM_ECL_R, by=StatementDate ACID_NUM, Output=MIS_MJ2018_CSPDLGDEADM_ECL_R);
%Sort_Data (Data_Name=NPA_MJ2018, by=StatementDate ACID_NUM, Output=NPA_MJ2018);
Data MIS_MJ2018_CSPDLGDEADM_ECL_R;
Merge MIS_MJ2018_CSPDLGDEADM_ECL_R NPA_MJ2018;
by StatementDate ACID_NUM;
Run;

Data MIS_MJ2018_CSPDLGDEADM_ECL_R;
set MIS_MJ2018_CSPDLGDEADM_ECL_R;
if Gross_NPA = . then Secured_Amt = Balance;
if SCHM_CODE in ("CC633",	"PL710",	"PL711",	"PL712",	"PL713",	"PL713",	"PL714",	"PL715",	"PL716",	"PL717",	"PL717",	"PL718",	"PL719",	"PL720",	"PL721",	"PL722",	"PL723",	"PL724",	"PL724",	"PL725",	"PL726",	"PL815",	"PL816",	"PL817",	"PL825",	"PL826",	"PL827",	"PL828",	"VG693") then Secured_Amt = 0;
Run;

Data MIS_MJ2018_CSPDLGDEADM_ECL_R;
set MIS_MJ2018_CSPDLGDEADM_ECL_R;
if Gross_NPA = . and classification = "MSME" then S_Prov = 0.0025;
if Gross_NPA = . and classification = "AGRI" then S_Prov = 0.0025;
if Gross_NPA = . and classification = "HL" then S_Prov = 0.0025;
if Gross_NPA = . and Restructure ne "" then S_Prov = 0.0286;
if ACID_NUM = 500706210002000 then S_Prov = 0.15;
if ACID_NUM = 515507131000001 then S_Prov = 0.05;
Run;

Data MIS_MJ2018_CSPDLGDEADM_ECL_R;
set MIS_MJ2018_CSPDLGDEADM_ECL_R;
if S_Prov = . then S_Prov = 0.0040;
Run;

Data MIS_MJ2018_CSPDLGDEADM_ECL_R;
set MIS_MJ2018_CSPDLGDEADM_ECL_R;
if Gross_Prov = . then Gross_Prov = Balance * S_Prov;
Run;
Data ECLMJ.MIS_MJ2018_CSPDLGDEADME_SECPROV;
set MIS_MJ2018_CSPDLGDEADM_ECL_R;
Run;



TITLE;
TITLE1 "Summary Tables";
FOOTNOTE;
FOOTNOTE1 "Generated by the SAS System (&_SASSERVERNAME, &SYSSCPL) on %TRIM(%QSYSFUNC(DATE(), NLDATE20.)) at %TRIM(%SYSFUNC(TIME(), TIMEAMPM12.))";
FOOTNOTE2 "SAS Library Name 'ECLMJ' and final dataset name 'MISDATA_SEP17_ECL_FINAL' ";
/* -------------------------------------------------------------------
   Code generated by SAS Task

   Generated on: Tuesday, January 16, 2018 at 3:07:49 PM
   By task: Summary Tables

   Input Data: SASApp:ECLC.CBS_DATA_STD5
   Server:  SASApp
   ------------------------------------------------------------------- */

%_eg_conditional_dropds(WORK.STABSummaryTables);

/* -------------------------------------------------------------------
   Run the tabulate procedure
   ------------------------------------------------------------------- */
PROC TABULATE
DATA=ECLMJ.MIS_MJ2018_CSPDLGDEADME_SECPROV

	OUT=ECLMJ.Report4_R(LABEL="Summary Tables  for ECLMJ.MIS_MJ2018_CSPDLGDEADM_ECL")	;
	 
	VAR BALANCE_GL BALANCE Gross_NPA Gross_Prov Secured_Amt ;
	CLASS StatementDate /	ORDER=UNFORMATTED MISSING;
	CLASS NPA_Class /	ORDER=UNFORMATTED MISSING;
	CLASS NPA_SubClass /	ORDER=UNFORMATTED MISSING;
	CLASS Stage /	ORDER=UNFORMATTED MISSING;
	TABLE 
		/* ROW Statement */
		NPA_Class *NPA_SubClass *Stage 
		all = 'Total'  ,
		/* COLUMN Statement */
		StatementDate  *(BALANCE_GL * Sum={LABEL="Sum"} BALANCE * Sum={LABEL="Sum"} Secured_Amt * Sum={LABEL="Sum"} Gross_NPA * Sum={LABEL="Sum"} Gross_Prov * Sum={LABEL="Sum"}) ;
	;

RUN;
/* -------------------------------------------------------------------
   End of task code
   ------------------------------------------------------------------- */
RUN; QUIT;
TITLE; FOOTNOTE;















/*------------------- Report - PD & Stage Classification----------------------------------*/


TITLE;
TITLE1 "Summary Tables";
FOOTNOTE;
FOOTNOTE1 "Generated by the SAS System (&_SASSERVERNAME, &SYSSCPL) on %TRIM(%QSYSFUNC(DATE(), NLDATE20.)) at %TRIM(%SYSFUNC(TIME(), TIMEAMPM12.))";
FOOTNOTE2 "SAS Library Name 'ECLMJ' and final dataset name 'MISDATA_SEP17_ECL_FINAL' ";
/* -------------------------------------------------------------------
   Code generated by SAS Task

   Generated on: Tuesday, January 16, 2018 at 3:07:49 PM
   By task: Summary Tables

   Input Data: SASApp:ECLC.CBS_DATA_STD5
   Server:  SASApp
   ------------------------------------------------------------------- */

%_eg_conditional_dropds(WORK.STABSummaryTables);

/* -------------------------------------------------------------------
   Run the tabulate procedure
   ------------------------------------------------------------------- */
PROC TABULATE
DATA=ECLMJ.MIS_MJ2018_CSPDLGDEADM_ECL

	OUT=WORK.STABSummaryTables(LABEL="Summary Tables  for ECLMJ.MIS_MJ2018_CSPDLGDEADM_ECL")
	
	;
	
	VAR BALANCE_GL BALANCE CRM EAD_FB EAD_Undrawn CRM_Undrawn EAD ECL_FB ECL_Undrawn Total_Undrawn ECL;
	CLASS StatementDate /	ORDER=UNFORMATTED MISSING;
	CLASS Main_Classification /	ORDER=UNFORMATTED MISSING;
	CLASS classification /	ORDER=UNFORMATTED MISSING;
	CLASS Stage /	ORDER=UNFORMATTED MISSING;
	TABLE 
		/* ROW Statement */
		Main_Classification *classification *Stage 
		all = 'Total'  ,
		/* COLUMN Statement */
		StatementDate  *(BALANCE_GL * Sum={LABEL="Sum"} BALANCE * Sum={LABEL="Sum"} CRM * Sum={LABEL="Sum"} EAD_FB * Sum={LABEL="Sum"} ECL_FB * Sum={LABEL="Sum"} Total_Undrawn * Sum={LABEL="Sum"} CRM_Undrawn * Sum={LABEL="Sum"} EAD_Undrawn * Sum={LABEL="Sum"} ECL_Undrawn * Sum={LABEL="Sum"} EAD * Sum={LABEL="Sum"} ECL * Sum={LABEL="Sum"} ) 		;
	;

RUN;
/* -------------------------------------------------------------------
   End of task code
   ------------------------------------------------------------------- */
RUN; QUIT;
TITLE; FOOTNOTE;




