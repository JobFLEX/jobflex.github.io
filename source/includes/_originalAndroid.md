# Legacy Android API

<aside class="notice">These methods are no longer working by the time of this deployment. This is for past reference only, please do not use anything here.</aside>


## Sign Up  

This method used to sign users up from the app.

### HTTP REQUEST
`POST cloudmanagerpro.com/AndroidGP/signup.php?year=johntestestemail%40jobflex.com%7CJF%7Cjobflex12345%7CJF%7CDrywall%7CJF%7C%7CJF%7CNocompany%7CJF%7C%7CJF%7C&SID=unknown`



### Query Parameters

Parameter | Description
--------- | -----------
year | This being exploded into the parameters below
ContEmail | Contractor/User email address.
Password | Contractor/User password.
ContBusName | Name of the business.
Referral | Refferal Source.
SID | Device serial number.
CLIncre | Industry number check note below.


### Inserted Into Table

> Example legacy import

``` php
$sql = "select * from ItemAccountPackages where Packages_ID = '9'";
	$query = mysql_query($sql);
	$packagerows = mysql_fetch_assoc($query);

	$sqlcont = "insert into ContractorInfo (NextQuoteNum, NextJobNum,
	EntityMaster, Sched, ContBusNam, ContContactFirst, ContContactLast, ServiceRadius,
	ContMobile, ContEmail,  ContDateCreated, OrgCreateDate, ContDateChanged, CreatedBy,
	Active, IsTrial, MaxTabletUse, MaxFileSizeUse, MaxMemoryUse, BaseMemoryUse,
	CLIncre,Title,AccountType, OrgAccountType,Referral)
	values ('1','1','1', '1','".htmlentities($_POST['ContBusName'],ENT_QUOTES)."','"
	.htmlentities($_POST['ContContactFirst'],ENT_QUOTES)."', '"
	.htmlentities($_POST['ContContactLast'],ENT_QUOTES)."', '25', '555-555-5555', '"
	.str_replace("'","\'",$_POST['ContEmail'])."', '"
	.date('Y-m-d H:i:s')."', '".date('Y-m-d H:i:s')."', '"
	.date('Y-m-d')."', '"
	.htmlentities($_POST['ContContactFirst'],ENT_QUOTES)." "
	.htmlentities($_POST['ContContactLast'],ENT_QUOTES)."',
	 '1', '0', '0', '150000','"
	. ($packagerows['Starting_Memory'] * 1073741824) ."', '". ($packagerows['Starting_Memory'] * 1073741824)
	."', '".$_POST['CLIncre']."','" . htmlentities($_REQUEST['title'],ENT_QUOTES) . "','9','9','"
	. htmlentities($_POST['Referral'],ENT_QUOTES) . "')";
```


``` table: ContractorInfo  ```

Field    | Value 
-------- | ------------
NextQuoteNum | defaults to 1
NextJobNum | defaults to 1
EntityMaster | defaults to 1
Sched | defaults to 1
ContBusNam | $ContBusName
ContContactFirst | $ContContactFirst
ContContactLast | $ContContactLast
ServiceRadius | defaulting to 25.
ContMobile | defaulting to 555-555-5555
ContEmail | ContEmail
ContDateCreated | date('Y-m-d H:i:s')
OrgCreateDate | date('Y-m-d H:i:s')
ContDateChanged | date('Y-m-d')
CreatedBy | $ContContactFirst + $ContContactLast
Active | 1
IsTrial | 0
MaxTabletUse | 0
MaxFileSizeUse | 150000
MaxMemoryUse | $packagerows['Starting_Memory'] * 1073741824
BaseMemoryUse | $packagerows['Starting_Memory'] * 1073741824
CLIncre | $CLIncre
Title | $title
AccountType | 9
OrgAccountType | 9
Referral | $Referral


After doing the insert into Contractor Info it then inserts into User


` Table Users `

