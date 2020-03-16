---
layout: post
title:  "Certificate and Public Key Pinning in Android"
author: prashant
categories: [ codeing, security ]
---

Certificate and Public Key Pinning in Android.

Now a days it’s very easy for an attacker to intercept the request and responses in secured channel ( SSL / TLS ).  This allows the attacker to get in the middle of the conversation between a client and server. They could be just eavesdropping on the conversation or could be changing the data as it moves to the client or server. This kind of attack is known as “Man in the Middle Attack” (MiTM).
You can read more about MiTM attack here (https://en.wikipedia.org/wiki/Man-in-the-middle_attack )

What is Certificate Pinning or Public key pinning?
                Certificate Pinning is most often used to address the scenario above, i.e. to keep unwanted eyes from looking into a mobile application’s traffic.
Certificate pinning is hardcoding or storing the information for digital certificates/public keys in a mobile application. In mobile application, the application knows what server they will connect to, so that the application can check for those specific certificates.

How to implement SSL Pinning?
Before going to start implementation of SSL pinning, there is few things which I want you to understand.
1.       SSL Handshake
http://www.pierobon.org/ssl/ch2/detail.htm

2.       X509Certificates & Certificate Authority
https://stormpath.com/blog/what-x509-certificate

3.       Public Key Encryption
http://www.onebigfluke.com/2013/11/public-key-crypto-math-explained.html
https://nrich.maths.org/2200

Let’s start implementing SSL pinning.
Pinning is done differently based on the mobile platform you’re using, and some variation is possible in actual implementation. OWASP has some good information on implementing for iOS and Android.

Now android application should aware about the server with which it is going to communicate. First step is to get the Root Certificate information so that we can pin/hardcode the certificate into code while in the development phase itself.

Let me tell you something about the Root certificate, leave certificate and intermediate certificates. During SSLhandshake, there is a Certificate chain received at client side. That certificate chain has a Root, leaf and intermediate certificates.

Root certificate: A certificate authority can issue multiple certificates in the form of a tree structure. A root certificate is the top-most certificate of the tree, the private key of which is used to "sign" other certificates. All certificates immediately below the root certificate inherit the trustworthiness of the root certificate.

Intermediate Certificate: We use intermediate certificates as a proxy because we must keep our root certificate behind numerous layers of security, ensuring its keys are absolutely inaccessible. However, because the root certificate itself signed the intermediate certificate, the intermediate certificate can be used to sign the SSLs our customers install and maintain the "Chain of Trust."

Leaf Certificate: A "leaf certificate" is what is more commonly known as end-entity certificate. Certificates come in chains, starting with the root CA, each certificate being the CA which issued (signed) the next one.


We will apply certificate pinning on Root certificate because leaf certificate has validity of shorter time span (1 or 2 Years) whereas Root certificate comparatively has much more validity time span. If we go for leaf certificate, then we need to update the android application more frequently. So for this reason, I will recommend to go for certificate pinning on Root Certificate.

Now,
Pinning Root certificate information to our android code has 2 approaches,
1.       Pin entire Root certificate
2.       Pin RSA public key from the Root certificate
I will follow the 2nd approach. There are no such significant points to decide which approach to follow.

We need to retrieve the RSA Public key and hardcode it in codebase. So whenever SSL request made to the server. Server will send all the certificates to the client. Client will fetch Root certificate from the list and retrieve RSA public key. Now if that RSA public key matches with the key hardcoded in code then we are sure that the communication happening with the right server. If that doesn’t match then probably there are chances of MiTM attack or Certificates changed. In this scenario, kill the connection and prompt the  user about the scenario.

You can find a sample program for Certificate pinning here

Is there a way to break certificate pinning?
An attacker would have to decompile the application, change the code, rebuild it and redeploy the application. Another option would be to run the application in a debugger.
For Android, you can obfuscate your code. You can also check to see if the application is running in a debugger. Code signing will also make it more difficult for an attacker to create an unauthorized patch for your application.
For Android, see Securing Android LVL Applications

Let me know if something unclear or need detailed understanding then do let me know. I will be happy to answer you...


Reference links:
https://www.owasp.org/index.php/Pinning_Cheat_Sheet
