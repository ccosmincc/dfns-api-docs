# Request Signing

All mutating requests need to be signed with a user/token credential.

## Private Key Credentials

When signing with a private key you need to:

1. Get a signing challenge from the Dfns API
2. Sign the challenge
3. Exchange the signed challenge for a user action signature with the Dfns API
4. Complete the original request

### The Signing Challenge

A signing challenge is returned from a call to:

* /auth/action/init

You will receive an object with the following properties (additional properties exist for signing with WebAuthn):

| field                | description                                                       |
| -------------------- | ----------------------------------------------------------------- |
| challenge            | A string that will be signed with your private key                |
| challengeIdentifier  | A JWT that identifies the signing session                         |
| allowCredentials.key | The list of private key credentials that are enabled for the user |

### How to Sign the Challenge with the Private Key

The user signs the challenge to verify they want to perform the requested action.&#x20;

The user needs to format the challenge into a[ Client Data object](api-objects.md#client-data).&#x20;

After creating this object, the user will convert the object to a JSON string and sign the string.

When returning the signature to the server, the user will base64url encode the signature and the client data along with the ID of the credential that was used.

#### Signing Example

```typescript
const signChallenge = (challenge: UserActionSignatureChallenge) => {
  /*
  challenge.allowCredentials.key is an array of registered credentials. If you have
  more than one Key credential, you may need to use challenge.allowCredentials.key[N].id to locate
  the key you're using. For example, you could have made your ID the base64url encoded name of the key on disk or in AWS KMS.

  In this example, we assume the user registered a single Key credential.
  */ 
  const clientData = { type: 'key.get', challenge: challenge.challenge }
  
  const clientDataBytes: Buffer = Buffer.from(JSON.stringify(clientData))

  const signature = crypto.sign(undefined, clientDataBytes, apiKeyPrivateKey)

  return {
    credId: challenge.allowCredentials.key[0].id,
    clientData: clientData.toString('base64url'),
    signature: signature.toString('base64url'),
  }
}
```