Field       | Value
------------| ----------
EntityMaster | 1
MasterTable | ContractorInfo
MasterID | $id
Rights | 5
Function | 'Admin'
ServiceRadius | 45
FirstName | $ContContactFirst
LastName | $ContContactLast
Phone | $str (555-555-5555)
Email | $ContEmail
Md5Pass | $Password
4CharPass | $Password-4
Active | 1 
DateCreated | date('Y-m-d H:i:s')
IsTrial | $IsTrial
MainUser | 1
PrimaryUser | 1
IsSalesPerson | 2
ControlPanelAccess | 1
ManageUsersAccess | 1 
CompanySetupAccess | 1 
TermsCondAccess | 1
SchedulingAccess | 1
CustomerMgtAccess | 1
FileDirectoryAccess | 1
ImpExpAccess | 1
MaterialsListAccess | 1
OverRideAccess | 1 
ServiceHeaderAccess | 1
TaxesAccess | 1
ReportsAccess | 1
FinancingAccess | 1
PhotoDescAccess | 1
EmailVerif | 1



And then it inserts into this next table

` ContractorTabletSetup `


Field       | Value
------------| --------
IntContNum | $id
ContBusName | $ContBusName
ContPhone | $str 
ContEmail | $ContEmail
UserID | $userid
LogoBlob | /Uploads/logo-placeholder-01.png


<aside class="notice"> 
It also insert's these other sample data
</aside>

```

$sqpc = "insert into ProgramCal (IntContNum, Utility_Service_Description) values
				('".$id."', 'Sample Roofing Heading'),
				('".$id."', 'Sample Flooring Heading'),
				('".$id."', 'Sample Landscaping Heading')";
	$qupc = mysql_query($sqpc) or die(mysql_error());

	$sqpc = "insert into ContractorPhotoDesc (IntContNum, Title, Description) values
				('".$id."', 'Sample - Condition','This is the existing condition of your ______'),
				('".$id."', 'Sample - Options','These are the available options to choose from')";
	$qupc = mysql_query($sqpc) or die(mysql_error());


	$sqctc = "Insert into  ContractorTermsConditions (IntContNum, TermsNote, TermCode) values ('".$id."', (select `Value` from DefaultFields where `Name` = 'Terms'), 'QuotationTerms')";
	$quctc = mysql_query($sqctc) or die(mysql_error());

	// sales/service/audit inspection types
	$sql = "insert into LeadServicesCat (ServicesCat,IntContNum,CustType,InspectionType,InspectionDesc,NewCat) values
		('Service','" . $id . "',1,1,'Service',1),
		('Sales','" . $id . "',0,0,'Sales',1),
		('Audit','" . $id . "',2,2,'Audit',1)";
	mysql_query($sql);
    
```

Finally The response output to android app is as follows.

```
// Successful
$output = "1Registration Successful! Sign in using your new account to finish the rest of the setup.";

// Failure
$output = "0Email address is already registered.";
    
```


## Sign In


``` URL https://testsite.cloudmanagerpro.com/AndroidGP/downloadContractorInfo.php ```

> Data Sent

``` 
year=jimbob%40bobbbbbbbburgerss.com%7CJF%7Cdbz95uov&SID=unknown&REGID=APA91bG31Q6yg7eknY6dBb2XY4PWS-0aueFz0kfoxCphcp5NYAsc_z0PNWUpiwbz-fE01mOMMksLD0rmMMNu8EtFt1pzr-dUqOHfxH-LAyPclYEmUvHAR3miE5T3s2i_EGKhkQ30-Sai
```


> Data Received

