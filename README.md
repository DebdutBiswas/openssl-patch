# openssl-patch

## OpenSSL Equal Preference Patch

### This file is not an official OpenSSL patch. Problems can arise and this is your responsibility.

- [Test Page - (TLS 1.3 draft 23, 28)](https://ssl.hakase.io/)
- [Result check testssl.sh](https://ssl.hakase.io/ssltest/hakase.io.html)
- **If you link site to a browser that supports draft 23 or 28, you'll see a TLS 1.3 message.**

**Support TLS 1.3 draft 28 browsers - _Chrome Canary, Firefox Nightly_**

**Latest patch : openssl-equal-pre8.patch, openssl-equal-pre8_ciphers.patch**

[View Tree (OpenSSL)](https://github.com/openssl/openssl/tree/5eb774324a14b03835020bb3ae2e1c6c92515db0)

[Original source](https://boringssl.googlesource.com/boringssl/+/858a88daf27975f67d9f63e18f95645be2886bfb%5E%21) by [BoringSSL](https://github.com/google/boringssl) & [buik](https://gitlab.com/buik/openssl/blob/openssl-patch/openssl-1.1/OpenSSL1.1h-equal-preference-cipher-groups.patch)

OpenSSL 1.1.0h patch is [here](https://gitlab.com/buik/openssl/blob/openssl-patch/openssl-1.1/OpenSSL1.1h-equal-preference-cipher-groups.patch)

## Patch files

Here is the basic patch content.
- Support TLS 1.3 draft 23 + 28
    - Server: draft 23 + 28
    - Client: draft 23 + 26 + 27 + 28
- BoringSSL's Equal Preference Patch
- Weak 3DES and not using ECDHE ciphers is not used in TLSv1.1 or later.

| Patch file name | Patch list |
| :--- |  :--- |
| openssl-equal-pre7.patch | TLS 1.3 cipher settings **_can not_** be changed on _nginx_. |
| openssl-equal-pre7_ciphers.patch | TLS 1.3 cipher settings **_can_** be changed on _nginx_. |
| openssl-equal-pre8.patch | TLS 1.3 cipher settings **_can not_** be changed on _nginx_. |
| openssl-equal-pre8_ciphers.patch | TLS 1.3 cipher settings **_can_** be changed on _nginx_. |

**The "_ciphers" patch file is a temporary change to the TLS 1.3 configuration.**

Example of setting TLS 1.3 cipher in nginx:
- ex 1. TLS13+AESGCM+AES128:TLS13+AESGCM+AES256:TLS13+CHACHA20
- ex 2. TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256
- ex 3. TLS13+AESGCM+AES128:EECDH+AES128 (TLS 1.3 + TLS 1.2 ciphers)

## nginx Configuration (ssl_ciphers)

### Default settings
```
ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
ssl_ciphers [Copy it from below and paste it here.];
ssl_ecdh_curve X25519:P-256:P-384;
ssl_prefer_server_ciphers on;
```

### OpenSSL-1.1.1-pre7, pre8 ciphers (draft 23, 28)
```
[EECDH+ECDSA+AESGCM+AES128|EECDH+ECDSA+CHACHA20]:EECDH+ECDSA+AESGCM+AES256:EECDH+ECDSA+AES128+SHA:EECDH+ECDSA+AES256+SHA:[EECDH+aRSA+AESGCM+AES128|EECDH+aRSA+CHACHA20]:EECDH+aRSA+AESGCM+AES256:EECDH+aRSA+AES128+SHA:EECDH+aRSA+AES256+SHA:RSA+AES128+SHA:RSA+AES256+SHA:RSA+3DES
```

### OpenSSL-1.1.1-pre7_ciphers, pre8_ciphers ciphers (draft 23, 28)
```
[TLS13+AESGCM+AES128|TLS13+AESGCM+AES256|TLS13+CHACHA20]:[EECDH+ECDSA+AESGCM+AES128|EECDH+ECDSA+CHACHA20]:EECDH+ECDSA+AESGCM+AES256:EECDH+ECDSA+AES128+SHA:EECDH+ECDSA+AES256+SHA:[EECDH+aRSA+AESGCM+AES128|EECDH+aRSA+CHACHA20]:EECDH+aRSA+AESGCM+AES256:EECDH+aRSA+AES128+SHA:EECDH+aRSA+AES256+SHA:RSA+AES128+SHA:RSA+AES256+SHA:RSA+3DES
```
