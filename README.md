# Export to S3 + Redshift

| Export to S3 |
| --- |
| Service => Mobile Analytics => Export to S3 |

| Export to Redshift |
| --- |
|1. Service => Mobile Analytics => Redshift + Export to S3 |
|2.  [Steps by docs](https://aws.amazon.com/tw/blogs/aws/export-amazon-mobile-analytics-to-redshift-automatically/)  or [Steps by blog](http://docs.aws.amazon.com/mobileanalytics/latest/ug/auto-export-getting-started-redshift.html)|
|3. **We need a group authorized which includes 「CloudFormation」 access advisor, or we get the return message "Bad request".**|
|4. (Only administrator allow to do) Service => IAM => User => add into "Develop group".|
|5. **The S3 bucket and Amazon Redshift cluster must be in the US East (N. Virginia) Region.**|
|6. Amazon Redshift does not provide or install any SQL client tools or libraries, so you must [install SQL client tool](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-prereq.html) |
|7. [Connect to the Redshift Cluster](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-connect-to-cluster.html) |
|8. **Your must add your IAM role to Redshift, or you can't operate SQL command for Redshift.**|
|![redshift_access_denied](https://cloud.githubusercontent.com/assets/22315139/24493702/5c831bf4-1562-11e7-966d-827fb16e90b4.png)|
|9. Set you EC2 instance inbound IP: **210.63.0.0/16** (Acer company)|
![ec2-security-group-ip](https://cloud.githubusercontent.com/assets/22315139/24493705/5dd096e4-1562-11e7-9248-de63148a52a1.png)

># Redshift SQL  
>1. How to get credentials? 
>![iam_role](https://cloud.githubusercontent.com/assets/22315139/24493697/59d6d35a-1562-11e7-9a38-5029cb82d500.png)
>2. Copy data from S3|
>```ruby
  copy customer
  from 's3://mybucket/customer'  //Your S3 file path//
  credentials '<aws-auth-args>';
  ```
>3. Example
>```ruby
  copy testtable
  from 's3://mobile-analytics-11-10-2016-68e606befbf542298f14a32d1515a9f7/awsma/events/7297ae7eed0c41998d05d842096c8537/2016/11/10/10/test_de_gz'
  credentials 'aws_iam_role=arn:aws:iam::641923872123:role/Halo-mobileanalytics-autoExportS3ToRedshift';
```
>4. [Copy data from S3 by JSON doc 1](http://docs.aws.amazon.com/redshift/latest/dg/copy-usage_notes-copy-from-json.html) or [Copy data from S3 by JSON doc 2](http://docs.aws.amazon.com/redshift/latest/dg/r_COPY_command_examples.html#r_COPY_command_examples-copy-from-json)
>5. Example
>```ruby
copy category_test2
from 's3://mobile-analytics-11-10-2016-68e606befbf542298f14a32d1515a9f7/awsma/events/7297ae7eed0c41998d05d842096c8537/2016/11/10/10/test_de_gz'
credentials 'aws_iam_role=arn:aws:iam::641923872123:role/Halo-mobileanalytics-autoExportS3ToRedshift'
json 's3://mobile-analytics-11-10-2016-68e606befbf542298f14a32d1515a9f7/jsonpaths/halo_awseventexportjsonpaths.json';
```
>6. [(ALTER TABLE ADD/DROP COLUMN](http://docs.aws.amazon.com/mobile/sdkforandroid/developerguide/analytics.html)

| APPENDS |
| --- |
| [Amazon Redshift and PostgreSQL differences](http://docs.aws.amazon.com/redshift/latest/dg/c_redshift-and-postgres-sql.html) |
| [Amazon Redshift Best Practices](http://docs.aws.amazon.com/redshift/latest/dg/best-practices.html) |
| [Configure Security Options for Connections](https://docs.aws.amazon.com/redshift/latest/mgmt/connecting-ssl-support.html) |
| [(Authorizing Amazon Redshift to Access Other AWS Services On Your Behalf] |(https://docs.aws.amazon.com/redshift/latest/mgmt/authorizing-redshift-service.html) |