``` 
[
  {
    "IntContNum": "988",
    "EntityMaster": "0",
    "ContractorStatus": "",
    "ContBusNam": "Jim",
    "DBAName": "",
    "ContAddr": "3444 Anderson Road",
    "ContAddrTwo": "",
    "ContCity": "Somewhere",
    "ContState": "MA",
    "ContZip": "55555",
    "ContPhone": "555-555-5555",
    "ContMobile": "",
    "ContEmail": "",
    "ContFax": "555-222-2222",
    "WebSite": "www.website.com",
    "lat": "",
    "lng": "",
    "CorpLogo": "png",
    "LogoBlob": "iVBORw0KGgoAAAANSUhEUgAAAfQAAAH0CAIAAABEtEjdAAAACXBIWXMAAAsTAAALEwEAmpwYAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAKUVJREFUeNrsnQl320bWYEkCBBeQlGgpduzE7Rz3lzPz/3/PTKbdTpzYkURxATeQ1DwRabUiiSR21HJvp92K25bIKuDy4dWrV/W7u7saAACYRYMhAABA7gAAgNwBAAC5AwBALrgMAWjB9p7dX1/sttFvrtfhwx+42+3CzSbjT2m6br3x34jH85rRF07DEe6/2H/FdID61KmWAdUM/vBLZPP1eq3gS/U878H10S94H5A7wF8eX4fru92dRNyRyw14X5Hm908Ada/pYXxA7mCFyjfhvcez50/0QlzvNl2xPLoH5A7a23y9Dre7+183YbjjMntEo153m03Pa95L33VxPSB3UBe5kNb3It9g8yyu9zyvXq8zJoDcofrwPEq22JZpKY4oh+OJ7b0mQT0gdyhb6PdZFyOWQFXmPk0vmkf0gNyhCKKUi9h8uVoh9ApF3261vL3pSd0Acof0bO4T6Ovlaq1mpbnNiN/b4nnPc132IQJyh3isViuCdL3C+dY+omc0ALnDy05f7v9LoYuONOr1VrsViZ7RAOQOOB3LA3IHnA5YHpA7KMhms1kslovFAqdbZfnOPW1WX5E7mMZ2u5UwPZgvWCO1Gcdx/G5HAnlK5pE7aI84XUL15WrFUMAD7VZLAnnSNcgdtAzVRemidUJ1OBLIi+I77TaBPHIHQnUgkAfkDiUi87hYLMiqQ.....",
    "DateCreated": "2016-04-19 15:35:59",
    "ProgramID": "0",
    "UserID": "988",
    "CompanyMoto": "",
    "UseDBA": "0",
    "DTELogo": "",
    "ContContactFirst": "",
    "ContContactLast": "",
    "Terms": "",
    "Footer": "",
    "ContDateCreated": "2016-04-19 15:35:59",
    "IsTrial": "0",
    "FinancingTerms": "0",
    "FinancingRate": "0.000",
    "AccountType": "9",
    "LatestVersion": "4.0.2",
    "FooterLogo": "",
    "FooterExt": "",
    "TrialExpires": "20160521"
  }
]

```


## SyncRights

` URL https://testsite.cloudmanagerpro.com/AndroidGP/syncRights.php `

> Data Sent

```
?year=jimbob%40bobbbbbbbburgerss.com%7CJF%7Cdbz95uov&SID=unknown&CustCount=1&CustIncrement=19&VersionNum=8.0.4-debug
```

> Data Received


```
[
  {
    "Title": "Sample - Condition",
    "Description": "This is the existing condition of your ______",
    "AutoIncre": "657"
  },
  {
    "Title": "Sample - Options",
    "Description": "These are the available options to choose from",
    "AutoIncre": "658"
  }
]
```


## Sync Schedule

` URL: https://testsite.cloudmanagerpro.com/AndroidGP/syncSchedule.php `

> Data Sent

``` 
year=jimbob%40bobbbbbbbburgerss.com%7CJF%7Cdbz95uov&SID=unknown 
```

> Data Recieved

```
None yet
```


## Download Headings

` URL: https://testsite.cloudmanagerpro.com/AndroidGP/downloadHeadings.php `

> Data Sent

```
?year=jimbob%40bobbbbbbbburgerss.com%7CJF%7Cdbz95uov&SID=unknown&date=0&sync=1
```

> Data Recieved

```
[
  {
    "Utility_Service_Detail_ID": "3469",
    "IntContNum": "1728",
    "Utility_Service_ID": "0",
    "ProgramID": "0",
    "Utility_Service_Description": "Sample Roofing Heading",
    "Utility_ListArray": "",
    "Utility_U_Value": "0.0000",
    "Utility_AreaCalcUnits": "",
    "Utility_PreValues": "",
    "Utility_PostValue": "",
    "Utility_Days_Use_Avg": "0",
    "Utility_Correction_Factor": "0.00",
    "Utility_MBTU": "0",
    "Utility_Life": "0",
    "PlaceOnSow": "",
    "Type": "0",
    "LastUpdated": "2016-04-19 15:35:59"
  },
  {
    "Utility_Service_Detail_ID": "3470",
    "IntContNum": "1728",
    "Utility_Service_ID": "0",
    "ProgramID": "0",
    "Utility_Service_Description": "Sample Flooring Heading",
    "Utility_ListArray": "",
    "Utility_U_Value": "0.0000",
    "Utility_AreaCalcUnits": "",
    "Utility_PreValues": "",
    "Utility_PostValue": "",
    "Utility_Days_Use_Avg": "0",
    "Utility_Correction_Factor": "0.00",
    "Utility_MBTU": "0",
    "Utility_Life": "0",
    "PlaceOnSow": "",
    "Type": "0",
    "LastUpdated": "2016-04-19 15:35:59"
  },
  {
    "Utility_Service_Detail_ID": "3471",
    "IntContNum": "1728",
    "Utility_Service_ID": "0",
    "ProgramID": "0",
    "Utility_Service_Description": "Sample Landscaping Heading",
    "Utility_ListArray": "",
    "Utility_U_Value": "0.0000",
    "Utility_AreaCalcUnits": "",
    "Utility_PreValues": "",
    "Utility_PostValue": "",
    "Utility_Days_Use_Avg": "0",
    "Utility_Correction_Factor": "0.00",
    "Utility_MBTU": "0",
    "Utility_Life": "0",
    "PlaceOnSow": "",
    "Type": "0",
    "LastUpdated": "2016-04-19 15:35:59"
  }
]

```

