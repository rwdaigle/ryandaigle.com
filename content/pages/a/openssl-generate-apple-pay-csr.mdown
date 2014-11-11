Title: Generate an Apple Pay CSR with OpenSSL
Date: November 6, 2014

When creating an [Apple Pay](https://www.apple.com/apple-pay/) certificate signing request, Apple specifies that you need to use a 256 bit elliptic curve key pair. To generate both the private key and the CSR using the `openssl` command line utility, do the following:

<pre><code lang="bash">
$ openssl ecparam -out private.key -name prime256v1 -genkey
$ openssl req -new -sha256 -key private.key -nodes -out request.csr -subj '/O=Your Name or Company/C=US'
</code></pre>

The result will be a private key file at `private.key` and the CSR in `request.csr`. They should look something like this.

<div class="file">private.key</div>
<pre><code lang="text">
-----BEGIN EC PARAMETERS-----
BgUrgQQACg==
-----END EC PARAMETERS-----
-----BEGIN EC PRIVATE KEY-----
MHQCAQEEID0Y+YLOz3ed+dMlh047WSwgxl3a0WVI4en3tjntAdwooAcGBSuBBAAK
oUQDQgAEwHGnT+kCI+oqFK8ALEZzBcqHC+QNwmCLQHx51zCT51TpZEIufTFpac3a
E5sNqznV2Dp39N0wVCAV7QPGI6SXvg==
-----END EC PRIVATE KEY-----
</code></pre>

<div class="file">request.csr</div>
<pre><code lang="text">
-----BEGIN CERTIFICATE REQUEST-----
MIH3MIGgAgEAMEExGTAXBgNVBAMTEHd3dy5zcHJlZWRseS5jb20xFzAVBgNVBAoT
DlNwcmVlZGx5LCBJbmMuMQswCQYDVQQGEwJVUzBWMBAGByqGSM49AgEGBSuBBAAK
A0IABMBxp0/pAiPqKhSvACxGcwXKhwvkDcJgi0B8edcwk+dU6WRCLn0xaWnN2hOb
Das51dg6d/TdMFQgFe0DxiOkl76gADAJBgcqhkjOPQQBA0cAMEQCIBGy+OBbsjey
lQhqezpSRt+IKfMMLdA78Pnck3fWIVxcAiBOYX1hmOREEysFQq0eX309iY0uZ3dm
MRDa/83lW8GcZQ==
-----END CERTIFICATE REQUEST-----
</code></pre>

You will need the private key to decrypt the Apple Pay cryptogram so keep it in a secure place. The CSR you will then upload to Apple, which will sign the certificate for you to use in your iOS app.

Normally, your payment provider will generate the private key (which they will retain) and CSR for you, so doing this yourself won't be necessary. However, if you're a payment provider like [Spreedly](https://spreedly.com) or are interested in managing more of the payment flow, this may be useful.

A few notes:

* Use the `prime256v1` elliptic curve when generating your private key as this is what's used by Apple's Keychain app when generating an EC key.
* Apple will add/overwrite the UID, CN (common name) and OU (organizational unit) certificate fields with the associated merchant identifier so your CSR doesn't need to specify them.
