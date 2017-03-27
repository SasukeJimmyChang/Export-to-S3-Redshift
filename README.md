<h1> Export to S3 + Redshift

<h2> Export to S3 

* Service => Mobile Analytics => Export to S3 

h2. Export to Redshift

* Service => Mobile Analytics => Redshift + Export to S3
* "Steps by docs":https://aws.amazon.com/tw/blogs/aws/export-amazon-mobile-analytics-to-redshift-automatically/ or "Steps by blog":http://docs.aws.amazon.com/mobileanalytics/latest/ug/auto-export-getting-started-redshift.html
* %{color:red}We need a group authorized which includes 「CloudFormation」 access advisor, or we get the return message "Bad request".%
** (Only administrator can do) Service => IAM => User => add into "Develop group".
* %{color:red}The S3 bucket and Amazon Redshift cluster must be in the US East (N. Virginia) Region.%

* Amazon Redshift does not provide or install any SQL client tools or libraries, so you must "install SQL client tool":http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-prereq.html
* "Connect to the Redshift Cluster":http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-connect-to-cluster.html

* %{color:red}Your must add your IAM role to Redshift, or you can't operate SQL command for Redshift.%
!redshift_access_denied.png!


* Set you EC2 instance inbound IP: %{color:red}210.63.0.0/16% (Acer company)
!ec2-security-group-ip.png!

h2. Redshift SQL 

* How to get credentials?
!iam_role.png!

* Copy data from S3
<pre><code class="ruby">
  copy customer
  from 's3://mybucket/customer'  //Your S3 file path//
  credentials '<aws-auth-args>';
</code></pre>
** Example
<pre><code class="ruby">
  copy testtable
  from 's3://mobile-analytics-11-10-2016-68e606befbf542298f14a32d1515a9f7/awsma/events/7297ae7eed0c41998d05d842096c8537/2016/11/10/10/test_de_gz'
  credentials 'aws_iam_role=arn:aws:iam::641923872123:role/Halo-mobileanalytics-autoExportS3ToRedshift';
</code></pre>

* "Copy data from S3 by JSON doc 1":http://docs.aws.amazon.com/redshift/latest/dg/copy-usage_notes-copy-from-json.html or "Copy data from S3 by JSON doc 2":http://docs.aws.amazon.com/redshift/latest/dg/r_COPY_command_examples.html#r_COPY_command_examples-copy-from-json
** Example
<pre><code class="ruby">
copy category_test2
from 's3://mobile-analytics-11-10-2016-68e606befbf542298f14a32d1515a9f7/awsma/events/7297ae7eed0c41998d05d842096c8537/2016/11/10/10/test_de_gz'
credentials 'aws_iam_role=arn:aws:iam::641923872123:role/Halo-mobileanalytics-autoExportS3ToRedshift'
json 's3://mobile-analytics-11-10-2016-68e606befbf542298f14a32d1515a9f7/jsonpaths/halo_awseventexportjsonpaths.json';
</code></pre>

* "(ALTER TABLE ADD/DROP COLUMN)":http://docs.aws.amazon.com/mobile/sdkforandroid/developerguide/analytics.html

h2. APPENDS
** "Amazon Redshift and PostgreSQL differences":http://docs.aws.amazon.com/redshift/latest/dg/c_redshift-and-postgres-sql.html
** "Amazon Redshift Best Practices":http://docs.aws.amazon.com/redshift/latest/dg/best-practices.html
** "Configure Security Options for Connections":https://docs.aws.amazon.com/redshift/latest/mgmt/connecting-ssl-support.html
** "(Authorizing Amazon Redshift to Access Other AWS Services On Your Behalf)":https://docs.aws.amazon.com/redshift/latest/mgmt/authorizing-redshift-service.html
