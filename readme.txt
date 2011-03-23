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
	2. CiviCRM < 3.4 (Versions with API V2)
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
	Transaction key

/*************************************************************
 * 4. Usage
 ************************************************************/	

	1. Set up the synchronization cron to run at your desired interval,
	it should look like
	
	curl -d "name=[user]&pass=[pass]&key=[civicrm_site_key]&count=[x]" 
	http://yoursite.com/civicrm_authorizenet_sync
	
	where
	
	[user] = drupal user that can run civicrm cron such as CiviCRM mail cron
	[pass] = password associated with that user
	[civicrm_site_key] = can be found under sites/default/civicrm.settings.php
	[x] = how many record you would like to process each cron run (defaults to 20)

	The cron process will update the following field in the civicrm_contribution
	
		fee_amount
		net_amount
		receipt_date

	If a contribution record shows up as "unsettled" in Authorize.NET, the CRON will finish
	running the current batch and next time it starts it will attempt to sync FROM that transaction
	again.
	

/*************************************************************
 * 6. Change log
 ************************************************************/
 
1.0 Working Version: 3/23/2011