## Download Terms

` URL: https://testsite.cloudmanagerpro.com/AndroidGP/downloadTerms.php `

> Data Sent

```
?year=jimbob%40bobbbbbbbburgerss.com%7CJF%7Cdbz95uov&SID=unknown
```

> Data Recieved

```
[
  {
    "TermsID": "535",
    "EntityMaster": "0",
    "IntContNum": "1728",
    "ContBusNam": "",
    "DBAName": "",
    "DateCreated": "2016-04-19 15:35:59",
    "ProgramID": "0",
    "UserID": "0",
    "UseDBA": "0",
    "TermsNote": "<font face=\"arial\" size=\"2\"><b><font size=\"3\"></font></b></font>\n\n<p class=\"MsoNormal\" style=\"margin-top:3.4pt;margin-right:-1.0pt;margin-bottom:\n0in;margin-left:5.1pt;margin-bottom:.0001pt;line-height:normal\"><b style=\"mso-bidi-font-weight:normal\">Terms and Conditions </b></p>\n\n<p class=\"MsoNormal\" style=\"margin-top:.95pt;margin-right:0in;margin-bottom:0in;\nmargin-left:0in;margin-bottom:.0001pt;line-height:10.0pt;mso-line-height-rule:\nexactly\"><a name=\"_GoBack\"></a>&nbsp;</p>\n\n<p class=\"MsoNormal\" style=\"margin-top:0in;margin-right:2.95pt;margin-bottom:\n0in;margin-left:5.1pt;margin-bottom:.0001pt;line-height:\n100%\"><b style=\"mso-bidi-font-weight:normal\">Scope of Work:</b> &nbsp;Company will provide to Homeowner the services as described in the attached\nproposal which is hereby incorporated into this Contract\nas reference. &nbsp;Company will provide all services, materials, labor, tools, and equipment\nneeded for completion of services in the\nquote at the previously listed address. All services will be in\ncompliance with local codes and company agrees to provide\nprofessional quality services\naccording to acceptable industry\nstandards and practices.</p>\n\n<p class=\"MsoNormal\" style=\"margin-top:.05pt;margin-right:0in;margin-bottom:0in;\nmargin-left:0in;margin-bottom:.0001pt;line-height:11.0pt;mso-line-height-rule:\nexactly\">&nbsp;</p>\n\n<p class=\"MsoNormal\" style=\"margin-top:0in;margin-right:3.3pt;margin-bottom:0in;\nmargin-left:5.1pt;margin-bottom:.0001pt;text-indent:.25pt;line-height:100%\"><b style=\"mso-bidi-font-weight:normal\">Payment Terms:</b> &nbsp;Receipt of a 50% down payment\nis due upon signing of quote acceptance and prior to final\nscheduling for project initiation. The balance of the contract\nis due the day of final contract\ncompletion. &nbsp;If for any reason, the project is delayed beyond the control\nof the company due to unforeseen issues\nwith the home, natural disasters, or homeowner circumstances the homeowner agrees to pay\ncompany for the work completed until a new schedule is agreed upon by\nboth parties.</p>\n\n<p class=\"MsoNormal\" style=\"margin-top:.1pt;margin-right:0in;margin-bottom:0in;\nmargin-left:0in;margin-bottom:.0001pt;line-height:11.0pt;mso-line-height-rule:\nexactly\">&nbsp;</p>\n\n<p class=\"MsoNormal\" style=\"margin-top:0in;margin-right:16.7pt;margin-bottom:\n0in;margin-left:5.35pt;margin-bottom:.0001pt;line-height:100%\"><b style=\"mso-bidi-font-weight:normal\">Change Order:</b> &nbsp;Any deviation from the\nabove Scope of Work involving a change in the scope of work or any additional costs will be executed only with\na written change order signed and dated by\nboth the Company and\nhomeowner.</p>\n\n<p class=\"MsoNormal\" style=\"margin-top:.05pt;margin-right:0in;margin-bottom:0in;\nmargin-left:0in;margin-bottom:.0001pt;line-height:11.0pt;mso-line-height-rule:\nexactly\">&nbsp;</p>\n\n<p class=\"MsoNormal\" style=\"margin-top:0in;margin-right:-1.0pt;margin-bottom:\n0in;margin-left:5.35pt;margin-bottom:.0001pt;line-height:normal\"><b style=\"mso-bidi-font-weight:normal\">Other Terms:</b> &nbsp;Homeowners who elect to use a financing\noption are still 100% responsible for payments to company. Payment is the responsibility of the homeowner and is due upon work completion.</p>\n\n<p class=\"MsoNormal\" style=\"margin-top:.15pt;margin-right:0in;margin-bottom:0in;\nmargin-left:0in;margin-bottom:.0001pt;line-height:11.0pt;mso-line-height-rule:\nexactly\">&nbsp;</p>\n\n<p class=\"MsoNormal\" style=\"margin-top:0in;margin-right:5.65pt;margin-bottom:\n0in;margin-left:5.35pt;margin-bottom:.0001pt;text-indent:-.25pt;line-height:\n100%\"><b style=\"mso-bidi-font-weight:normal\">Warranty:</b> Company warrants all work will be performed in a good and workmanlike manner. All materials\nused in the project\nwill be new and of good\nquality; and all work will be\ncompleted as defined in scope of work outlined\nin the proposal acceptance. Any warranties for parts\nor materials are subject to specific manufacturer terms on such\nproducts.</p>\n\n<p class=\"MsoNormal\" style=\"margin-top:.05pt;margin-right:0in;margin-bottom:0in;\nmargin-left:0in;margin-bottom:.0001pt;line-height:11.0pt;mso-line-height-rule:\nexactly\">&nbsp;</p>\n\n<p class=\"MsoNormal\" style=\"margin-top:0in;margin-right:39.3pt;margin-bottom:\n0in;margin-left:5.6pt;margin-bottom:.0001pt;line-height:100%\"><b style=\"mso-bidi-font-weight:normal\">Conditions:</b> This proposal is valid for 30 days.\nCompany reserves the right to withdraw this proposal or re-quote the project if contract acceptance is beyond 30 days.</p>\n\n<p class=\"MsoNormal\" style=\"margin-top:.2pt;margin-right:0in;margin-bottom:0in;\nmargin-left:0in;margin-bottom:.0001pt;line-height:13.0pt;mso-line-height-rule:\nexactly\">&nbsp;</p>\n\n<p class=\"MsoNormal\" style=\"margin-top:0in;margin-right:-1.0pt;margin-bottom:\n0in;margin-left:5.1pt;margin-bottom:.0001pt;line-height:normal\">Authorized Signature: __________________________________________</p>\n\n<p class=\"MsoNormal\" style=\"margin-top:.5pt;margin-right:0in;margin-bottom:0in;\nmargin-left:0in;margin-bottom:.0001pt;line-height:8.5pt;mso-line-height-rule:\nexactly\">&nbsp;</p>\n\n<p class=\"MsoNormal\" style=\"margin-top:0in;margin-right:.45in;margin-bottom:0in;\nmargin-left:5.6pt;margin-bottom:.0001pt;text-indent:-.25pt;line-height:100%\">The above prices, specifications, and conditions are satisfactory and are hereby accepted. Company\nis authorized to do the work as specified. A 50% deposit\nis required at time\nof signing and balance is due at\ncompletion of job. The signature on this contract is evidence of Homeowner's acceptance of all terms and conditions within.<br /></p>\n\n<p class=\"MsoNormal\" style=\"margin-bottom:0in;margin-bottom:.0001pt;line-height:\n10.0pt;mso-line-height-rule:exactly\">Acceptance Date:<br />\n</p><p class=\"MsoNormal\" style=\"margin-bottom:0in;margin-bottom:.0001pt;line-height:\n10.0pt;mso-line-height-rule:exactly\">________________________________<br />\n</p><p class=\"MsoNormal\" style=\"margin-bottom:0in;margin-bottom:.0001pt;line-height:\n10.0pt;mso-line-height-rule:exactly\">Print Name:<br />\n</p><p class=\"MsoNormal\" style=\"margin-bottom:0in;margin-bottom:.0001pt;line-height:\n10.0pt;mso-line-height-rule:exactly\">________________________________<br />\n</p><p class=\"MsoNormal\" style=\"margin-bottom:0in;margin-bottom:.0001pt;line-height:\n10.0pt;mso-line-height-rule:exactly\">Signature:<br />\n</p><p class=\"MsoNormal\" style=\"margin-bottom:0in;margin-bottom:.0001pt;line-height:\n10.0pt;mso-line-height-rule:exactly\">________________________________<br />\n</p><p class=\"MsoNormal\" style=\"margin-bottom:0in;margin-bottom:.0001pt;line-height:\n10.0pt;mso-line-height-rule:exactly\"><br></p><p class=\"MsoNormal\" style=\"margin-top:0in;margin-right:11.6pt;margin-bottom:\n0in;margin-left:5.85pt;margin-bottom:.0001pt;text-indent:-.25pt;line-height:\n100%\">Thank you for allowing our company\nto be a part of your home improvement project. &nbsp;If you\nhave any additional questions or concerns, please contact us directly.</p>",
    "TermCode": "QuotationTerms",
    "SetupType": "",
    "IsDefault": "0"
  }
]

```

