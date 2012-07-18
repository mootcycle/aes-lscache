aes-lscache
===============================
This is a fork of the excellent lscache module to add AES encryption to the local store. For most of the documentation, please refer to the original repo here:
https://github.com/pamelafox/lscache

I'm using Gibberish-AES to provide the encryption, which is available here:
https://github.com/mdp/gibberish-aes/

This is mostly an experiment based on my thoughts about Nicholas Zakas's blog post on the need for secure local storage:
http://www.nczonline.net/blog/2010/04/13/towards-more-secure-client-side-data-storage/

Additional Methods
-------

### lscache.setEncryptionKey
Sets the passphrase that will be used to encrypt/decrypt the data.
#### Arguments
1. `key` (**string**)

* * *

### lscache.clearEncryptionKey
Removes the encryption key and clears the encryption cache.

Usage
-------

This is supposed to work identically to lscache, but with the added (very simple) encryption.

```js
lscache.setEncryptionKey("myVeryStrongPassword");
lscache.set('greeting', 'Hello World!', 2);
```

After a browser refresh:

```js
console.log(lscache.get('greeting'));
// null
lscache.setEncryptionKey("myVeryStrongPassword");
lscache.get('greeting');
// "Hello World!"
```

Looking at localStorage shows that we're not leaking variable or bucket names and the values are encrypted:
```js
console.log(Object.keys(localStorage)[0]);
// "U2FsdGVkX1+ePtMnRyd4XGMgT+6PPHFJwflMAPAm908=\nU2FsdGVkX19DJPfXBhkHNWqOe4eZjjU6jqbRH9Nssjg=\nU2FsdGVkX1+wCOwnYu8kW5cL3PtLDfuM6ZbcqIqzFB9orHCIR13okZxEWYBf7HzG\n"
console.log(localStorage["U2FsdGVkX1+ePtMnRyd4XGMgT+6PPHFJwflMAPAm908=\nU2FsdGVkX19DJPfXBhkHNWqOe4eZjjU6jqbRH9Nssjg=\nU2FsdGVkX1+wCOwnYu8kW5cL3PtLDfuM6ZbcqIqzFB9orHCIR13okZxEWYBf7HzG\n"]);
// U2FsdGVkX18GFlWv+0KCPf9cOl3V5Ccm0fR7tDp1NJI=\n
```

