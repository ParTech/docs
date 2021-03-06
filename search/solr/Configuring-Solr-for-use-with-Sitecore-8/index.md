---
layout: default
title: Configuring Solr for use with Sitecore 8
category: search
---

> This is a work in progress!

### How to configure SOLR to enable only analytics index 

**Step 1.** Install Solr via Bitnami as described  [here]({{ site.baseurl }}/search/solr/installing-solr-using-the-bitnami-apache-solr-stack/)

**Step 2.** Download [this zip]({{ site.baseurl}}/search/solr/Configuring-Solr-for-use-with-Sitecore-8/sitecore_analytics_index.zip) with sample analytics index (it has no data, just configuration)

**Step 3.** Unpack downloaded zip to C:\Bitnami\solr-5.0.0-0\apache-solr\solr folder:

<img src="/docs/images/search/solr/Configuring-Solr-for-use-with-Sitecore-8/solrfolder.png"  />

**Step 4.** Go to Solr admin page by loading http://localhost:8983/solr/ (it is default port, you should use the one that you specified when installed Bitnami) or by pressing Go to application in Bitnami tool:

<img src="/docs/images/search/solr/Configuring-Solr-for-use-with-Sitecore-8/bitnamistart.png"  />

**Step 5.** Go to Core admin, press Add core and fill **name** and **instance dir** with "sitecore_analytics_index":

<img src="/docs/images/search/solr/Configuring-Solr-for-use-with-Sitecore-8/addcore.png"  />

**Step 6.** Download Solr Support Package from your <a href="https://dev.sitecore.net/Downloads/Sitecore_Experience_Platform/8_0.aspx" >update of Sitecore 8</a>. Unpack the package and copy all dlls from bin folder to your Sitecore bin folder.

**Step 7.** If you don’t have the Microsoft.Practices.Unity.dll in your bin folder, download and install <a href="http://www.microsoft.com/en-gb/download/details.aspx?id=17866">Unity</a> and the needed dll will be in C:\Program Files (x86)\Microsoft Unity Application Block 2.1\Bin.

**Step 8.** Go to App_Config/Include folder:

- Uncomment Sitecore.ContentSearch.Solr.DefaultIndexConfiguration.config
- Uncomment Sitecore.ContentSearch.Solr.Index.Analytics.config
- Comment Sitecore.ContentSearch.Lucene.Index.Analytics.config

**Step 9.** Change next settings in Sitecore.ContentSearch.Solr.DefaultIndexConfiguration.config:

`<setting name="ContentSearch.Solr.ServiceBaseAddress" value="http://localhost:9999/solr" />` - set your port here

`<setting name="ContentSearch.Solr.EnableHttpCache" value="false" />` - change to false

**Step 10.** If you get an `index has no configuration` exception after starting the site, check your config patch files for Lucene indexes that are missing the `<configuration />` element.  
When you enable SOLR it becomes the default provider and that causes this exception on existing Lucene indexes that don't have the provider explicitely configured.   

Enjoy!

**P.S.** To check if anything is getting into your index, make some completed visits to the website (followed by session end) and run next query in the browser:

`http://localhost:8983/solr/sitecore_analytics_index/select?q=*&rows=1000

