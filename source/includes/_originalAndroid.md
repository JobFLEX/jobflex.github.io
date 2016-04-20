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


` table: ContractorInfo `

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

` ContractorTabletSetup


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

public void lookup() {
}

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








