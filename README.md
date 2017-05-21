# i2b2-pm-cas
[Atlanta Clinical and Translational Science Institute (ACTSI)](http://www.actsi.org), [Emory University](http://www.emory.edu), Atlanta, GA

## What does it do?
This is a stock i2b2 1.7.05 project management module patched with support for delegating authentication to Eureka via its patched [JASIG CAS](http://jasig.github.io/cas/4.1.x/index.html) server module, found at https://github.com/eurekaclinical/cas. JASIG CAS is a single sign-on system. This allows users of Eureka and i2b2 to login once and have access to both systems.

The code is adapted from similar code for an older version of i2b2 by Dan Connolly found at https://bitbucket.org/DanC/i2b2-pm-cas.

## Version history
### Version 1.0
Initial release.

## CAS implementations supported
We expect any full implementation of version 2 of the CAS protocol to work. In particular, the implementation must support proxying. The following implementations of CAS are known to work:
* [Eureka! Clinical CAS](https://github.com/eurekaclinical/cas), which is a patched version of [JASIG CAS version 3.5.2](https://wiki.jasig.org/display/CASUM/Home)
* [Shibboleth Identity Provider version 3](https://wiki.shibboleth.net/confluence/display/IDP30/Home) with CAS emulation turned on

## Requirements
See the [i2b2 installation guide](https://www.i2b2.org/software/files/PDF/current/FR_Installation_Guide.pdf) for requirements. Our patches do not change them.


## Versions of CAS Supported
Eureka! currently uses a patched copy of CAS version 3.5.2. While these i2b2 patches have only been tested with Eureka!'s patched CAS, they likely also will work with stock CAS 3.5.2.


## Installation
If you are installing i2b2 on a separate machine from Eureka!, create a file, `/etc/eureka/application.properties` with one line: 
`cas.url=URL to your cas server`. This file already exists if you have already installed Eureka on the same machine.

Finally, you may need to install the SSL certificate served by CAS server into the certificate store of the Java installation that i2b2 is using. This is especially true if the CAS server's certificate is self-signed. See https://www.sslshopper.com/article-most-common-java-keytool-keystore-commands.html for instructions on how to work with the Java certificate store.

In order to login to i2b2 using CAS, you also need to install our patched i2b2 webclient, found at https://github.com/eurekaclinical/i2b2-webclient-cas.


## Configuration
When using these patches, the i2b2 project management module's user data table becomes an authorization table. The code authenticates the user with Eureka! CAS, and then it checks the user data table for the existence of the user's account before authorizing the user. Any passwords in the user data table are ignored.

When logging into i2b2 services by means other than the patched web client, this patched PM cell will authenticate the user with i2b2's built-in authentication mechanism, that is, by checking the username and password against what is stored in the `PM_USER_DATA` table in the project management cell's schema. 

## Security warning
**If your users will only be logging into i2b2 through the patched web client, populate the password field in `PM_USER_DATA` with a randomly generated hashed password. Do not set the password field to NULL.** Our limited testing suggests that leaving the password field set to NULL currently results in the user not being able to login to i2b2 through the built-in mechanism, but we are unaware of any guarantee that the behavior will stay that way. 

## Licensing
This code is released under the i2b2 Software License version 2.1, available at https://www.i2b2.org/software/i2b2_license.html.

