# Kinetic TypeScript SDK for CDNs

> Note: This build of the Kinetic TypeScript SDK is experimental.

## Usage

Include the following script tag in your HTML:

```html
<script
  src="https://unpkg.com/@kinetic-cdn/build/kinetic-sdk.js"
  type="module"
></script>
```

Next, listen for the `kinetic-sdk-ready` event to know when the SDK is ready to use:

```js
document.addEventListener(
  "kinetic-sdk-ready",
  ({ detail: { Keypair, KineticSdk } }) => {
    console.log("Kinetic SDK is ready!", Keypair, KineticSdk);
    // Do stuff with the Keypair class;
    // Do stuff with the KineticSdk class;
  }
);
```

## Example

A more complete example that sets up the Kinetic Sdk, and creates a keypair in local storage.

> ⚠️ Warning: this code is not ready for production, sharing the user secret in local storage is a security risk.

> ⚠️ PLEASE BE AWARE OF THE SECURITY IMPLICATIONS OF THIS CODE.

```js
function setupKeypair(Keypair) {
  let publicKey = localStorage.getItem("publicKey");
  if (!publicKey) {
    const keypair = Keypair.random();
    publicKey = keypair.publicKey;
    console.log("No public key found, generating a new one", keypair.publicKey);
    localStorage.setItem("publicKey", keypair.publicKey);
    localStorage.setItem(keypair.publicKey, keypair.mnemonic);
  }

  window["owner"] = Keypair.fromMnemonic(localStorage.getItem(publicKey));
}

function setupSdk(KineticSdk) {
  KineticSdk.setup({
    endpoint: "https://sandbox.kinetic.host",
    environment: "devnet",
    index: 1,
    logger: console,
  }).then((sdk) => {
    window["sdk"] = sdk;
  });
}

document.addEventListener(
  "kinetic-sdk-ready",
  ({ detail: { Keypair, KineticSdk } }) => {
    console.log("Kinetic SDK is ready!", Keypair, KineticSdk);
    setupKeypair(Keypair);
    setupSdk(KineticSdk);
  }
);
```
