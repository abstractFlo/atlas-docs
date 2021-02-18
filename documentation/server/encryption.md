---
description: Learn more about Encryption
---

# Encryption

## Introduction

We have a simple encryption service powered by [sjcl](http://bitwiseshiftleft.github.io/sjcl/). We're using it to create secure tokens for the Discord auth module. If you want, you can use it too. We only implement encryption _**no decryption**_.

## Base Usage

```typescript
export class MyComponent {

    // Persistent SHA256 Hash
    public myMethod(): void {
        const tokenData = 'myAwesomeDataToBeEncrypted';
        const encryptedToken = EncryptionService.sha256(tokenData);
        // Now inside encryptedToken lives your encrypted data.
    }

    // Random SHA256 Hash
    public myOtherMethod(): void {
        const tokenData = 'myAwesomeDataToBeEncrypted';
        const encryptedToken = EncryptionService.sha256Random(tokenData);
        // Now inside encryptedToken lives your encrypted data.
    }

}
```

