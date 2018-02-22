# Free, easy SSL for your Elastic Beanstalk single instance.

#### Why pay $20/month for SSL ("load balancing") when you can automatically fetch and renew certificates for your Elastic Beanstalk instance with these hot .ebextensions!

Now with 100% EFF, Let's Encrypt, and Certbot!

#### Disclaimer
* Currently only configured for NGINX
* Apache config welcome, please contribute

#### Documentation
Control ebcert with three Elastic Beanstalk environment variables:
* `CERT_EMAIL`: Email address for registration, renewal, and recovery
* `CERT_DOMAIN`: The domain or comma separated domains you want to certify
* `CERT_PRODUCTION`:
	* When `true`, genuine, production-ready  Let's Encrypt certificates will be issued
	* In all other cases, certificates will be issued by the Let's Encrypt staging server. *These certificates are not production ready.* [(Let's Encrypt rate limits)](https://letsencrypt.org/docs/rate-limits/)

#### Usage
1. Boot or redeploy your app with the ebcert .ebextensions.
2. Set `CERT_EMAIL`, `CERT_DOMAIN`, and (if production-ready) `CERT_PRODUCTION` as Elastic Beanstalk environment variables.
3. Profit!

#### Migrating to production
Please note that if `CERT_PRODUCTION` was not `true` when ebcert first ran successfully, you have a certificate from the Let's Encrypt staging server that is not due for renewal and will not automatically be replaced. Replace your staging certificate with a production certificate by **a)** manually running certbot via SSH or **b)** cloning your environment with `CERT_PRODUCTION=true`.

##### Via SSH
1. [SSH into your ec2 instance.](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)
2. Run `sudo /opt/ebcert/cert.sh -f`.

##### Via [EB CLI](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3.html)

1. Select your clone's CNAME (`your-new-env-cname`) and point the domain(s) you wish to certify to that CNAME.
2. Run `eb clone your-current-env-cname -n your-new-env-name -c your-new-env-cname --envvars CERT_PRODUCTION=true`.
