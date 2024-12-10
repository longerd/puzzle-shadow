# puzzle-shadow

**DO NOT FORK THE REPOSITORY, AS IT WILL MAKE YOUR SOLUTION PUBLIC. INSTEAD, CLONE IT AND ADD A NEW REMOTE TO A PRIVATE REPOSITORY, OR SUBMIT A GIST**

Trying it out
=============


## Setup

### Noir
Install Noir v0.37.0.

```
curl -L noirup.dev | bash
noirup --version 0.37.0
```

### Proving Backend
Install the compatible version of the `bb` CLI for the Barretenberg proving backend.

```
curl -L bbup.dev | bash
bbup --version 0.61.0
```

## Generate the witness & proof

Generate the witness:   
```
nargo execute
```

Generate the proof:
```
bb prove -b ./target/puzzle3.json -w ./target/puzzle3.gz -o ./target/proof
```

Verify the proof:
```
bb verify -p ./target/proof
```

## (For documentation only, no need for the puzzle) - Generate the circuit and vk

Compile:
```
nargo compile
```

Generate the verification key:
```
bb write_vk -b ./target/puzzle3.json -o ./target/vk
```

Submitting a solution
=====================

[Submit a solution](https://form.typeform.com/to/Iljb6IGi)

[Submit a write-up](https://form.typeform.com/to/nssigeCO)

Puzzle description
==================

```
    ______ _   __  _   _            _
    |___  /| | / / | | | |          | |
       / / | |/ /  | |_| | __ _  ___| | __
      / /  |    \  |  _  |/ _` |/ __| |/ /
    ./ /___| |\  \ | | | | (_| | (__|   <
    \_____/\_| \_/ \_| |_/\__,_|\___|_|\_\

A cutting-edge crypto company unveiled http://JWT.pk, a revolutionary identity 
infrastructure platform designed to simplify private key management. By allowing 
users to register seamlessly with existing keys, the service promised to redefine convenience and 
security in the digital world. It allows users to send amounts up to $100 without having access to their 
private keys.

The registration process goes as follows: Users sign up with their existing 
public keys (pk) http://JWT.pk then samples a secret value called “pepper” and computes an identifier 
of the form: “{pk}_{pepper}”. SHA256 of the identifier is then added to the set of registered identifiers.

Users can later authenticate a transaction by simply providing a proof 
of knowledge of the whitelisted identifier formed by the (pk, pepper) pair without needing to use 
their private key at any step of the process.

Alice found out that Bob registered to http://JWT.pk with a 
public key “BOB_pk”. She then registered using “ALICE_pk” and 
obtained “ALICE_pepper” from http://JWT.pk.

Bob woke up one morning to see his account drained. How did Alice do it?
```


