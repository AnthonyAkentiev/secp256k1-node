# API Reference (v4.x)

- [`privateKey`]()
  - [`verify(Buffer privateKey)`]()
  - [`export(Buffer privateKey [, Boolean compressed = true])`]()
  - [`import(Buffer privateKey)`]()
  - [`tweakAdd(Buffer privateKey, Buffer tweak)`]()
  - [`tweakMul(Buffer privateKey, Buffer tweak)`]()
- [`publicKey`]()
  - [`create(Buffer privateKey [, Boolean compressed = true])`]()
  - [`convert(Buffer publicKey [, Boolean compressed = true])`]()
  - [`verify(Buffer publicKey)`]()
  - [`tweakAdd(Buffer publicKey, Buffer tweak [, Boolean compressed = true])`]()
  - [`tweakMul(Buffer publicKey, Buffer tweak [, Boolean compressed = true])`]()
  - [`combine(Array<Buffer> publicKeys [, Boolean compressed = true])`]()
- [`ecdsa`]()
  - [`signature`]()
    - [`normalize(Buffer signature)`]()
    - [`export(Buffer signature)`]()
    - [`import(Buffer signature)`]()
    - [`importLax(Buffer signature)`]()
  - [`sign(Buffer message, Buffer privateKey [, Function noncefn [, Buffer noncedata]])`]()
  - [`verify(Buffer signature, Buffer message, Buffer publicKey)`]()
  - [`recover(Buffer signature, Number recovery, Buffer message [, Boolean compressed = true])`]()
- [`schnorr`]()
  - [`sign(Buffer message, Buffer privateKey [, Function noncefn [, Buffer noncedata]])`]()
  - [`verify(Buffer signature, Buffer message, Buffer publicKey)`]()
  - [`recover(Buffer signature, Buffer message [, Boolean compressed = true])`]()
  - [`generateNoncePair(Buffer message, Buffer privateKey [, Function noncefn [, Buffer noncedata [, Boolean compressed]]])`]()
  - [`partialSign(Buffer message, Buffer privateKey, Buffer pubnonceOther, Buffer privnonce)`]()
  - [`partialCombine(Array<Buffer> signatures)`]()
- [`ecdh`]()
  - [`sha256(Buffer publicKey, Buffer privateKey)`]()
  - [`unsafe(Buffer publicKey, Buffer privateKey [, Boolean compressed = true])`]()

#####`.privateKeyVerify(Buffer privateKey)` -> `Boolean`

Verify an ECDSA *privateKey*.

<hr>

#####`.privateKeyExport(Buffer privateKey [, Boolean compressed = true])` -> `Buffer`

Export a *privateKey* in DER format.

<hr>

#####`.privateKeyImport(Buffer privateKey)` -> `Buffer`

Import a *privateKey* in DER format.

<hr>

#####`.privateKeyTweakAdd(Buffer privateKey, Buffer tweak)` -> `Buffer`

Tweak a *privateKey* by adding *tweak* to it.

<hr>

#####`.privateKeyTweakMul(Buffer privateKey, Buffer tweak)` -> `Buffer`

Tweak a *privateKey* by multiplying it by a *tweak*.

<hr>

#####`.publicKeyCreate(Buffer privateKey [, Boolean compressed = true])` -> `Buffer`

Compute the public key for a *privateKey*.

<hr>

#####`.publicKeyConvert(Buffer publicKey [, Boolean compressed = true])` -> `Buffer`

Convert a *publicKey* to *compressed* or *uncompressed* form.

<hr>

#####`.publicKeyVerify(Buffer publicKey)` -> `Boolean`

Verify an ECDSA *publicKey*.

<hr>

#####`.publicKeyTweakAdd(Buffer publicKey, Buffer tweak [, Boolean compressed = true])` -> `Buffer`

Tweak a *publicKey* by adding *tweak* times the generator to it.

<hr>

#####`.publicKeyTweakMul(Buffer publicKey, Buffer tweak [, Boolean compressed = true])` -> `Buffer`

Tweak a *publicKey* by multiplying it by a *tweak* value.

<hr>

#####`.publicKeyCombine(Array<Buffer> publicKeys [, Boolean compressed = true])` -> `Buffer`

Add a given *publicKeys* together.

<hr>

#####`.signatureNormalize(Buffer signature)` -> `Buffer`

Convert a *signature* to a normalized lower-S form.

<hr>

#####`.signatureExport(Buffer signature)` -> `Buffer`

Serialize an ECDSA *signature* in DER format.

<hr>

#####`.signatureImport(Buffer signature)` -> `Buffer`

Parse a DER ECDSA *signature* (follow by [BIP66](https://github.com/bitcoin/bips/blob/master/bip-0066.mediawiki)).

<hr>

#####`.signatureImportLax(Buffer signature)` -> `Buffer`

Same as [signatureImport](#signatureimportbuffer-signature---buffer) but not follow by [BIP66](https://github.com/bitcoin/bips/blob/master/bip-0066.mediawiki).

<hr>

#####`.sign(Buffer message, Buffer privateKey [, Object options])` -> `{signature: Buffer, recovery: number}`

Create an ECDSA signature. Always return low-S signature.

Inputs: 32-byte message m, 32-byte scalar key d, 32-byte scalar nonce k.

* Compute point R = k * G. Reject nonce if R's x coordinate is zero.
* Compute 32-byte scalar r, the serialization of R's x coordinate.
* Compose 32-byte scalar s = k^-1 \* (r \* d + m). Reject nonce if s is zero.
* The signature is (r, s).

######Option: `Function noncefn`

Nonce generator. By default it is [rfc6979](https://tools.ietf.org/html/rfc6979).

Function signature: `noncefn(Buffer message, Buffer privateKey, ?Buffer algo, ?Buffer data, Number attempt)` -> `Buffer`

######Option: `Buffer data`

Additional data for [noncefn](#option-function-noncefn) (RFC 6979 3.6) (32 bytes). By default is `null`.

<hr>

#####`.verify(Buffer message, Buffer signature, Buffer publicKey)` -> `Boolean`

Verify an ECDSA signature.

Note: **return false for high signatures!**

Inputs: 32-byte message m, public key point Q, signature: (32-byte r, scalar s).

* Signature is invalid if r is zero.
* Signature is invalid if s is zero.
* Compute point R = (s^-1 \* m \* G + s^-1 \* r \* Q). Reject if R is infinity.
* Signature is valid if R's x coordinate equals to r.

<hr>

#####`.recover(Buffer message, Buffer signature, Number recovery [, Boolean compressed = true])` -> `Buffer`

Recover an ECDSA public key from a signature.

<hr>

#####`.ecdh(Buffer publicKey, Buffer privateKey)` -> `Buffer`

Compute an EC Diffie-Hellman secret and applied sha256 to compressed public key.

<hr>

#####`.ecdhUnsafe(Buffer publicKey, Buffer privateKey [, Boolean compressed = true])` -> `Buffer`

Compute an EC Diffie-Hellman secret and return public key as result.
