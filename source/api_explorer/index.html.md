---
title: GOV.UK Pay API
language_tabs:
  - shell: Shell
  - http: HTTP
  - javascript: JavaScript
  - javascript--nodejs: Node.JS
  - ruby: Ruby
  - python: Python
  - java: Java
  - go: Go
toc_footers: []
includes: []
search: true
highlight_theme: darkula
headingLevel: 2

---

# Test

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

GOV.UK Pay API

Base URLs:

* <a href="https://publicapi.payments.service.gov.uk">https://publicapi.payments.service.gov.uk</a>

<h1 id="gov-uk-pay-api-default">Default</h1>

## searchPayments

<a id="opIdsearchPayments"></a>

`GET /v1/payments`

*Search payments*

Search payments by reference, state, 'from' and 'to' date. The Authorisation token needs to be specified in the 'authorization' header as 'authorization: Bearer YOUR_API_KEY_HERE'

<h3 id="searchpayments-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|reference|query|string|false|Your payment reference to search|
|email|query|string|false|The user email used in the payment to be searched|
|state|query|string|false|State of payments to be searched. Example=success|
|card_brand|query|string|false|Card brand used for payment. Example=master-card|
|from_date|query|string|false|From date of payments to be searched (this date is inclusive). Example=2015-08-13T12:35:00Z|
|to_date|query|string|false|To date of payments to be searched (this date is exclusive). Example=2015-08-14T12:35:00Z|
|page|query|string|false|Page number requested for the search, should be a positive integer (optional, defaults to 1)|
|display_size|query|string|false|Number of results to be shown per page, should be a positive integer (optional, defaults to 500, max 500)|
|cardholder_name|query|string|false|Name on card used to make payment|
|first_digits_card_number|query|string|false|First six digits of the card used to make payment|
|last_digits_card_number|query|string|false|Last four digits of the card used to make payment|

#### Enumerated Values

