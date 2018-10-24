# london-hackday-2018
Notes and code for the London Lucene Hackday 2018 (see below for notes on the Montreal hackday that followed a week afterwards)

## Task 1: How can we build a command-line application for inspecting Lucene indexes? Some of these may be very large and use versions of Lucene from 4.x upwards. Considering various existing projects such as:
* Marple https://github.com/flaxsearch/marple
* Solr Chrome Debugger https://chrome.google.com/webstore/detail/solr-query-debugger/
* Quepid www.quepid.com
* Sense plugin for Elasticsearch https://www.elastic.co/guide/en/sense/current/installing.html
* Luke https://github.com/DmitryKey/luke
* Clue https://github.com/javasoze/clue
![whiteboard1](https://github.com/flaxsearch/london-hackday-2018/blob/master/IMAG2335.jpg)

Results: The team reported that they had managed to build Marple to read indexes from Lucene versions 4,5,6,7 and 8. They also managed to add a 'Query' tab to the UI to allow queries to be sent to the index under inspection. Some of the team will continue to work on this on a branch after the hackday and we expect this to be merged back into the main development branch at some point.

## Task 2: Review Alessandro's various Lucene and Solr JIRA tickets:
* Bugs
* https://issues.apache.org/jira/browse/LUCENE-6687 More Like This Bug
* https://issues.apache.org/jira/browse/LUCENE-8329 Size Estimator Bug
* https://issues.apache.org/jira/browse/SOLR-12304 More Like This Bug Solr side

* Improvements
* https://issues.apache.org/jira/browse/LUCENE-8326 More Like This Params Refactor
* https://issues.apache.org/jira/browse/LUCENE-8347 BlendedInfixSuggester Improvement* 
* https://issues.apache.org/jira/browse/SOLR-12238 Synonym Query Style Boost By Payload
* https://issues.apache.org/jira/browse/SOLR-12243 Edismax Bug ( Elizabeth Haubert, me)

Reference for the Existing Bugs tracks:
* https://github.com/SeaseLtd/lucene-solr
Reference for how to contribute to Lucene/Solr:
* https://wiki.apache.org/lucene-java/HowToContribute
* https://wiki.apache.org/solr/HowToContribute

Results: the team were taken through the process of reporting bugs and creating patches on the Lucene/Solr JIRA repository, and looked in depth at one of the issues. 

## Task3: Review issue 'Different Solr replicas give different result positions' from https://github.com/flaxsearch/london-hackday-2016 item 3 - what's the current state of play with this? *

* References: https://mail-archives.apache.org/mod_mbox/lucene-solr-user/201301.mbox/%3Czarafa.51006f06.0ea4.1386468330e702d7@mail.openindex.io%3E

* Here is our working branch: https://github.com/fguery/lucene-solr/tree/replicaChoice

* Useful command: cd solr/core ant test -Dtestcase=DistributedQueryComponentReplicaMarkerTest

Results: the team managed to reproduce the issue with indexes as small as 50 documents: different numbers of deleted documents in a replica caused different term statistics and thus different result ordering.

René Kriegler comments:
"There is an additional issue with floating point operation precision across JVMs. There is actually no guarantee in Java that a math calculation using numbers of type float will result in exactly the same floating point number across JVM instances (even for the same JVM version). We rarely run into issues here, but I’ve seen a Solr custom plugin having problems when calculating scores. 

I think there is nothing that prevents the issue in standard Lucene similarity calculations - we are probably just lucky that scores are normally discrete enough. To avoid the issue we would have to use the strictfp (https://www.codejava.net/java-core/the-java-language/java-keyword-strictfp) keyword to mark up components involved in the calculation."

# montreal-hackday-2018

* Andy Hind of Oracle has worked on a patch to create a Query Parser for MinHash queries, a development on https://issues.apache.org/jira/browse/LUCENE-6968 - now at https://issues.apache.org/jira/browse/SOLR-12879
* Matt Pearce of Flax worked on adapting Flax's Harahachibu https://github.com/flaxsearch/harahachibu to work with the Solr Metrics API as another source of data to indicate that storage might be full - now in a branch at https://github.com/flaxsearch/harahachibu/tree/solr_metrics
* Steve Rowe of Lucidworks and Liz Haubert of OSC worked on https://issues.apache.org/jira/browse/SOLR-12243 and ended up separating it into two bugs - one in Lucene as https://issues.apache.org/jira/browse/LUCENE-8531 and the remainder on the original ticket - a fix for the Lucene bug has been proposed
* Ravindra Harige discussed with Lucene committers how the process works for committing changes to Lucene/Solr
* Alexandre Rafalovitch discussed with several people improving the onboarding process for new users of Lucene/Solr
* Michael Suzuki of Alfresco worked on https://issues.apache.org/jira/browse/SOLR-11589 a ticket to add eigen decomposition support to the Stream Expression machine learning library.
* Varun Thacker of Lucidworks worked on improving autoscaling suggestions https://issues.apache.org/jira/browse/SOLR-11359 
* Christine Poerschke of Bloomberg worked on how to plug a Deeplearning4j model into Solr streaming expressions (see issue #5429 on Deeplearning4j's Github); revised https://issues.apache.org/jira/browse/SOLR-12699 (a LTR patch for Solr); cleaned up some Solrconfig.xml code https://issues.apache.org/jira/browse/SOLR-12873 

Thanks to all who attended the Hackdays, our kind hosts Mimecast & Netgovern and our dinner/lunch sponsors Elastic, OneMoreCloud and SearchStax. 
