---
layout: post
title:  "Protect sensitive information or credentials using Android Keystore"
author: prashant
featured: false
image: assets\images\android-keystore.jpg
categories: [ coding, development, android, security ]
---

The **Android keystore** provides secure system level credential storage. With the keystore, an application creates a new Private/Public key pair, and uses this to encrypt application secrets before saving it in the private storage. We will learn how to use Android keystore to create and delete keys also how to encrypt the user sensitive data using these keys.
The Keystore system is used by the KeyChain API as well as the Android Keystore provider feature that was introduced in Android 4.3 (API level 18). This document goes over when and how to use the Android Keystore provider

Android has had a system-level credential storage since Donut (1.6). Up until ICS (4.0), it was only used by the VPN and WiFi connection services to store private keys and certificates, and a public API was not available. ICS introduced a public API  and integrated the credential storage with the rest of the OS.

------------

**Why to use Keystore?**
There are many situations where we use encryption in code, also some situations where we need to save some tokens or keys, Android keystore is the best approach to implement.

#### Benefits
- Android Keystore system protects key material from unauthorized use.
- As the secure storage of cryptographic keys was concerned, Google decided to introduce a standardised way for apps to store credentials, in the hope of adding some order to the mobile cryptographic chaos.
- Better yet, the KeyStore implementation, significantly enhanced in Android 4.3, automatically ensures that each app’s KeyStore data is strongly encrypted with its own unique key as well as a key derived from your lockscreen passcode, and uses special-purpose hardware for key storage if it is available (as it is on the Nexus 7, for example).


**How to use Android Keystore Provider?**
Before we start, lets understand something about Android keystore and its capabilities. The keystore is not used directly for storing application secrets such as Password, however, it provides a secure container, which can be used by apps to store their private keys, in a way that’s pretty difficult for unauthorized users and apps to retrieve.

####Add a new key to the Keystore:
- Each key created by an app should have unique alias which can be any string.
- We use a KeyPairGeneratorSpec object to build specifications of our required key.
- We can set validity period of the key, alias and subject.
- Each key is bound to the UID of the process that created it, so that apps cannot access each other's keys or the system ones. Additionally, even system apps cannot see app keys, and root is explicitly prohibited from creating or listing keys.
- The subject must be an X500Principal object that resolves to a String of the form “CN=Common Name, O=Organization, C=Country”.
- To generate a  public/private keypair,  we need a KeyPairGenerator object.
- We get an instance of KeyPairGenerator set to use the RSA algorithm with the “AndroidKeyStore”. Calling generateKeyPair() creates the new pair of keys (Private and corresponding Public key), and adds it to the Keystore.          

```
public void createNewKeys(String alias) {

  try {
      // Create new key if needed
      if (!keyStore.containsAlias(alias)) {
         Calendar start = Calendar.getInstance();
         Calendar end = Calendar.getInstance();
         end.add(Calendar.YEAR, 1);
         KeyPairGeneratorSpec spec = new KeyPairGeneratorSpec.Builder(this)
                        .setAlias(alias)
                        .setSubject(new X500Principal("CN=Sample Name, O=Android Authority"))
                        .setSerialNumber(BigInteger.ONE)
                        .setStartDate(start.getTime())
                        .setEndDate(end.getTime())
                        .build();
                KeyPairGenerator generator = KeyPairGenerator.getInstance("RSA", "AndroidKeyStore");
                generator.initialize(spec);

                KeyPair keyPair = generator.generateKeyPair();
            }
        } catch (Exception e) {
            Toast.makeText(this, "Exception " + e.getMessage() + " occured", Toast.LENGTH_LONG).show();
            Log.e(TAG, Log.getStackTraceString(e));
        }
        refreshKeys();
    }
```

####Delete key from the Keystore:
Deleting a key from the keystore is pretty straightforward.
Call keystore.deleteEntry(keyAlias). There is no way to restore a deleted key, so you may want to be certain before using this.

```
public void deleteKey(final String alias) {

        AlertDialog alertDialog =new AlertDialog.Builder(this)
                .setTitle("Delete Key")
                .setMessage("Do you want to delete the key \"" + alias + "\" from the keystore?")
                .setPositiveButton("Yes", new DialogInterface.OnClickListener() {
                    public void onClick(DialogInterface dialog, int which) {
                        try {
                            keyStore.deleteEntry(alias);
                            refreshKeys();
                        } catch (KeyStoreException e) {
                            Toast.makeText(MainActivity.this, "Exception " + e.getMessage() + " occured",Toast.LENGTH_LONG).show();
                            Log.e(TAG, Log.getStackTraceString(e));
                        }
                        dialog.dismiss();
                    }
                })
                .setNegativeButton("No", new DialogInterface.OnClickListener() {
                    public void onClick(DialogInterface dialog, int which) {
                        dialog.dismiss();
                    }
                })
                .create();
        alertDialog.show();
    }
```

