## Puzzle description

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

## Solution

[main.nr](./src/main.nr) 代码中实现了电路逻辑: 

```rust
fn main(identifier: BoundedVec<u8, 128>, pub_key: pub [u8; 32], whitelist: pub [[u8; 32]; 10]) {
    // the identifier hashes to a digest that is in the public whitelist
    let digest = std::hash::sha256_var(identifier.storage(), identifier.len() as u64);
    let mut present = false;
    for i in 0..whitelist.len() {
        if whitelist[i] == digest {
            present = true;
        }
    }
    assert(present);

    // the specified public key is in the identifier
    let id_haystack: StringBody128 = StringBody::new(identifier.storage(), 128);
    let pk_needle: SubString32 = SubString::new(pub_key, 32);
    let (result, _): (bool, u32) = id_haystack.substring_match(pk_needle);
    assert(result);
}
```

代码检查两方面:
* identifier 的 sha256 hash 在白名单里
* pub_key 在 identifier 里

identifier 中需要包括三个信息:
* Alice 的 pub_key
* Alice 的 pepper 
* Bob 的 pub_key

**sha256_var** 可以将 hash length 作为参数传入以 hash 指定长度的数据. 
所以 identifier 可以为 [32B alice pub_key, b'_', 32B alice pepper, 32B bob pub_key, 31B zeros], len = "65".

## Ref

* https://zkhack.dev/zkhackV/puzzleV3.html
* https://zkhack.dev/zkhackV/solutionV3.html
* https://learnblockchain.cn/article/10289


