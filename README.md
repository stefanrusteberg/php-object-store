# Object Storage With OpenShift

This is an sample PHP application that uses S3 for storage backend. You will need your `AWS_ACCESS_KEY_ID` and your `AWS_SECRET_ACCESS_KEY` from your AWS account.

First Create your application providing your AWS keys as environment variables
```
user@host$ oc new-app openshift/php~https://github.com/christianh814/php-object-store.git -e AWS_ACCESS_KEY='XXXXXXXXXXXXXXXXXXXX' -e AWS_SECRET_KEY='XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX'
```

Things to keep in mind:
* `ose new-app` Creates a new application on OSE3
* `openshift/php` This tells OSEv3 to use the PHP image stream provided by OSE
* Provide the git URL for the project
  * Syntax is `imagestream~souce`
* Provide your aws keys (passing `-e` for every keypair) as the following
  * `AWS_ACCESS_KEY`
  * `AWS_SECRET_KEY`

Once you created the app, the build should start normally. If not, envoke with

```
user@host$ oc start-build php-object-store
```

Once the build completes; create and add your route:
```
user@host$ oc expose svc php-object-store --hostname=uploader-objstore.cloudapps.example.com
```

Scale up as you wish
```
user@host$ oc scale --replicas=3 dc/php-object-store
```