####Encrypt the block of Text:
-  Encrypting a block of text is performed with the Public key of the Key Pair.
-  We retrieve the Public Key, request for a Cipher using our preferred encryption/decryption transformation (“RSA/ECB/PKCS1Padding”), then initialize the cipher to perform encryption (Cipher.ENCRYPT_MODE) using the retrieved Public Key.
-  Cipher operates on (and returns) a byte []. We wrap the Cipher in a CipherOutputStream, along with a ByteArrayOutputStream to handle the encryption complexities. The result of the encryption process is then converted to a Base64 string for display.

```
public void encryptString(String alias) {
        try {
            KeyStore.PrivateKeyEntry privateKeyEntry = (KeyStore.PrivateKeyEntry)keyStore.getEntry(alias, null);
            RSAPublicKey publicKey = (RSAPublicKey) privateKeyEntry.getCertificate().getPublicKey();

            // Encrypt the text
            String initialText = startText.getText().toString();  // Secret text
            if(initialText.isEmpty()) {
                Toast.makeText(this, "Enter text in the 'Initial Text' widget", Toast.LENGTH_LONG).show();
                return;
            }

            Cipher input = Cipher.getInstance("RSA/ECB/PKCS1Padding", "AndroidOpenSSL");
            input.init(Cipher.ENCRYPT_MODE, publicKey);

            ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
            CipherOutputStream cipherOutputStream = new CipherOutputStream(
                    outputStream, input);
            cipherOutputStream.write(initialText.getBytes("UTF-8"));
            cipherOutputStream.close();

            byte [] vals = outputStream.toByteArray();
            encryptedText.setText(Base64.encodeToString(vals, Base64.DEFAULT));
        } catch (Exception e) {
            Toast.makeText(this, "Exception " + e.getMessage() + " occured", Toast.LENGTH_LONG).show();
            Log.e(TAG, Log.getStackTraceString(e));
        }
    }

```

####Decryption
Decryption is basically the Encryption process in reverse. Decryption is done using the Private Key of the key pair. We then initialize a Cipher with the same transformation algorithm used for encryption, but set to Cipher.DECRYPT_MODE. The Base64 string is decoded to a byte[], which is then placed in a ByteArrayInputStream. We then use a CipherInputStream to decrypt the data into a byte[]. This is then displayed as a String.

```
public void decryptString(String alias) {
        try {
            KeyStore.PrivateKeyEntry privateKeyEntry = (KeyStore.PrivateKeyEntry)keyStore.getEntry(alias, null);
            RSAPrivateKey privateKey = (RSAPrivateKey) privateKeyEntry.getPrivateKey();

            Cipher output = Cipher.getInstance("RSA/ECB/PKCS1Padding", "AndroidOpenSSL");
            output.init(Cipher.DECRYPT_MODE, privateKey);

            String cipherText = encryptedText.getText().toString();
            CipherInputStream cipherInputStream = new CipherInputStream(
                    new ByteArrayInputStream(Base64.decode(cipherText, Base64.DEFAULT)), output);
            ArrayList<Byte> values = new ArrayList<>();
            int nextByte;
            while ((nextByte = cipherInputStream.read()) != -1) {
                values.add((byte)nextByte);
            }

            byte[] bytes = new byte[values.size()];
            for(int i = 0; i < bytes.length; i++) {
                bytes[i] = values.get(i).byteValue();
            }

            String finalText = new String(bytes, 0, bytes.length, "UTF-8");
            decryptedText.setText(finalText);

        } catch (Exception e) {
            Toast.makeText(this, "Exception " + e.getMessage() + " occured", Toast.LENGTH_LONG).show();
            Log.e(TAG, Log.getStackTraceString(e));
        }
    }
```


> The Android Keystore system lets you store cryptographic keys in a container to make it more difficult to extract from the device. Once keys are in the keystore, they can be used for cryptographic operations with the key material remaining non-exportable. Moreover, it offers facilities to restrict when and how keys can be used, such as requiring user authentication for key use or restricting keys to be used only in certain cryptographic modes.
— The Google docs


Thank you.

Happy Coding.. !!