## Download Material List

` URL: https://testsite.cloudmanagerpro.com/AndroidGP/downloadMaterialList.php `

> Data Sent

```
?year=jimbob%40bobbbbbbbburgerss.com%7CJF%7Cdbz95uov&SID=unknown&date=1461089006&startingRow=0&sync=1
```
> Data Recieved

```
null for now need to update 
```


## Downlload Inspection Types

` URL: https://testsite.cloudmanagerpro.com/AndroidGP/downloadInspectionTypes.php `

> Data Sent

```
year=jimbob%40bobbbbbbbburgerss.com%7CJF%7Cdbz95uov&SID=unknown
```

> Data Recieved

```
[
  {
    "InspectionType": "0",
    "InspectionName": "Sales",
    "SweepID": "0"
  },
  {
    "InspectionType": "1",
    "InspectionName": "Service",
    "SweepID": "0"
  },
  {
    "InspectionType": "2",
    "InspectionName": "Audit",
    "SweepID": "0"
  }
]
```

## Download Contractor Tax

` URL: https://testsite.cloudmanagerpro.com/AndroidGP/downloadContractorTax.php `

> Data Sent 

```
?year=jimbob%40bobbbbbbbburgerss.com%7CJF%7Cdbz95uov&SID=unknown&date=0&sync=1
```

