# Whole Whale GTM Basic Formula

The file is meant to be a ready-set starting point to set basic analytics tracking on multiple sites through a single Google Tag Manager container and multiple property ids instead of views.

Make sure you have a property for each site that you will connect and one standalone cross-domain property, and make sure that each site has the same container's GTM script.
   
## Formula Contents

### Variables

* Google Analytics Settings: 
You will need to add your cross-domain Google Analytics property ID. Already connected to the currently existing tags, it is a reusable option to not have to rewrite any general settings that your tag would share with others
* Cross-Domain Google Analytics Settings: 
Already connected to the currently existing tags, it is a reusable option to not have to rewrite any general settings that your tag would share with others
* PII Scrubber:
You will need to edit the code by replacing in `?!youremail\.com` and `?youremail\.com`the domain name of your sites email (make sure to keep the back slash before the dot)
to use this variable, edit the Google Analytics Settings variable. Under "more settings" > "fields to set" add a field and fill in `customTask` under name, and `{{PII Scrubber}}`under value
* Cross-domain GA Propety ID: 
This variable is meant to provide the property id related to the visited page. It will check the corresponding hostname on the browser to the assigned ones on the variable and return the assigned property id. For it to work, under the `input` column you will fill the fields with the sites hostnames and fill in the related property id across from it under the `output` column 

Ie.

|`Input`         |          `Output`|
|-----------------|------------------|
|www.wholewhale.com |       UA-XXXX123|
|sample.wholewhale.com  |   UA-XXXX321|
|ie.wholewhale.com   |      UA-XXXX231|

* Auto-Link Domains: 
This variable makes it easy to edit how many URL domains we need to account for like in the Cross-domain GA Property ID. In this case you only need to provide a **comma separated** list of URLS: `www.wholewhale.com,sample.wholewhale.com,ie.wholewhale.com`

### Triggers
If not very acquainted with GTM. Triggers are like sensors (commonly called "listeners")whose solely job is to check if an action has been performed on the page by a visitor and let GTM know that the tags related to it can be executed. Each Trigger relates to an action and it contains certain requirements for it to accept it (called "rules").

* 15 Second Timer: It checks if a visitor has been on the site for 15 seconds
* All Clicks: It checks if anything on the page has been clicked
* All LinksL: It checks if any links have been clicked
* All Outbound Links: it checks if any links outside of the site (AKA that have a different hostname)
* Form Submissions: it checks if a form has been filled and sent
* PDF Click: Checks on clicks to PDF downloadable content
* Scroll Depth: It listents for difenrent percentage ranges that the user had scrolled through the page

### Tags

All tags have a duplicate version. One will send the tracking data to the visited site's GA Property, and the other is meant to send the same tracking data to the cross-domain property.

* Adjusted Bounce Rate
`Category: visit
Action: 15 Seconds
Label: {{Page Path}}`
* All Link Tracking
`Category: Link Clicks
Action: Page: {{Page URL}}
Label: Link: {{Click URL}} - {{Click Text}}`
* Click Tracking
`Category: All Clicks
Action: Text: {{Click Text}}
Label: Link: Page: {{Page URL}}`

* Google Analytics Pageview

* Outbound Links
`Category: Outbound Links
Action: Click
Label: {{Click URL}}`
* PDF Downloads
`Category: Download
Action: PDF
Label: {{Click URL}}`
* Scroll Depth 
`Category: Scroll Depth
Action: Percentage: {{Scroll Depth Threshold}}
Label: Page URL: {{Page URL}}`

## Installation

After downloading the .json file, under the Admin tab within the GTM container, click `Import Container`, click `Choose Container File` and grab the .json file, then select `existing`, select your default works space, and keep `merge` as the option checked, select rename conflicting tags if your container is not empty and click confirm.

You are now ready to use the tags. Preview and edit as needed. 
