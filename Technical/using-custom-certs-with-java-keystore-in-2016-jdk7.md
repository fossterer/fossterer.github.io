# Using Custom SSL certificates with Java Keystore in 2016

Date: Nov 16 2016

For a recent assignment, I had to make a Mule app (say, just another Java program) use an SSL certificate in order establish a secure channel channel to communicate with a remote server.

In general, our web browsers handle this certificate exchange procedure behind the scenes.

In such a case one would naturally incline to adopt the approach described at [1]. However, in this case, the certificate obtained fails the process described by throwing:

    javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target

Good amount of time was spent reading on the root cause for this which led me to try another developer's findings published at [2].
All is not well yet!

    javax.net.ssl.SSLException: java.lang.UnsupportedOperationException

The issue here is that the proposed solution uses JDK 1.5.0 which would have been fine if I were doing this in 2006 but I'm not!
JDK 1.7.x that I'm using now brought in a few changes which makes a call to a function that wouldn't have been expected in JDK 1.5.0

The fix for this was thankfully, documented in 2013 (well, almost 7 years later) at [3].

At the end, by following all these instructions, I was able to add the certificate to my trusted keystore which enabled the SSL process as required.


[1]: http://blog.jgc.org/2011/06/importing-existing-ssl-keycertificate.html
[2]: http://nodsw.com/blog/leeland/2006/12/06-no-more-unable-find-valid-certification-path-requested-target
[3]: http://infposs.blogspot.com/2013/06/installcert-and-java-7.html