|Parameter|Value|
|---|---|
|state|range[created|
|state|started|
|state|submitted|
|state|success|
|state|failed|
|state|cancelled|
|state|error|

> Example responses

> 200 Response

<details>
<pre>
{
  "type": "object",
  "properties": {
    "results": {
      "type": "array",
      "readOnly": true,
      "items": {
        "allOf": [
          {
            "type": "object",
            "discriminator": {
              "propertyName": "paymentType"
            },
            "properties": {
              "amount": {
                "type": "integer",
                "format": "int64",
                "example": 1200
              },
              "state": {
                "type": "object",
                "properties": {
                  "status": {
                    "type": "string",
                    "example": "created",
                    "description": "Current progress of the payment in its lifecycle",
                    "readOnly": true
                  },
                  "finished": {
                    "type": "boolean",
                    "description": "Whether the payment has finished",
                    "readOnly": true
                  },
                  "message": {
                    "type": "string",
                    "example": "User cancelled the payment",
                    "description": "What went wrong with the Payment if it finished with an error - English message",
                    "readOnly": true
                  },
                  "code": {
                    "type": "string",
                    "example": "P010",
                    "description": "What went wrong with the Payment if it finished with an error - error code",
                    "readOnly": true
                  }
                },
                "description": "A structure representing the current state of the payment in its lifecycle."
              },
              "description": {
                "type": "string",
                "example": "Your Service Description"
              },
              "reference": {
                "type": "string",
                "example": "your-reference"
              },
              "email": {
                "type": "string",
                "example": "your email"
              },
              "payment_id": {
                "type": "string",
                "example": "hu20sqlact5260q2nanm0q8u93",
                "readOnly": true
              },
              "payment_provider": {
                "type": "string",
                "example": "worldpay",
                "readOnly": true
              },
              "return_url": {
                "type": "string",
                "example": "http://your.service.domain/your-reference",
                "readOnly": true
              },
              "created_date": {
                "type": "string",
                "example": "2016-01-21T17:15:00Z",
                "readOnly": true
              }
            }
          },
          {
            "type": "object",
            "properties": {
              "refund_summary": {
                "type": "object",
                "properties": {
                  "status": {
                    "type": "string",
                    "example": "available",
                    "description": "Availability status of the refund"
                  },
                  "amount_available": {
                    "type": "integer",
                    "format": "int64",
                    "description": "Amount available for refund in pence",
                    "readOnly": true
                  },
                  "amount_submitted": {
                    "type": "integer",
                    "format": "int64",
                    "description": "Amount submitted for refunds on this Payment in pence",
                    "readOnly": true
                  }
                },
                "description": "A structure representing the refunds availability"
              },
              "settlement_summary": {
                "type": "object",
                "properties": {
                  "capture_submit_time": {
                    "type": "string",
                    "example": "2016-01-21T17:15:00Z",
                    "description": "Date and time capture request has been submitted (may be null if capture request was not immediately acknowledged by payment gateway)",
                    "readOnly": true
                  },
                  "captured_date": {
                    "type": "string",
                    "example": "2016-01-21",
                    "description": "Date of the capture event",
                    "readOnly": true
                  }
                },
                "description": "A structure representing information about a settlement"
              },
              "card_details": {
                "type": "object",
                "properties": {
                  "last_digits_card_number": {
                    "type": "string",
                    "example": "1234",
                    "readOnly": true
                  },
                  "first_digits_card_number": {
                    "type": "string",
                    "example": "123456",
                    "readOnly": true
                  },
                  "cardholder_name": {
                    "type": "string",
                    "example": "Mr. Card holder",
                    "readOnly": true
                  },
                  "expiry_date": {
                    "type": "string",
                    "example": "12/20",
                    "readOnly": true
                  },
                  "billing_address": {
                    "type": "object",
                    "properties": {},
                    "description": "A structure representing the billing address of a card"
                  },
                  "card_brand": {
                    "type": "string",
                    "example": "Visa",
                    "readOnly": true
                  }
                },
                "description": "A structure representing the payment card"
              },
              "delayed_capture": {
                "type": "boolean",
                "example": false,
                "description": "delayed capture flag",
                "readOnly": true
              },
              "corporate_card_surcharge": {
                "type": "integer",
                "format": "int64",
                "example": 250,
                "readOnly": true
              },
              "total_amount": {
                "type": "integer",
                "format": "int64",
                "example": 1450,
                "readOnly": true
              },
              "card_brand": {
                "type": "string",
                "example": "Visa",
                "description": "Card Brand",
                "readOnly": true
              }
            }
          }
        ]
      }
    }
  }
}
</pre>
</details>

<h3 id="searchpayments-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[PaymentSearchResults](#schemapaymentsearchresults)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Credentials are required to access this resource|None|
|422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|Invalid parameters: from_date, to_date, status, display_size. See Public API documentation for the correct data formats|[PaymentError](#schemapaymenterror)|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too many requests|[ErrorResponse](#schemaerrorresponse)|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Downstream system error|[PaymentError](#schemapaymenterror)|

<aside class="success">
This operation does not require authentication
</aside>

## newPayment

<a id="opIdnewPayment"></a>

`POST /v1/payments`

*Create new payment*

Create a new payment for the account associated to the Authorisation token. The Authorisation token needs to be specified in the 'authorization' header as 'authorization: Bearer YOUR_API_KEY_HERE'

> Body parameter

```json
{
  "type": "object",
  "required": [
    "amount",
    "description",
    "reference",
    "return_url"
  ],
  "properties": {
    "amount": {
      "type": "integer",
      "format": "int32",
      "example": 12000,
      "description": "amount in pence",
      "minimum": 1,
      "maximum": 10000000
    },
    "reference": {
      "type": "string",
      "example": "12345",
      "description": "payment reference"
    },
    "return_url": {
      "type": "string",
      "example": "https://service-name.gov.uk/transactions/12345",
      "description": "service return url"
    },
    "description": {
      "type": "string",
      "example": "New passport application",
      "description": "payment description"
    }
  }
}
```

<h3 id="newpayment-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[ValidCreatePaymentRequest](#schemavalidcreatepaymentrequest)|true|requestPayload|

> Example responses

> 201 Response

```json
{
  "type": "object",
  "properties": {
    "payment": {
      "type": "object",
      "discriminator": {
        "propertyName": "paymentType"
      },
      "properties": {
        "amount": {
          "type": "integer",
          "format": "int64",
          "example": 1200
        },
        "state": {
          "type": "object",
          "properties": {
            "status": {
              "type": "string",
              "example": "created",
              "description": "Current progress of the payment in its lifecycle",
              "readOnly": true
            },
            "finished": {
              "type": "boolean",
              "description": "Whether the payment has finished",
              "readOnly": true
            },
            "message": {
              "type": "string",
              "example": "User cancelled the payment",
              "description": "What went wrong with the Payment if it finished with an error - English message",
              "readOnly": true
            },
            "code": {
              "type": "string",
              "example": "P010",
              "description": "What went wrong with the Payment if it finished with an error - error code",
              "readOnly": true
            }
          },
          "description": "A structure representing the current state of the payment in its lifecycle."
        },
        "description": {
          "type": "string",
          "example": "Your Service Description"
        },
        "reference": {
          "type": "string",
          "example": "your-reference"
        },
        "email": {
          "type": "string",
          "example": "your email"
        },
        "payment_id": {
          "type": "string",
          "example": "hu20sqlact5260q2nanm0q8u93",
          "readOnly": true
        },
        "payment_provider": {
          "type": "string",
          "example": "worldpay",
          "readOnly": true
        },
        "return_url": {
          "type": "string",
          "example": "http://your.service.domain/your-reference",
          "readOnly": true
        },
        "created_date": {
          "type": "string",
          "example": "2016-01-21T17:15:00Z",
          "readOnly": true
        }
      }
    },
    "_links": {
      "type": "object",
      "properties": {
        "self": {
          "type": "object",
          "properties": {
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "GET",
              "readOnly": true
            }
          },
          "description": "A link related to a payment"
        },
        "next_url": {
          "type": "object",
          "properties": {
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "GET",
              "readOnly": true
            }
          },
          "description": "A link related to a payment"
        },
        "next_url_post": {
          "type": "object",
          "properties": {
            "type": {
              "type": "string",
              "example": "multipart/form-data"
            },
            "params": {
              "type": "object",
              "example": "\"description\":\"This is a value for a parameter called description\"",
              "additionalProperties": {
                "type": "object"
              }
            },
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "POST",
              "readOnly": true
            }
          },
          "description": "A POST link related to a payment"
        },
        "events": {
          "type": "object",
          "properties": {
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "GET",
              "readOnly": true
            }
          },
          "description": "A link related to a payment"
        },
        "refunds": {
          "type": "object",
          "properties": {
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "GET",
              "readOnly": true
            }
          },
          "description": "A link related to a payment"
        },
        "cancel": {
          "type": "object",
          "properties": {
            "type": {
              "type": "string",
              "example": "multipart/form-data"
            },
            "params": {
              "type": "object",
              "example": "\"description\":\"This is a value for a parameter called description\"",
              "additionalProperties": {
                "type": "object"
              }
            },
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "POST",
              "readOnly": true
            }
          },
          "description": "A POST link related to a payment"
        },
        "capture": {
          "type": "object",
          "properties": {
            "type": {
              "type": "string",
              "example": "multipart/form-data"
            },
            "params": {
              "type": "object",
              "example": "\"description\":\"This is a value for a parameter called description\"",
              "additionalProperties": {
                "type": "object"
              }
            },
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "POST",
              "readOnly": true
            }
          },
          "description": "A POST link related to a payment"
        }
      },
      "description": "links for payment"
    }
  }
}
```

<h3 id="newpayment-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Created|[PaymentWithAllLinks](#schemapaymentwithalllinks)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[PaymentError](#schemapaymenterror)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Credentials are required to access this resource|None|
|422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|Invalid attribute value: description. Must be less than or equal to 255 characters length|[PaymentError](#schemapaymenterror)|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too many requests|[ErrorResponse](#schemaerrorresponse)|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Downstream system error|[PaymentError](#schemapaymenterror)|

<aside class="success">
This operation does not require authentication
</aside>

## getPayment

<a id="opIdgetPayment"></a>

`GET /v1/payments/{paymentId}`

*Find payment by ID*

Return information about the payment The Authorisation token needs to be specified in the 'authorization' header as 'authorization: Bearer YOUR_API_KEY_HERE'

<h3 id="getpayment-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|paymentId|path|string|true|none|

> Example responses

> 200 Response

```json
{
  "type": "object",
  "properties": {
    "payment": {
      "type": "object",
      "discriminator": {
        "propertyName": "paymentType"
      },
      "properties": {
        "amount": {
          "type": "integer",
          "format": "int64",
          "example": 1200
        },
        "state": {
          "type": "object",
          "properties": {
            "status": {
              "type": "string",
              "example": "created",
              "description": "Current progress of the payment in its lifecycle",
              "readOnly": true
            },
            "finished": {
              "type": "boolean",
              "description": "Whether the payment has finished",
              "readOnly": true
            },
            "message": {
              "type": "string",
              "example": "User cancelled the payment",
              "description": "What went wrong with the Payment if it finished with an error - English message",
              "readOnly": true
            },
            "code": {
              "type": "string",
              "example": "P010",
              "description": "What went wrong with the Payment if it finished with an error - error code",
              "readOnly": true
            }
          },
          "description": "A structure representing the current state of the payment in its lifecycle."
        },
        "description": {
          "type": "string",
          "example": "Your Service Description"
        },
        "reference": {
          "type": "string",
          "example": "your-reference"
        },
        "email": {
          "type": "string",
          "example": "your email"
        },
        "payment_id": {
          "type": "string",
          "example": "hu20sqlact5260q2nanm0q8u93",
          "readOnly": true
        },
        "payment_provider": {
          "type": "string",
          "example": "worldpay",
          "readOnly": true
        },
        "return_url": {
          "type": "string",
          "example": "http://your.service.domain/your-reference",
          "readOnly": true
        },
        "created_date": {
          "type": "string",
          "example": "2016-01-21T17:15:00Z",
          "readOnly": true
        }
      }
    },
    "_links": {
      "type": "object",
      "properties": {
        "self": {
          "type": "object",
          "properties": {
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "GET",
              "readOnly": true
            }
          },
          "description": "A link related to a payment"
        },
        "next_url": {
          "type": "object",
          "properties": {
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "GET",
              "readOnly": true
            }
          },
          "description": "A link related to a payment"
        },
        "next_url_post": {
          "type": "object",
          "properties": {
            "type": {
              "type": "string",
              "example": "multipart/form-data"
            },
            "params": {
              "type": "object",
              "example": "\"description\":\"This is a value for a parameter called description\"",
              "additionalProperties": {
                "type": "object"
              }
            },
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "POST",
              "readOnly": true
            }
          },
          "description": "A POST link related to a payment"
        },
        "events": {
          "type": "object",
          "properties": {
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "GET",
              "readOnly": true
            }
          },
          "description": "A link related to a payment"
        },
        "refunds": {
          "type": "object",
          "properties": {
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "GET",
              "readOnly": true
            }
          },
          "description": "A link related to a payment"
        },
        "cancel": {
          "type": "object",
          "properties": {
            "type": {
              "type": "string",
              "example": "multipart/form-data"
            },
            "params": {
              "type": "object",
              "example": "\"description\":\"This is a value for a parameter called description\"",
              "additionalProperties": {
                "type": "object"
              }
            },
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "POST",
              "readOnly": true
            }
          },
          "description": "A POST link related to a payment"
        },
        "capture": {
          "type": "object",
          "properties": {
            "type": {
              "type": "string",
              "example": "multipart/form-data"
            },
            "params": {
              "type": "object",
              "example": "\"description\":\"This is a value for a parameter called description\"",
              "additionalProperties": {
                "type": "object"
              }
            },
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "POST",
              "readOnly": true
            }
          },
          "description": "A POST link related to a payment"
        }
      },
      "description": "links for payment"
    }
  }
}
```

<h3 id="getpayment-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[PaymentWithAllLinks](#schemapaymentwithalllinks)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Credentials are required to access this resource|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|[PaymentError](#schemapaymenterror)|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too many requests|[ErrorResponse](#schemaerrorresponse)|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Downstream system error|[PaymentError](#schemapaymenterror)|

<aside class="success">
This operation does not require authentication
</aside>

## cancelPayment

<a id="opIdcancelPayment"></a>

`POST /v1/payments/{paymentId}/cancel`

*Cancel payment*

Cancel a payment based on the provided payment ID and the Authorisation token. The Authorisation token needs to be specified in the 'authorization' header as 'authorization: Bearer YOUR_API_KEY_HERE'. A payment can only be cancelled if it's in a state that isn't finished.

<h3 id="cancelpayment-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|paymentId|path|string|true|none|

> Example responses

> 400 Response

```json
{
  "type": "object",
  "properties": {
    "field": {
      "type": "string",
      "example": "amount"
    },
    "code": {
      "type": "string",
      "example": "P0102"
    },
    "description": {
      "type": "string",
      "example": "Invalid attribute value: amount. Must be less than or equal to 10000000"
    }
  },
  "description": "A Payment Error response"
}
```

<h3 id="cancelpayment-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|No Content|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Cancellation of payment failed|[PaymentError](#schemapaymenterror)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Credentials are required to access this resource|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|[PaymentError](#schemapaymenterror)|
|409|[Conflict](https://tools.ietf.org/html/rfc7231#section-6.5.8)|Conflict|[PaymentError](#schemapaymenterror)|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too many requests|[ErrorResponse](#schemaerrorresponse)|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Downstream system error|[PaymentError](#schemapaymenterror)|

<aside class="success">
This operation does not require authentication
</aside>

## capturePayment

<a id="opIdcapturePayment"></a>

`POST /v1/payments/{paymentId}/capture`

*Capture payment*

Capture a payment based on the provided payment ID and the Authorisation token. The Authorisation token needs to be specified in the 'authorization' header as 'authorization: Bearer YOUR_API_KEY_HERE'. A payment can only be captured if it's in 'submitted' state

<h3 id="capturepayment-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|paymentId|path|string|true|none|

> Example responses

> 400 Response

```json
{
  "type": "object",
  "properties": {
    "field": {
      "type": "string",
      "example": "amount"
    },
    "code": {
      "type": "string",
      "example": "P0102"
    },
    "description": {
      "type": "string",
      "example": "Invalid attribute value: amount. Must be less than or equal to 10000000"
    }
  },
  "description": "A Payment Error response"
}
```

<h3 id="capturepayment-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|No Content|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Capture of payment failed|[PaymentError](#schemapaymenterror)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Credentials are required to access this resource|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|[PaymentError](#schemapaymenterror)|
|409|[Conflict](https://tools.ietf.org/html/rfc7231#section-6.5.8)|Conflict|[PaymentError](#schemapaymenterror)|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too many requests|[ErrorResponse](#schemaerrorresponse)|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Downstream system error|[PaymentError](#schemapaymenterror)|

<aside class="success">
This operation does not require authentication
</aside>

## getPaymentEvents

<a id="opIdgetPaymentEvents"></a>

`GET /v1/payments/{paymentId}/events`

*Return payment events by ID*

Return payment events information about a certain payment The Authorisation token needs to be specified in the 'authorization' header as 'authorization: Bearer YOUR_API_KEY_HERE'

<h3 id="getpaymentevents-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|paymentId|path|string|true|none|

> Example responses

> 200 Response

```json
{
  "type": "object",
  "properties": {
    "events": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "payment_id": {
            "type": "string",
            "example": "hu20sqlact5260q2nanm0q8u93",
            "readOnly": true
          },
          "state": {
            "type": "object",
            "properties": {
              "status": {
                "type": "string",
                "example": "created",
                "description": "Current progress of the payment in its lifecycle",
                "readOnly": true
              },
              "finished": {
                "type": "boolean",
                "description": "Whether the payment has finished",
                "readOnly": true
              },
              "message": {
                "type": "string",
                "example": "User cancelled the payment",
                "description": "What went wrong with the Payment if it finished with an error - English message",
                "readOnly": true
              },
              "code": {
                "type": "string",
                "example": "P010",
                "description": "What went wrong with the Payment if it finished with an error - error code",
                "readOnly": true
              }
            },
            "description": "A structure representing the current state of the payment in its lifecycle."
          },
          "updated": {
            "type": "string",
            "example": "updated_date",
            "description": "updated",
            "readOnly": true
          },
          "_links": {
            "type": "object",
            "properties": {
              "payment_url": {
                "type": "object",
                "properties": {
                  "href": {
                    "type": "string",
                    "example": "https://an.example.link/from/payment/platform",
                    "readOnly": true
                  },
                  "method": {
                    "type": "string",
                    "example": "GET",
                    "readOnly": true
                  }
                },
                "description": "A link related to a payment"
              }
            },
            "description": "Resource link for a payment of a payment event"
          }
        },
        "description": "A List of Payment Events information"
      }
    },
    "payment_id": {
      "type": "string",
      "example": "hu20sqlact5260q2nanm0q8u93",
      "readOnly": true
    },
    "_links": {
      "type": "object",
      "properties": {
        "self": {
          "type": "object",
          "properties": {
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "GET",
              "readOnly": true
            }
          },
          "description": "A link related to a payment"
        },
        "next_url": {
          "type": "object",
          "properties": {
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "GET",
              "readOnly": true
            }
          },
          "description": "A link related to a payment"
        },
        "next_url_post": {
          "type": "object",
          "properties": {
            "type": {
              "type": "string",
              "example": "multipart/form-data"
            },
            "params": {
              "type": "object",
              "example": "\"description\":\"This is a value for a parameter called description\"",
              "additionalProperties": {
                "type": "object"
              }
            },
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "POST",
              "readOnly": true
            }
          },
          "description": "A POST link related to a payment"
        },
        "events": {
          "type": "object",
          "properties": {
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "GET",
              "readOnly": true
            }
          },
          "description": "A link related to a payment"
        },
        "refunds": {
          "type": "object",
          "properties": {
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "GET",
              "readOnly": true
            }
          },
          "description": "A link related to a payment"
        },
        "cancel": {
          "type": "object",
          "properties": {
            "type": {
              "type": "string",
              "example": "multipart/form-data"
            },
            "params": {
              "type": "object",
              "example": "\"description\":\"This is a value for a parameter called description\"",
              "additionalProperties": {
                "type": "object"
              }
            },
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "POST",
              "readOnly": true
            }
          },
          "description": "A POST link related to a payment"
        },
        "capture": {
          "type": "object",
          "properties": {
            "type": {
              "type": "string",
              "example": "multipart/form-data"
            },
            "params": {
              "type": "object",
              "example": "\"description\":\"This is a value for a parameter called description\"",
              "additionalProperties": {
                "type": "object"
              }
            },
            "href": {
              "type": "string",
              "example": "https://an.example.link/from/payment/platform",
              "readOnly": true
            },
            "method": {
              "type": "string",
              "example": "POST",
              "readOnly": true
            }
          },
          "description": "A POST link related to a payment"
        }
      },
      "description": "links for payment"
    }
  },
  "description": "A List of Payment Events information"
}
```

<h3 id="getpaymentevents-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[PaymentEvents](#schemapaymentevents)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Credentials are required to access this resource|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|[PaymentError](#schemapaymenterror)|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too many requests|[ErrorResponse](#schemaerrorresponse)|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Downstream system error|[PaymentError](#schemapaymenterror)|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="gov-uk-pay-api-refunds">refunds</h1>

Public Api Endpoints for Refunds

## getRefunds

<a id="opIdgetRefunds"></a>

`GET /v1/payments/{paymentId}/refunds`

*Get all refunds for a payment.*

Return refunds for a payment. The Authorisation token needs to be specified in the 'authorization' header as 'authorization: Bearer YOUR_API_KEY_HERE'

<h3 id="getrefunds-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|paymentId|path|string|true|none|

> Example responses

> 404 Response

```json
{
  "type": "object",
  "properties": {
    "field": {
      "type": "string",
      "example": "amount"
    },
    "code": {
      "type": "string",
      "example": "P0102"
    },
    "description": {
      "type": "string",
      "example": "Invalid attribute value: amount. Must be less than or equal to 10000000"
    }
  },
  "description": "A Payment Error response"
}
```

<h3 id="getrefunds-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Credentials are required to access this resource|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|[PaymentError](#schemapaymenterror)|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too many requests|[ErrorResponse](#schemaerrorresponse)|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Downstream system error|[PaymentError](#schemapaymenterror)|

<aside class="success">
This operation does not require authentication
</aside>

## submitRefund

<a id="opIdsubmitRefund"></a>

`POST /v1/payments/{paymentId}/refunds`

*Submit a refund for a payment*

Return issued refund information. The Authorisation token needs to be specified in the 'authorization' header as 'authorization: Bearer YOUR_API_KEY_HERE'

> Body parameter

```json
{
  "type": "object",
  "required": [
    "amount"
  ],
  "properties": {
    "amount": {
      "type": "integer",
      "format": "int32",
      "example": 150000,
      "description": "Amount in pence. Can't be more than the available amount for refunds",
      "minimum": 1,
      "maximum": 10000000
    },
    "refund_amount_available": {
      "type": "integer",
      "format": "int32",
      "example": 200000,
      "description": "Amount in pence. Total amount still available before issuing the refund",
      "readOnly": true,
      "minimum": 1,
      "maximum": 10000000
    }
  },
  "description": "The Payment Refund Request Payload"
}
```

<h3 id="submitrefund-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|paymentId|path|string|true|paymentId|
|body|body|[CreatePaymentRefundRequest](#schemacreatepaymentrefundrequest)|true|requestPayload|

> Example responses

> 404 Response

```json
{
  "type": "object",
  "properties": {
    "field": {
      "type": "string",
      "example": "amount"
    },
    "code": {
      "type": "string",
      "example": "P0102"
    },
    "description": {
      "type": "string",
      "example": "Invalid attribute value: amount. Must be less than or equal to 10000000"
    }
  },
  "description": "A Payment Error response"
}
```

<h3 id="submitrefund-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|202|[Accepted](https://tools.ietf.org/html/rfc7231#section-6.3.3)|ACCEPTED|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Credentials are required to access this resource|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|[PaymentError](#schemapaymenterror)|
|412|[Precondition Failed](https://tools.ietf.org/html/rfc7232#section-4.2)|Refund amount available mismatch|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too many requests|[ErrorResponse](#schemaerrorresponse)|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Downstream system error|[PaymentError](#schemapaymenterror)|

<aside class="success">
This operation does not require authentication
</aside>

## getRefundById

<a id="opIdgetRefundById"></a>

`GET /v1/payments/{paymentId}/refunds/{refundId}`

*Find payment refund by ID*

Return payment refund information by Refund ID The Authorisation token needs to be specified in the 'authorization' header as 'authorization: Bearer YOUR_API_KEY_HERE'

<h3 id="getrefundbyid-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|paymentId|path|string|true|none|
|refundId|path|string|true|none|

> Example responses

> 404 Response

```json
{
  "type": "object",
  "properties": {
    "field": {
      "type": "string",
      "example": "amount"
    },
    "code": {
      "type": "string",
      "example": "P0102"
    },
    "description": {
      "type": "string",
      "example": "Invalid attribute value: amount. Must be less than or equal to 10000000"
    }
  },
  "description": "A Payment Error response"
}
```

<h3 id="getrefundbyid-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Credentials are required to access this resource|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|[PaymentError](#schemapaymenterror)|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too many requests|[ErrorResponse](#schemaerrorresponse)|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Downstream system error|[PaymentError](#schemapaymenterror)|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocSaddress">Address</h2>

<a id="schemaaddress"></a>

```yaml
type: object
properties:
  line1:
    type: string
    example: address line 1
    readOnly: true
  line2:
    type: string
    example: address line 2
    readOnly: true
  postcode:
    type: string
    example: AB1 2CD
    readOnly: true
  city:
    type: string
    example: address city
    readOnly: true
  country:
    type: string
    example: UK
    readOnly: true
description: A structure representing the billing address of a card

```

*A structure representing the billing address of a card*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|line1|string|false|read-only|none|
|line2|string|false|read-only|none|
|postcode|string|false|read-only|none|
|city|string|false|read-only|none|
|country|string|false|read-only|none|

<h2 id="tocScarddetails">CardDetails</h2>

<a id="schemacarddetails"></a>

```yaml
type: object
properties:
  last_digits_card_number:
    type: string
    example: '1234'
    readOnly: true
  first_digits_card_number:
    type: string
    example: '123456'
    readOnly: true
  cardholder_name:
    type: string
    example: Mr. Card holder
    readOnly: true
  expiry_date:
    type: string
    example: 12/20
    readOnly: true
  billing_address:
    type: object
    properties:
      line1:
        type: string
        example: address line 1
        readOnly: true
      line2:
        type: string
        example: address line 2
        readOnly: true
      postcode:
        type: string
        example: AB1 2CD
        readOnly: true
      city:
        type: string
        example: address city
        readOnly: true
      country:
        type: string
        example: UK
        readOnly: true
    description: A structure representing the billing address of a card
  card_brand:
    type: string
    example: Visa
    readOnly: true
description: A structure representing the payment card

```

*A structure representing the payment card*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|last_digits_card_number|string|false|read-only|none|
|first_digits_card_number|string|false|read-only|none|
|cardholder_name|string|false|read-only|none|
|expiry_date|string|false|read-only|none|
|billing_address|[Address](#schemaaddress)|false|none|A structure representing the billing address of a card|
|card_brand|string|false|read-only|none|

<h2 id="tocScardpayment">CardPayment</h2>

<a id="schemacardpayment"></a>

```yaml
allOf:
  - type: object
    discriminator:
      propertyName: paymentType
    properties:
      amount:
        type: integer
        format: int64
        example: 1200
      state:
        type: object
        properties:
          status:
            type: string
            example: created
            description: Current progress of the payment in its lifecycle
            readOnly: true
          finished:
            type: boolean
            description: Whether the payment has finished
            readOnly: true
          message:
            type: string
            example: User cancelled the payment
            description: >-
              What went wrong with the Payment if it finished with an error -
              English message
            readOnly: true
          code:
            type: string
            example: P010
            description: >-
              What went wrong with the Payment if it finished with an error -
              error code
            readOnly: true
        description: >-
          A structure representing the current state of the payment in its
          lifecycle.
      description:
        type: string
        example: Your Service Description
      reference:
        type: string
        example: your-reference
      email:
        type: string
        example: your email
      payment_id:
        type: string
        example: hu20sqlact5260q2nanm0q8u93
        readOnly: true
      payment_provider:
        type: string
        example: worldpay
        readOnly: true
      return_url:
        type: string
        example: 'http://your.service.domain/your-reference'
        readOnly: true
      created_date:
        type: string
        example: '2016-01-21T17:15:00Z'
        readOnly: true
  - type: object
    properties:
      refund_summary:
        type: object
        properties:
          status:
            type: string
            example: available
            description: Availability status of the refund
          amount_available:
            type: integer
            format: int64
            description: Amount available for refund in pence
            readOnly: true
          amount_submitted:
            type: integer
            format: int64
            description: Amount submitted for refunds on this Payment in pence
            readOnly: true
        description: A structure representing the refunds availability
      settlement_summary:
        type: object
        properties:
          capture_submit_time:
            type: string
            example: '2016-01-21T17:15:00Z'
            description: >-
              Date and time capture request has been submitted (may be null if
              capture request was not immediately acknowledged by payment
              gateway)
            readOnly: true
          captured_date:
            type: string
            example: '2016-01-21'
            description: Date of the capture event
            readOnly: true
        description: A structure representing information about a settlement
      card_details:
        type: object
        properties:
          last_digits_card_number:
            type: string
            example: '1234'
            readOnly: true
          first_digits_card_number:
            type: string
            example: '123456'
            readOnly: true
          cardholder_name:
            type: string
            example: Mr. Card holder
            readOnly: true
          expiry_date:
            type: string
            example: 12/20
            readOnly: true
          billing_address:
            type: object
            properties:
              line1:
                type: string
                example: address line 1
                readOnly: true
              line2:
                type: string
                example: address line 2
                readOnly: true
              postcode:
                type: string
                example: AB1 2CD
                readOnly: true
              city:
                type: string
                example: address city
                readOnly: true
              country:
                type: string
                example: UK
                readOnly: true
            description: A structure representing the billing address of a card
          card_brand:
            type: string
            example: Visa
            readOnly: true
        description: A structure representing the payment card
      delayed_capture:
        type: boolean
        example: false
        description: delayed capture flag
        readOnly: true
      corporate_card_surcharge:
        type: integer
        format: int64
        example: 250
        readOnly: true
      total_amount:
        type: integer
        format: int64
        example: 1450
        readOnly: true
      card_brand:
        type: string
        example: Visa
        description: Card Brand
        readOnly: true

```

### Properties

*allOf - discriminator: Payment.paymentType*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[Payment](#schemapayment)|false|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» refund_summary|[RefundSummary](#schemarefundsummary)|false|none|A structure representing the refunds availability|
|» settlement_summary|[SettlementSummary](#schemasettlementsummary)|false|none|A structure representing information about a settlement|
|» card_details|[CardDetails](#schemacarddetails)|false|none|A structure representing the payment card|
|» delayed_capture|boolean|false|read-only|delayed capture flag|
|» corporate_card_surcharge|integer(int64)|false|read-only|none|
|» total_amount|integer(int64)|false|read-only|none|
|» card_brand|string|false|read-only|Card Brand|

<h2 id="tocScreatepaymentrefundrequest">CreatePaymentRefundRequest</h2>

<a id="schemacreatepaymentrefundrequest"></a>

```yaml
type: object
required:
  - amount
properties:
  amount:
    type: integer
    format: int32
    example: 150000
    description: Amount in pence. Can't be more than the available amount for refunds
    minimum: 1
    maximum: 10000000
  refund_amount_available:
    type: integer
    format: int32
    example: 200000
    description: Amount in pence. Total amount still available before issuing the refund
    readOnly: true
    minimum: 1
    maximum: 10000000
description: The Payment Refund Request Payload

```

*The Payment Refund Request Payload*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|amount|integer(int32)|true|none|Amount in pence. Can't be more than the available amount for refunds|
|refund_amount_available|integer(int32)|false|read-only|Amount in pence. Total amount still available before issuing the refund|

<h2 id="tocSdirectdebitpayment">DirectDebitPayment</h2>

<a id="schemadirectdebitpayment"></a>

```yaml
allOf:
  - type: object
    discriminator:
      propertyName: paymentType
    properties:
      amount:
        type: integer
        format: int64
        example: 1200
      state:
        type: object
        properties:
          status:
            type: string
            example: created
            description: Current progress of the payment in its lifecycle
            readOnly: true
          finished:
            type: boolean
            description: Whether the payment has finished
            readOnly: true
          message:
            type: string
            example: User cancelled the payment
            description: >-
              What went wrong with the Payment if it finished with an error -
              English message
            readOnly: true
          code:
            type: string
            example: P010
            description: >-
              What went wrong with the Payment if it finished with an error -
              error code
            readOnly: true
        description: >-
          A structure representing the current state of the payment in its
          lifecycle.
      description:
        type: string
        example: Your Service Description
      reference:
        type: string
        example: your-reference
      email:
        type: string
        example: your email
      payment_id:
        type: string
        example: hu20sqlact5260q2nanm0q8u93
        readOnly: true
      payment_provider:
        type: string
        example: worldpay
        readOnly: true
      return_url:
        type: string
        example: 'http://your.service.domain/your-reference'
        readOnly: true
      created_date:
        type: string
        example: '2016-01-21T17:15:00Z'
        readOnly: true
  - type: object

```

### Properties

*allOf - discriminator: Payment.paymentType*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[Payment](#schemapayment)|false|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|

<h2 id="tocSerrorresponse">ErrorResponse</h2>

<a id="schemaerrorresponse"></a>

```yaml
type: object
properties:
  code:
    type: string
    example: P0900
  description:
    type: string
    example: Too many requests
description: An error response

```

*An error response*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|string|false|none|none|
|description|string|false|none|none|

<h2 id="tocSlink">Link</h2>

<a id="schemalink"></a>

```yaml
type: object
properties:
  href:
    type: string
    example: 'https://an.example.link/from/payment/platform'
    readOnly: true
  method:
    type: string
    example: GET
    readOnly: true
description: A link related to a payment

```

*A link related to a payment*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|href|string|false|read-only|none|
|method|string|false|read-only|none|

<h2 id="tocSpayment">Payment</h2>

<a id="schemapayment"></a>

```yaml
type: object
discriminator:
  propertyName: paymentType
properties:
  amount:
    type: integer
    format: int64
    example: 1200
  state:
    type: object
    properties:
      status:
        type: string
        example: created
        description: Current progress of the payment in its lifecycle
        readOnly: true
      finished:
        type: boolean
        description: Whether the payment has finished
        readOnly: true
      message:
        type: string
        example: User cancelled the payment
        description: >-
          What went wrong with the Payment if it finished with an error -
          English message
        readOnly: true
      code:
        type: string
        example: P010
        description: >-
          What went wrong with the Payment if it finished with an error - error
          code
        readOnly: true
    description: >-
      A structure representing the current state of the payment in its
      lifecycle.
  description:
    type: string
    example: Your Service Description
  reference:
    type: string
    example: your-reference
  email:
    type: string
    example: your email
  payment_id:
    type: string
    example: hu20sqlact5260q2nanm0q8u93
    readOnly: true
  payment_provider:
    type: string
    example: worldpay
    readOnly: true
  return_url:
    type: string
    example: 'http://your.service.domain/your-reference'
    readOnly: true
  created_date:
    type: string
    example: '2016-01-21T17:15:00Z'
    readOnly: true

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|amount|integer(int64)|false|none|none|
|state|[PaymentState](#schemapaymentstate)|false|none|A structure representing the current state of the payment in its lifecycle.|
|description|string|false|none|none|
|reference|string|false|none|none|
|email|string|false|none|none|
|payment_id|string|false|read-only|none|
|payment_provider|string|false|read-only|none|
|return_url|string|false|read-only|none|
|created_date|string|false|read-only|none|

<h2 id="tocSpaymenterror">PaymentError</h2>

<a id="schemapaymenterror"></a>

```yaml
type: object
properties:
  field:
    type: string
    example: amount
  code:
    type: string
    example: P0102
  description:
    type: string
    example: 'Invalid attribute value: amount. Must be less than or equal to 10000000'
description: A Payment Error response

```

*A Payment Error response*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|field|string|false|none|none|
|code|string|false|none|none|
|description|string|false|none|none|

<h2 id="tocSpaymentevent">PaymentEvent</h2>

<a id="schemapaymentevent"></a>

```yaml
type: object
properties:
  payment_id:
    type: string
    example: hu20sqlact5260q2nanm0q8u93
    readOnly: true
  state:
    type: object
    properties:
      status:
        type: string
        example: created
        description: Current progress of the payment in its lifecycle
        readOnly: true
      finished:
        type: boolean
        description: Whether the payment has finished
        readOnly: true
      message:
        type: string
        example: User cancelled the payment
        description: >-
          What went wrong with the Payment if it finished with an error -
          English message
        readOnly: true
      code:
        type: string
        example: P010
        description: >-
          What went wrong with the Payment if it finished with an error - error
          code
        readOnly: true
    description: >-
      A structure representing the current state of the payment in its
      lifecycle.
  updated:
    type: string
    example: updated_date
    description: updated
    readOnly: true
  _links:
    type: object
    properties:
      payment_url:
        type: object
        properties:
          href:
            type: string
            example: 'https://an.example.link/from/payment/platform'
            readOnly: true
          method:
            type: string
            example: GET
            readOnly: true
        description: A link related to a payment
    description: Resource link for a payment of a payment event
description: A List of Payment Events information

```

*A List of Payment Events information*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|payment_id|string|false|read-only|none|
|state|[PaymentState](#schemapaymentstate)|false|none|state|
|updated|string|false|read-only|updated|
|_links|[PaymentEventLink](#schemapaymenteventlink)|false|none|Resource link for a payment of a payment event|

<h2 id="tocSpaymenteventlink">PaymentEventLink</h2>

<a id="schemapaymenteventlink"></a>

```yaml
type: object
properties:
  payment_url:
    type: object
    properties:
      href:
        type: string
        example: 'https://an.example.link/from/payment/platform'
        readOnly: true
      method:
        type: string
        example: GET
        readOnly: true
    description: A link related to a payment
description: Resource link for a payment of a payment event

```

*Resource link for a payment of a payment event*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|payment_url|[Link](#schemalink)|false|none|payment_url|

<h2 id="tocSpaymentevents">PaymentEvents</h2>

<a id="schemapaymentevents"></a>

```yaml
type: object
properties:
  events:
    type: array
    items:
      type: object
      properties:
        payment_id:
          type: string
          example: hu20sqlact5260q2nanm0q8u93
          readOnly: true
        state:
          type: object
          properties:
            status:
              type: string
              example: created
              description: Current progress of the payment in its lifecycle
              readOnly: true
            finished:
              type: boolean
              description: Whether the payment has finished
              readOnly: true
            message:
              type: string
              example: User cancelled the payment
              description: >-
                What went wrong with the Payment if it finished with an error -
                English message
              readOnly: true
            code:
              type: string
              example: P010
              description: >-
                What went wrong with the Payment if it finished with an error -
                error code
              readOnly: true
          description: >-
            A structure representing the current state of the payment in its
            lifecycle.
        updated:
          type: string
          example: updated_date
          description: updated
          readOnly: true
        _links:
          type: object
          properties:
            payment_url: &ref_0
              type: object
              properties:
                href:
                  type: string
                  example: 'https://an.example.link/from/payment/platform'
                  readOnly: true
                method:
                  type: string
                  example: GET
                  readOnly: true
              description: A link related to a payment
          description: Resource link for a payment of a payment event
      description: A List of Payment Events information
  payment_id:
    type: string
    example: hu20sqlact5260q2nanm0q8u93
    readOnly: true
  _links:
    type: object
    properties:
      self: *ref_0
      next_url: *ref_0
      next_url_post: &ref_1
        type: object
        properties:
          type:
            type: string
            example: multipart/form-data
          params:
            type: object
            example: '"description":"This is a value for a parameter called description"'
            additionalProperties:
              type: object
          href:
            type: string
            example: 'https://an.example.link/from/payment/platform'
            readOnly: true
          method:
            type: string
            example: POST
            readOnly: true
        description: A POST link related to a payment
      events: *ref_0
      refunds: *ref_0
      cancel: *ref_1
      capture: *ref_1
    description: links for payment
description: A List of Payment Events information

```

*A List of Payment Events information*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|events|[[PaymentEvent](#schemapaymentevent)]|false|none|[A List of Payment Events information]|
|payment_id|string|false|read-only|none|
|_links|[PaymentLinks](#schemapaymentlinks)|false|none|links for payment|

<h2 id="tocSpaymentlinks">PaymentLinks</h2>

<a id="schemapaymentlinks"></a>

```yaml
type: object
properties:
  self: &ref_0
    type: object
    properties:
      href:
        type: string
        example: 'https://an.example.link/from/payment/platform'
        readOnly: true
      method:
        type: string
        example: GET
        readOnly: true
    description: A link related to a payment
  next_url: *ref_0
  next_url_post: &ref_1
    type: object
    properties:
      type:
        type: string
        example: multipart/form-data
      params:
        type: object
        example: '"description":"This is a value for a parameter called description"'
        additionalProperties:
          type: object
      href:
        type: string
        example: 'https://an.example.link/from/payment/platform'
        readOnly: true
      method:
        type: string
        example: POST
        readOnly: true
    description: A POST link related to a payment
  events: *ref_0
  refunds: *ref_0
  cancel: *ref_1
  capture: *ref_1
description: links for payment

```

*links for payment*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|self|[Link](#schemalink)|false|none|self|
|next_url|[Link](#schemalink)|false|none|next_url|
|next_url_post|[PostLink](#schemapostlink)|false|none|next_url_post|
|events|[Link](#schemalink)|false|none|events|
|refunds|[Link](#schemalink)|false|none|refunds|
|cancel|[PostLink](#schemapostlink)|false|none|cancel|
|capture|[PostLink](#schemapostlink)|false|none|capture|

<h2 id="tocSpaymentlinksforsearch">PaymentLinksForSearch</h2>

<a id="schemapaymentlinksforsearch"></a>

```yaml
type: object
properties:
  self: &ref_0
    type: object
    properties:
      href:
        type: string
        example: 'https://an.example.link/from/payment/platform'
        readOnly: true
      method:
        type: string
        example: GET
        readOnly: true
    description: A link related to a payment
  cancel: &ref_1
    type: object
    properties:
      type:
        type: string
        example: multipart/form-data
      params:
        type: object
        example: '"description":"This is a value for a parameter called description"'
        additionalProperties:
          type: object
      href:
        type: string
        example: 'https://an.example.link/from/payment/platform'
        readOnly: true
      method:
        type: string
        example: POST
        readOnly: true
    description: A POST link related to a payment
  events: *ref_0
  refunds: *ref_0
  capture: *ref_1
description: links for search payment resource

```

*links for search payment resource*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|self|[Link](#schemalink)|false|none|self|
|cancel|[PostLink](#schemapostlink)|false|none|cancel|
|events|[Link](#schemalink)|false|none|events|
|refunds|[Link](#schemalink)|false|none|refunds|
|capture|[PostLink](#schemapostlink)|false|none|capture|

<h2 id="tocSpaymentsearchresults">PaymentSearchResults</h2>

<a id="schemapaymentsearchresults"></a>

```yaml
type: object
properties:
  results:
    type: array
    readOnly: true
    items:
      allOf:
        - type: object
          discriminator:
            propertyName: paymentType
          properties:
            amount:
              type: integer
              format: int64
              example: 1200
            state:
              type: object
              properties:
                status:
                  type: string
                  example: created
                  description: Current progress of the payment in its lifecycle
                  readOnly: true
                finished:
                  type: boolean
                  description: Whether the payment has finished
                  readOnly: true
                message:
                  type: string
                  example: User cancelled the payment
                  description: >-
                    What went wrong with the Payment if it finished with an
                    error - English message
                  readOnly: true
                code:
                  type: string
                  example: P010
                  description: >-
                    What went wrong with the Payment if it finished with an
                    error - error code
                  readOnly: true
              description: >-
                A structure representing the current state of the payment in its
                lifecycle.
            description:
              type: string
              example: Your Service Description
            reference:
              type: string
              example: your-reference
            email:
              type: string
              example: your email
            payment_id:
              type: string
              example: hu20sqlact5260q2nanm0q8u93
              readOnly: true
            payment_provider:
              type: string
              example: worldpay
              readOnly: true
            return_url:
              type: string
              example: 'http://your.service.domain/your-reference'
              readOnly: true
            created_date:
              type: string
              example: '2016-01-21T17:15:00Z'
              readOnly: true
        - type: object
          properties:
            refund_summary:
              type: object
              properties:
                status:
                  type: string
                  example: available
                  description: Availability status of the refund
                amount_available:
                  type: integer
                  format: int64
                  description: Amount available for refund in pence
                  readOnly: true
                amount_submitted:
                  type: integer
                  format: int64
                  description: Amount submitted for refunds on this Payment in pence
                  readOnly: true
              description: A structure representing the refunds availability
            settlement_summary:
              type: object
              properties:
                capture_submit_time:
                  type: string
                  example: '2016-01-21T17:15:00Z'
                  description: >-
                    Date and time capture request has been submitted (may be
                    null if capture request was not immediately acknowledged by
                    payment gateway)
                  readOnly: true
                captured_date:
                  type: string
                  example: '2016-01-21'
                  description: Date of the capture event
                  readOnly: true
              description: A structure representing information about a settlement
            card_details:
              type: object
              properties:
                last_digits_card_number:
                  type: string
                  example: '1234'
                  readOnly: true
                first_digits_card_number:
                  type: string
                  example: '123456'
                  readOnly: true
                cardholder_name:
                  type: string
                  example: Mr. Card holder
                  readOnly: true
                expiry_date:
                  type: string
                  example: 12/20
                  readOnly: true
                billing_address:
                  type: object
                  properties: {}
                  description: A structure representing the billing address of a card
                card_brand:
                  type: string
                  example: Visa
                  readOnly: true
              description: A structure representing the payment card
            delayed_capture:
              type: boolean
              example: false
              description: delayed capture flag
              readOnly: true
            corporate_card_surcharge:
              type: integer
              format: int64
              example: 250
              readOnly: true
            total_amount:
              type: integer
              format: int64
              example: 1450
              readOnly: true
            card_brand:
              type: string
              example: Visa
              description: Card Brand
              readOnly: true

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|results|[[CardPayment](#schemacardpayment)]|false|read-only|none|

<h2 id="tocSpaymentstate">PaymentState</h2>

<a id="schemapaymentstate"></a>

```yaml
type: object
properties:
  status:
    type: string
    example: created
    description: Current progress of the payment in its lifecycle
    readOnly: true
  finished:
    type: boolean
    description: Whether the payment has finished
    readOnly: true
  message:
    type: string
    example: User cancelled the payment
    description: >-
      What went wrong with the Payment if it finished with an error - English
      message
    readOnly: true
  code:
    type: string
    example: P010
    description: What went wrong with the Payment if it finished with an error - error code
    readOnly: true
description: A structure representing the current state of the payment in its lifecycle.

```

*A structure representing the current state of the payment in its lifecycle.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|status|string|false|read-only|Current progress of the payment in its lifecycle|
|finished|boolean|false|read-only|Whether the payment has finished|
|message|string|false|read-only|What went wrong with the Payment if it finished with an error - English message|
|code|string|false|read-only|What went wrong with the Payment if it finished with an error - error code|

<h2 id="tocSpaymentwithalllinks">PaymentWithAllLinks</h2>

<a id="schemapaymentwithalllinks"></a>

```yaml
type: object
properties:
  payment:
    type: object
    discriminator:
      propertyName: paymentType
    properties:
      amount:
        type: integer
        format: int64
        example: 1200
      state:
        type: object
        properties:
          status:
            type: string
            example: created
            description: Current progress of the payment in its lifecycle
            readOnly: true
          finished:
            type: boolean
            description: Whether the payment has finished
            readOnly: true
          message:
            type: string
            example: User cancelled the payment
            description: >-
              What went wrong with the Payment if it finished with an error -
              English message
            readOnly: true
          code:
            type: string
            example: P010
            description: >-
              What went wrong with the Payment if it finished with an error -
              error code
            readOnly: true
        description: >-
          A structure representing the current state of the payment in its
          lifecycle.
      description:
        type: string
        example: Your Service Description
      reference:
        type: string
        example: your-reference
      email:
        type: string
        example: your email
      payment_id:
        type: string
        example: hu20sqlact5260q2nanm0q8u93
        readOnly: true
      payment_provider:
        type: string
        example: worldpay
        readOnly: true
      return_url:
        type: string
        example: 'http://your.service.domain/your-reference'
        readOnly: true
      created_date:
        type: string
        example: '2016-01-21T17:15:00Z'
        readOnly: true
  _links:
    type: object
    properties:
      self: &ref_0
        type: object
        properties:
          href:
            type: string
            example: 'https://an.example.link/from/payment/platform'
            readOnly: true
          method:
            type: string
            example: GET
            readOnly: true
        description: A link related to a payment
      next_url: *ref_0
      next_url_post: &ref_1
        type: object
        properties:
          type:
            type: string
            example: multipart/form-data
          params:
            type: object
            example: '"description":"This is a value for a parameter called description"'
            additionalProperties:
              type: object
          href:
            type: string
            example: 'https://an.example.link/from/payment/platform'
            readOnly: true
          method:
            type: string
            example: POST
            readOnly: true
        description: A POST link related to a payment
      events: *ref_0
      refunds: *ref_0
      cancel: *ref_1
      capture: *ref_1
    description: links for payment

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|payment|[Payment](#schemapayment)|false|none|none|
|_links|[PaymentLinks](#schemapaymentlinks)|false|none|links for payment|

<h2 id="tocSpostlink">PostLink</h2>

<a id="schemapostlink"></a>

```yaml
type: object
properties:
  type:
    type: string
    example: multipart/form-data
  params:
    type: object
    example: '"description":"This is a value for a parameter called description"'
    additionalProperties:
      type: object
  href:
    type: string
    example: 'https://an.example.link/from/payment/platform'
    readOnly: true
  method:
    type: string
    example: POST
    readOnly: true
description: A POST link related to a payment

```

*A POST link related to a payment*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|type|string|false|none|none|
|params|object|false|none|none|
|» **additionalProperties**|object|false|none|none|
|href|string|false|read-only|none|
|method|string|false|read-only|none|

<h2 id="tocSrefundsummary">RefundSummary</h2>

<a id="schemarefundsummary"></a>

```yaml
type: object
properties:
  status:
    type: string
    example: available
    description: Availability status of the refund
  amount_available:
    type: integer
    format: int64
    description: Amount available for refund in pence
    readOnly: true
  amount_submitted:
    type: integer
    format: int64
    description: Amount submitted for refunds on this Payment in pence
    readOnly: true
description: A structure representing the refunds availability

```

*A structure representing the refunds availability*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|status|string|false|none|Availability status of the refund|
|amount_available|integer(int64)|false|read-only|Amount available for refund in pence|
|amount_submitted|integer(int64)|false|read-only|Amount submitted for refunds on this Payment in pence|

<h2 id="tocSsettlementsummary">SettlementSummary</h2>

<a id="schemasettlementsummary"></a>

```yaml
type: object
properties:
  capture_submit_time:
    type: string
    example: '2016-01-21T17:15:00Z'
    description: >-
      Date and time capture request has been submitted (may be null if capture
      request was not immediately acknowledged by payment gateway)
    readOnly: true
  captured_date:
    type: string
    example: '2016-01-21'
    description: Date of the capture event
    readOnly: true
description: A structure representing information about a settlement

```

*A structure representing information about a settlement*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|capture_submit_time|string|false|read-only|Date and time capture request has been submitted (may be null if capture request was not immediately acknowledged by payment gateway)|
|captured_date|string|false|read-only|Date of the capture event|

<h2 id="tocSvalidcreatepaymentrequest">ValidCreatePaymentRequest</h2>

<a id="schemavalidcreatepaymentrequest"></a>

```yaml
type: object
required:
  - amount
  - description
  - reference
  - return_url
properties:
  amount:
    type: integer
    format: int32
    example: 12000
    description: amount in pence
    minimum: 1
    maximum: 10000000
  reference:
    type: string
    example: '12345'
    description: payment reference
  return_url:
    type: string
    example: 'https://service-name.gov.uk/transactions/12345'
    description: service return url
  description:
    type: string
    example: New passport application
    description: payment description

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|amount|integer(int32)|true|none|amount in pence|
|reference|string|true|none|payment reference|
|return_url|string|true|none|service return url|
|description|string|true|none|payment description|

