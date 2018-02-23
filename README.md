# Free, easy SSL for your Elastic Beanstalk single instance.

#### Fetch and renew certificates for your Elastic Beanstalk instance automatically with these hot .ebextensions! Because you don't want to pay $20/month for load balancing.

Now with 100% [EFF](https://www.eff.org/), [Let's Encrypt](https://letsencrypt.org/), and [Certbot](https://certbot.eff.org/)!

#### Disclaimer
* Currently only configured for NGINX
* Apache config welcome, please contribute

#### Documentation
Control ebcert with three Elastic Beanstalk environment variables:
* `CERT_EMAIL`: Email address for registration, renewal, and recovery
* `CERT_DOMAIN`: The domain or comma separated domains you want to certify
* `CERT_PRODUCTION`:
	* When `true`, genuine, production-ready  Let's Encrypt certificates will be issued
	* In all other cases, certificates will be issued by the Let's Encrypt staging server. *These certificates are not production ready.*
	* Let's Encrypt has serious [rate limits](https://letsencrypt.org/docs/rate-limits/). You are far more likely to encounter these limits when `CERT_PRODUCTION` is `true`.

#### Usage
1. Customize nginx.conf to suit your use case.
2. Boot or redeploy your app with your customized ebcert .ebextensions.
3. Point desired `CERT_DOMAINS` to your Elastic Beanstalk environment. (ebcert uses certbot's webroot authentication method, which requires connectivity at the domain at the time of authentication.)
4. Set `CERT_EMAIL`, `CERT_DOMAIN`, and (if production-ready) `CERT_PRODUCTION` as Elastic Beanstalk environment variables.
5. Keep $20/month!

#### Migrating to production
* Via environment variable
	* Set the Elastic Beanstalk environment variable `CERT_PRODUCTION` to `true`

* Via [SSH](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)
	* `sudo /opt/ebcert/cert.sh -m`

* Via [EB CLI](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3.html) clone
	* Point the domain(s) you wish to certify to a new CNAME you select for your clone (`your-new-env-cname`)
	* `eb clone your-current-env-cname -n your-new-env-name -c your-new-env-cname --envvars CERT_PRODUCTION=true`

### Changing certified domains
Change (add, remove, swap) your certified domains at any time by changing the value of `CERT_DOMAIN`. Note that such changes will count toward your rate limit if `CERT_PRODUCTION` is `true`.

#### Thanks
Thanks to Tony Gutierrez and all participants in [this gist](https://gist.github.com/tony-gutierrez/198988c34e020af0192bab543d35a62a).
