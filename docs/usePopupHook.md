## usePopupHook

_[https://developer.yoco.com/online/popup/reference](https://developer.yoco.com/online/popup/reference)_

The full list of acceptable config options for the `showPopup` method are:

```tsx
interface YocoPopupConfig {
  id?: string;
  callback: (result: YocoCheckoutResult) => void;
  onClose?: () => void;
  amountInCents?: number;
  currency?: Currency;
  YocoCustomer?: string;
  'YocoCustomer.email'?: string;
  'YocoCustomer.phone'?: string;
  'YocoCustomer.firstName'?: string;
  'YocoCustomer.lastName'?: string;
  description?: string;
  image?: string;
  metadata?: object;
  name?: string;
  paymentMethod?: string;
  paymentType?: 'CARD';
}
```

The callback, passed in as the second parameter, will receive a `YocoCheckoutResult` upon completion, whose type definition is:

```tsx
interface YocoCheckoutResult {
  id?: string;
  error?: {
    data: {
      message: string;
      status: number;
    };
    message: string;
  };
  paymentMethod: string | undefined;
  source: {
    card: {
      expiryMonth: number;
      expiryYear: number;
      maskedCard: string;
      scheme: string;
      usable: string;
    };
  };
  status: string;
}
```

The `stories` directory contains additional example usage you may reference.

