# React hooks for Yoco Gateway

_**v2.0.0 🎉 - Now with EFT.**_

React hooks and components for accepting card and EFT payments in your React application with [Yoco Gateway](https://www.yoco.com/za/yoco-gateway/).

![Yoco payments banner](./docs/yoco-banner-1544x500.png)

## Installation

```
yarn add @yoco/yoco-react
```
## Getting Started

The [Yoco Popup](https://developer.yoco.com/online/popup/popup) offers you the quickest and easiest way to accept online card payments.

You can use the `usePopup` hook to present the payment form in your React application.

The hook returns a `showPopup` method used to trigger the pop-up. It also returns variable `isYocoReady` for use in determining whether the Yoco SDK is loaded and ready or not.

The hook itself accepts two parameters:
- Your Yoco public key. Learn how to get one [on the Yoco website](https://developer.yoco.com/online/resources/integration-keys).
- The payment ID for the checkout that will have been initiated with a server-to-server call on the backend. Learn more [about payment initiation](https://deploy-preview-38--modest-shannon-b4f7f0.netlify.app/blackbird/sdk/accept-payments#2-initiate-a-payment).

🛠️ Use [the Postman collection](./docs/YocoBlackbirdv1.0.0.postman_collection.json) to quickly create test payments.

The `showPopup` method expects a `callback`. This callback will receive the `YocoCheckoutResult` when the pop-up completes processing a payment.
Additionally, it requires an `onClose` method to be invoked when a user dimisses the pop-up.
Finally, it will expect a `currency` and an `amountInCents`.

Here's an example implementation:

```tsx
import React, { FC }  from 'react';
import { usePopup } from '@yoco/yoco-react';

const PopupExample: FC = () => {
  const [showPopup, isYocoReady] = usePopup('your_public_key', 'a_valid_payment_id');

  async function callback(result: YocoCheckoutResult) {
    if (result.error) {
      // handle failure
      // result.error.message - Error message
      // result.error.status  - Error status
      return;
    }
    // result.id has the payment's id for verification
  }

  async function onClose() {
    // handle pop-up dismissal
  }

  async function onSubmit() {
    showPopup({ amountInCents: 1000, callback, currency: 'ZAR', onClose });
  }

  return (
    <button disabled={!isYocoReady} onClick={onSubmit}>Pay</button>
  );
}
```

You can view the full list of configuration options for the showPopup method in the [usePopupHook reference](./docs/usePopupHook.md).

### Verifying successful payments
The `result` will have a value of "succeeded" in the `status` property for a successful payment.
It also has an `id` that you can use for verifying a payment with a server-to-server call.
Learn how to use the API in the [official guide](https://deploy-preview-38--modest-shannon-b4f7f0.netlify.app/blackbird/sdk/save-card-during-payment#6-optional-verify-the-payment-succeeded).

The [Postman collection](./docs/YocoBlackbirdv1.0.0.postman_collection.json) has a payment verification helper.

### useYoco hook

If all you need is the base Yoco SDK without the hooks and components, the library exposes a `useYoco` hook.

```tsx
import { useYoco } from '@yoco/yoco-react';

...

const yocoSDK = useYoco(publicKey);
```

You can then use the SDK as normal according to the official docs. It is the same hook used internally by the rest of the hooks in this library.

The hook expects your Yoco public key, and optionally a payment ID depending on your use case.

You may also use the `useYocoLegacy` hook to get the previous generation SDK (https://js.yoco.com/sdk/v1/yoco-sdk-web.js). This may be useful if you prefer to use [the inline checkout experience](https://developer.yoco.com/online/inline/inline) which is currently not available in v2.0.0.

## Accepting EFT payments
You can easily accept instant EFT payments in your React application with the `useEFT` hook.
The hook takes a public key as the only parameter and returns a `showEFT` method and a boolean variable `isYocoReady`.

To display an EFT pop-up, call the `showEFT` method with the `id` of the payment you would have prepared in a server-to-server call.

```tsx
import React from 'react';
import { useEFT } from '@yoco/yoco-react';

const App = () => {
  const [showEFT, isYocoReady] = useEFT('your_public_key');

  async function onSubmit() {
    try {
      const result = await showEFT({ id: 'your_payment_id' });
      if (result.error) {
        // handle failure
        // result.error.message - Error message
        // result.error.status  - Error status
        return;
      }
      if (result.status === "succeeded") {
        // handle success for result.id
        return;
      }
      // handle payment statuses that are not successes.
      // for example, the user can dismiss the EFT pop-up
      // resulting in a status of "cancelled"
    } catch (err) {
      // handle general failures
    }
  }

  return (
    <button disabled={!isYocoReady} onClick={onSubmit}>Pay</button>
  );
};
```

## Inline Checkouts
[Inline checkouts](https://developer.yoco.com/online/inline/inline) will be available in a future release.

# Playground
The playground is a quick way to try out the different hooks:
```
cd example && yarn start
```

See [CONTRIBUTING.md](./CONTRIBUTING.md) for more.

# Support

If you have any questions, feedback or you are experiencing issues integrating, please contact developers@yoco.com.
