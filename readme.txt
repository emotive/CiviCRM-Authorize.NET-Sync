/*****************************************
 * CiviCRM Authorize.net CRON Sync
 * by Chang Xiao (chang@emotivellc.com)
 * emotve LLC, 2011
 ****************************************/
 
 ----------------------------------------
 Description
 ----------------------------------------
 This module will attempt to synchronize CiviCRM contributions with Authorize.NET and update
 the contribution information accordingly
 
 
1. Requirements
2. Installation
3. Configuration 
4. Usage
5. Future considerations
6. Change log

/*************************************************************
 * 1. Requirements
 ************************************************************/
 
This module requires the following:
	
	(CiviCRM/Drupal Requirement)
	1. Drupal Version 6.x
	2. CiviCRM < 3.4 (Versions with API V2) Or (Turn off API V3 on CiviCRM 3.4)
	3. An active Authorize.NET payment processor (live and being used)

/*************************************************************
 * 2. Installation 
 ************************************************************/	

 The following steps are needed to make the module work:
 
	1. Upload all the contents in the civicrm_authorizenet folder to your drupal 
	sites/all/modules/ folder.
	
	2. Enable the module "CiviCRM AuthorizeNet Sync" from admin/build/modules

/*************************************************************
 * 3. Configuration
 ************************************************************/		
	
	There is not configuration except that you need a live 
	Authorize.NET payment processor and an API login and 
	Transaction key set up in CiviCRM's backend

/*************************************************************
 * 4. Usage
 ************************************************************/	

	1. Set up the synchronization cron to run at your desired interval,
	it should look like
	
	curl -d "name=[user]&pass=[pass]&key=[civicrm_site_key]" 
	http://yoursite.com/civicrm_authorizenet_sync
	
	where
	
	[user] = drupal user that can run civicrm cron such as CiviCRM mail cron
	[pass] = password associated with that user
	[civicrm_site_key] = can be found under sites/default/civicrm.settings.php

	The cron process will perform the following:
	
	If the transaction has been successfully settled (cleared), it will update the contribution record's:
		fee_amount
		net_amount
		receipt_date

	If the transaction has been voided, it will create a new contribution record with all the details of the original contribution and:
		contribution_status_id: 3 (cancelled)
		cancelled date
		cancelled reason: original transaction id
		total amount: negative amount of the original contribution
		
	If the transaction has been refunded, it will create a new contribution record with all the details of the original contribution and:
		trxn_id: new transaction id
		cancelled date: date the refund was issued
		cancelled reason: original transaction id
		total amount: amount refunded (might be different from original contribution because of partial refund.
		contribution_status_id: 7 (Refunded)

/*************************************************************
 * 6. Change log
 ************************************************************/

1.5 Total Code Rework 4/12/2011
	Now can sync refuneded and voided transactions by creating new 
	contribution records with negative amount.
	New contribution status id 7 (Refunded) that can be used in 
	Donor detail report.
	Database logs.
	
1.0 Working Version: 3/23/2011