> Data Received

```
[
  {
    "AutoIncre": "193",
    "IntContNum": "1728",
    "TaxName": "Mich",
    "TaxRate": "6.0",
    "LastUpdated": "2016-04-21 14:57:34"
  }
]
```

### Sync Contractor Info

`URL https://testsite.cloudmanagerpro.com/AndroidGP/syncContInfo.php `

> Data Sent

```
year=jimbob%40bobbbbbbbburgerss.com%7CJF%7Cdbz95uov&SID=unknown&Date=1461264709&CurrentTime=1461264733&BusName=Jim&Address=3444%20Anderson%20Road&City=Somewhere&State=MA&Zip=55555&Phone=555-555-5555&Fax=555-222-2222&Website=www.website.com&Motto=&ContContactFirst=&ContContactLast=&ContEmail=&FinanceTerms=0&FinanceRate=0.000
```

> Data Received

```
EMPTY RESPONSE
```


### Sync Deleted Items

` URL: https://testsite.cloudmanagerpro.com/AndroidGP/syncDeletedItems.php `

> Data Sent

```
year=jimbob%40bobbbbbbbburgerss.com%7CJF%7Cdbz95uov&SID=unknown&send=1&CurrentTime=1461264886&Date=0
```

> Data Recieved

```
null atm

```



### Sync Taxes

` URL https://testsite.cloudmanagerpro.com/AndroidGP/syncTaxes.php `

> Data Sent

```
year=jimbob%40bobbbbbbbburgerss.com%7CJF%7Cdbz95uov&SID=unknown&CurrentTime=1461264953&Date=1461264867&ID=0&Name=Mich&Rate=6.0
```

> Data Received

```
193
```







