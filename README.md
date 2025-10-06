{
    "gatewayCustomerId": "e659aa04-65fc-42f4-8133-32b9cf9040e8",
    "name": "Test",
    "cisNumbers": [
        "100100100155"
    ],
    "enabled": true,
    "customerSettings": {
        "transactionLimit": 1000000,
        "cumulativeTransactionLimit": 5000000,
        "processingWindow": "00:00-23:59",
        "processingWindowTimezone": "CST"
    },
    "customerType": "COMMERCIAL",
    "virtualAcctCustomer": false,
    "demoCustomer": true,
    "customerAccounts": [
        {
            "number": "1000100010055",
            "name": "CBNK00001_00008",
            "routingNumber": "072000096",
            "bankCode": "10002",
            "cisNumbers": [
                "100100100155"
            ],
            "accountSettings": {
                "transactionLimit": 100,
                "cumulativeTransactionLimit": 10000
            },
            "billingAccount": true,
            "enabled": true,
            "products": [
                {
                    "id": null,
                    "name": "INSTANT_PAYMENTS_API",
                    "friendlyName": null,
                    "shortName": null,
                    "description": null,
                    "productSettings": null,
                    "enabled": true,
                    "billable": true,
                    "resources": [
                        {
                            "name": "CREATE_CREDIT_TRANSFER",
                            "friendlyName": null,
                            "description": null,
                            "enabled": true,
                            "billable": true
                        },
                        {
                            "name": "RETRIEVE_CREDIT_TRANSFER",
                            "friendlyName": null,
                            "description": null,
                            "enabled": true,
                            "billable": true
                        }
                    ],
                    "paymentRails": [
                        {
                            "name": "RTP",
                            "friendlyName": null,
                            "description": null,
                            "paymentRailSettings": {
                                "transactionLimit": 100,
                                "cumulativeTransactionLimit": 10000,
                                "duplicateCheckDuration": 0,
                                "allowedCreditAccountList": [
                                    {
                                        "accountNumber": "{\"accountNumber\": \"123456789\", \"routingNumber\": \"123456789\"}",
                                        "routingNumber": null
                                    }
                                ]
                            },
                            "enabled": true,
                            "billable": true
                        }
                    ]
                }
            ]
        }
    ],
    "customerProducts": [
        {
            "id": null,
            "name": "ACCOUNT_BALANCE_API",
            "friendlyName": null,
            "shortName": null,
            "description": null,
            "productSettings": null,
            "enabled": true,
            "billable": true,
            "resources": [
                {
                    "name": "RETRIEVE_ACCOUNT_BALANCE",
                    "friendlyName": null,
                    "description": null,
                    "enabled": true,
                    "billable": true
                }
            ],
            "paymentRails": null
        },
        {
            "id": null,
            "name": "INSTANT_PAYMENTS_API",
            "friendlyName": null,
            "shortName": null,
            "description": null,
            "productSettings": null,
            "enabled": true,
            "billable": true,
            "resources": [
                {
                    "name": "CREATE_CREDIT_TRANSFER",
                    "friendlyName": null,
                    "description": null,
                    "enabled": true,
                    "billable": true
                },
                {
                    "name": "RETRIEVE_CREDIT_TRANSFER",
                    "friendlyName": null,
                    "description": null,
                    "enabled": true,
                    "billable": true
                }
            ],
            "paymentRails": [
                {
                    "name": "RTP",
                    "friendlyName": null,
                    "description": null,
                    "paymentRailSettings": {
                        "transactionLimit": 500000,
                        "cumulativeTransactionLimit": 2000000,
                        "duplicateCheckDuration": 0
                    },
                    "enabled": true,
                    "billable": true
                },
                {
                    "name": "FEDNOW",
                    "friendlyName": null,
                    "description": null,
                    "paymentRailSettings": {
                        "transactionLimit": 500000,
                        "cumulativeTransactionLimit": 2000000,
                        "duplicateCheckDuration": 0
                    },
                    "enabled": true,
                    "billable": true
                }
            ]
        }
    ],
    "billingCustomerId": "SHB1001005",
    "createdBy": "IPA",
    "customerContacts": [
        {
            "contactName": "James",
            "contactTitle": "QA Engineer",
            "contactPhone": "129-000-0002",
            "contactPhoneType": "MOBILE",
            "contactEmail": "james.murphy@comerica.com",
            "contactPreferredType": "Email",
            "contactType": "SECONDARY",
            "enabled": false
        },
        {
            "contactName": "Ben Reynold",
            "contactTitle": "Developer",
            "contactPhone": "5309999999 ext 2345",
            "contactPhoneType": "WORK",
            "contactEmail": "ben.reynold@comerica.com",
            "contactPreferredType": "Phone",
            "contactType": "TERTIARY",
            "enabled": false
        }
    ]
}
