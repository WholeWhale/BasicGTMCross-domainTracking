# Whole Whale GTM Basic Cross-Domain Formula

This file is meant to be a ready-set starting point to set basic analytics tracking on multiple sites through a single Google Tag Manager container and multiple property ids instead of views.

*Make sure you have a property for each site that you will connect and one standalone cross-domain property, and make sure that each site has the same container's GTM script.*

## Installation

Download the .json file.  
In Google Tag Manager:

* Click the Admin tab in your GTM container
* Click `Import Container`
* Click `Choose Container` File and select the .json file
* Under `Choose a workspace`, select existing and select your default workspace
* Under `Choose an import option`, select `merge`
* Select `rename conflicting tags` if your container is not empty
* Click *confirm*

You are now ready to customize and use the tags. Preview and edit as needed.
   
## Formula Contents

### Variables

* Google Analytics Settings: 
*You will need to add your cross-domain Google Analytics property ID*, which is necessary since it is already connected to the currently existing tags, This variable is a reusable option to not have to rewrite any general settings that your tag would share with others. - If you create a tag where you will need to change any of this general settings, within the tag options you can check the `Enable overriding settings in this tag` section which will allow you to update those settings for the individual tag only
* Cross-Domain Google Analytics Settings: 
Same rules as the `Google Analytics settings` apply to this variable. The difference is that this variable will apply the specific property id based on the hostname of your site and the previous variable is only used to send duplicate data to you cross-domain property
* PII Scrubber:
(*Not necessary for everyone. If you know or you believe you have Personal Identifiable Information being passed any of the URL paths of your site, this variable will rid of it*) You will need to edit the code by replacing `?!youremail\.com` and `?youremail\.com`with the domain name of your site's email (make sure to keep the back slash before the dot).  
**_If you don't need a scrubber, follow these steps to get rid of it_**: Edit the Google Analytics Settings and Cross-Domain Google Analytics Settings variables as follows: under "more settings" > "fields to set" click on the `-` button next to the `customTask` field with the value `{{PII Scrubber}}`. Save your settings variable and you're done
* Cross-domain GA Propety ID: (*updates in this variable are necessary*)
This variable is meant to provide the property id related to the visited page. It will check the corresponding hostname on the browser to the available ones in the variable and return the corelated property id. For it to work, under the `input` column you will fill the fields with the hostnames of all the site you want to have cross domain tracking on and across from each one of them, under the `output` column, fill in the field with the related property id

Ie.

|`Input`         |          `Output`|
|-----------------|------------------|
|www.wholewhale.com |       UA-XXXX123|
|sample.wholewhale.com  |   UA-XXXX321|
|ie.wholewhale.com   |      UA-XXXX231|

* Auto-Link Domains: (*updates in this variable are necessary*)
This variable makes it easy to edit how many URL domains we need to account for in the Cross-domain GA Property ID. To edit, you only need to provide a **comma separated** list of URLS:  
ie. `www.wholewhale.com,sample.wholewhale.com,ie.wholewhale.com, app.someotherurlweown.org`

### Triggers
Triggers are like sensors (commonly called "listeners")whose sole job is to check if an action has been performed on site and let GTM know that the tags related to that trigger can be executed. Each Trigger relates to an action and contains certain requirements in order to fire (called "rules").

* 15 Second Timer: Fires if a visitor has been on the site for at least 15 seconds
* All Clicks: Fires if anything on the page has been clicked
* All Links: Fires if any link has been clicked
* All Outbound Links: Fires if any link to an external site has been clicked(so, any link that does not contain your website's hostname).  
*for this to properly work, you will need to update the second rule under the `trigger fires on` where it says `yoursite.org`. You will need to change it for your site's hostname*
* Form Submissions: Fires if a form has been properly filled out and submitted
* PDF Click: Fires on clicks to PDF downloadable content
* Scroll Depth: Fires when users reach difenrent percentage ranges through the page

### Tags

All tags have a duplicate version. One will send the tracking data to the visited site's GA Property, and the other is meant to send the same tracking data to the cross-domain property.

* Adjusted Bounce Rate  
```
Category: visit  
Action: 15 Seconds  
Label: {{Page Path}}
```  
* All Link Tracking  
```
Category: Link Clicks  
Action: Page: {{Page URL}}  
Label: Link: {{Click URL}} - {{Click Text}}
```  
* Click Tracking  
```
Category: All Clicks  
Action: Text: {{Click Text}}  
Label: Link: Page: {{Page URL}}
```  

* Google Analytics Pageview  

* Outbound Links  
```
Category: Outbound Links  
Action: Click  
Label: {{Click URL}}
```  
* PDF Downloads  
```
Category: Download  
Action: PDF  
Label: {{Click URL}}
```  
* Scroll Depth   
```
Category: Scroll Depth 
Action: Percentage: {{Scroll Depth Threshold}}  
Label: Page URL: {{Page URL}}
```  

 
