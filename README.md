# S3 Object Storage With OpenShift

This is an sample PHP application that uses S3 for storage backend. You will need your `AWS_ACCESS_KEY_ID` and your `AWS_SECRET_ACCESS_KEY` from your AWS account.

First Create your application providing your AWS keys as environment variables
```
user@host$ oc new-app openshift/php~https://github.com/christianh814/php-object-store.git --name=uploader 
```

Things to keep in mind:
* `ose new-app` Creates a new application on OSE3
* `openshift/php` This tells OSEv3 to use the PHP image stream provided by OSE
* Provide the git URL for the project
  * Syntax is `imagestream~souce`


Create your `~/ec2-creds` file with your `AWS_ACCESS_KEY_ID` and your `AWS_SECRET_ACCESS_KEY` variables set; and `source` them
```
user@host$ source ~/ec2-creds
```

Create a secret file using this information
```
user@host$ echo -n $AWS_ACCESS_KEY_ID > /tmp/aws-access-key
user@host$ echo -n $AWS_SECRET_ACCESS_KEY  > /tmp/aws-secret-key
user@host$ oc secrets new s3secret aws-access-key=/tmp/aws-access-key aws-secret-key=/tmp/aws-secret-key
user@host$ oc secrets add serviceaccounts/default secrets/s3secret
user@host$ oc volume dc/uploader --add --type=secret  --secret-name=s3secret -m /etc/secret
```

Expose the route (if it isn't already)
```
user@host$ oc expose svc/uploader
```

Set the Environment variable for your bucketname (triggers a new deployment)
```
user@host$ oc env dc/uploader S3_STORAGE_BUCKET=openshift-objstore-demo
```

Scale up as you wish
```
user@host$ oc scale --replicas=3 dc/uploader
```
