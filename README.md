# paypal-nvp-api
Node.js wrapper for the Paypal Name-Value Pair — NVP

[![NPM](https://badge.fury.io/js/paypal-nvp-api.svg)](https://badge.fury.io/js/paypal-nvp-api)
![Travis](https://travis-ci.org/ndaidong/paypal-nvp-api.svg?branch=master)
[![Coverage Status](https://coveralls.io/repos/github/ndaidong/paypal-nvp-api/badge.svg?branch=master)](https://coveralls.io/github/ndaidong/paypal-nvp-api?branch=master)
![devDependency Status](https://david-dm.org/ndaidong/paypal-nvp-api.svg)
[![Known Vulnerabilities](https://snyk.io/test/npm/paypal-nvp-api/badge.svg)](https://snyk.io/test/npm/paypal-nvp-api)


# Usage

Import module and init an instance with given config:

```
var Paypal = require('paypal-nvp-api');

let config = {
  mode: 'sandbox', // or 'live'
  track: 'https://www.sandbox.paypal.com', // or 'https://www.paypal.com' if live
  username: 'someone.itravellocal.com',
  password: 'DYKNJZZE42ASN699',
  signature: 'A0aEilikhBmwfK.NlduDjCbsdgXdA8VDPMDksDhGsHmLQECu80Qtru09'
}

let paypal = Paypal(config);
```

Build query and send to Paypal server:

```
paypal.request('GetBalance', {}).then((result) => {
  console.log(result);
}).catch((err) => {
  console.trace(err);
});
```

The 'result' looks like this:

```
{
  L_AMT0: '71084.27',
  L_CURRENCYCODE0: 'USD',
  TIMESTAMP: '2016-03-18T09:48:16Z',
  CORRELATIONID: 'e3de6137ce65c',
  ACK: 'Success',
  VERSION: '124',
  BUILD: '18316154'
}
```

Another example with "SetExpressCheckout" operation:

```

let query = {
  'PAYMENTREQUEST_0_AMT': '20.00',
  'PAYMENTREQUEST_0_CURRENCYCODE': 'USD',
  'PAYMENTREQUEST_0_PAYMENTACTION': 'Sale',
  'RETURNURL': 'http://google.com/test/onreturn',
  'CANCELURL': 'http://google.com/test/oncancel'
};

paypal.request('SetExpressCheckout', query).then((result) => {
  console.log(result);
}).catch((err) => {
  console.trace(err);
});
```

In this case, it returns something like this:

```
{
  TOKEN: 'EC-5Y171147E8077933D',
  TIMESTAMP: '2016-03-18T09:58:38Z',
  CORRELATIONID: 'e8289e4235624',
  ACK: 'Success',
  VERSION: '124',
  BUILD: '18316154'
}

```


# API reference

### request(String method, Object query)

In which:

- *method*: one of API operations Paypal NVP supports, such as SetExpressCheckout, DoCapture, SetCustomerBillingAgreement, etc.
- *query*: a set of parameters you want to send to Paypal API endpoint, relying on which *method* is being used.

For more info:

- [NVP and SOAP API Reference](https://developer.paypal.com/docs/classic/api/)
- [Creating and managing NVP/SOAP API credentials](https://developer.paypal.com/docs/classic/api/apiCredentials/)

### formatCurrency(Number amount)

You can use this util to quickly convert a number to standard currency format that fits Paypal convention.

Return: a string in the format of X,XXX,XX.XX (used in United States, Canada).

```
paypal.formatCurrency(123456); // = '123,456.00'
paypal.formatCurrency(12345); // = '12,345.00'
paypal.formatCurrency(1234); // = '1,234.00'
paypal.formatCurrency(12); // = '12.00'
paypal.formatCurrency(12.5); // = '12.50'
paypal.formatCurrency('12.00'); // = '12.00'
```

# Test

```
git clone https://github.com/ndaidong/paypal-nvp-api.git
cd paypal-nvp-api
npm install
npm test
```


# License

The MIT License (MIT)
