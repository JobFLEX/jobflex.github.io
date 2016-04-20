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




