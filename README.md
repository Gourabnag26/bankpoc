import React, { useEffect, useState } from 'react';
import { Box ,PageSpinner} from '@ucl/ui-components';
import { useLocation, useNavigate } from 'react-router-dom';
import CustomerInfo from './components/customer-info/customer-info';
import BusinessContact from './components/business-contact/business-contact';
import TechnicalContact from './components/technical-contact/technical-contact';
import TransactionLimit from './components/transaction-limit/transaction-limit';
import ApiCustomerConfig from './components/api-customer/api-customer';
import AccountPreference from './components/account-preference/account-preference';
import ActionButton from './components/action-button/action-button';
import { useCustomer } from '../../context/customer-context';
import ApproverView from './approver-view/approver-view';
import { CustomerService } from 'shared';
const { v4: uuidv4 } = require('uuid');


export interface ICustomerData {
  gatewayCustomerId: string;
  name: string;
  cisNumbers: string[];
  enabled: boolean;
  customerSettings: ICustomerSettings;
  customerType: string;
  virtualAcctCustomer: boolean;
  demoCustomer: boolean;
  customerAccounts: ICustomerAccount[];
  customerProducts: ICustomerProduct[];
  billingCustomerId: string;
  createdBy: string;
  customerContacts: ICustomerContact[];
}

export interface ICustomerSettings {
  transactionLimit: number;
  cumulativeTransactionLimit: number;
  processingWindow: string;
  processingWindowTimezone: string;
}

export interface ICustomerAccount {
  number: string;
  name: string;
  routingNumber: string;
  bankCode: string;
  cisNumbers: string[];
  accountSettings: IAccountSettings;
  billingAccount: boolean;
  enabled: boolean;
  products: ICustomerProduct[];
}

export interface IAccountSettings {
  transactionLimit: number;
  cumulativeTransactionLimit: number;
}

export interface ICustomerProduct {
  id: string | null;
  name: string;
  friendlyName: string | null;
  shortName: string | null;
  description: string | null;
  productSettings: IProductSettings | null;
  enabled: boolean;
  billable: boolean;
  resources: IProductResource[];
  paymentRails: IPaymentRail[] | null;
}

export interface IProductSettings {
  transactionLimit?: number;
  cumulativeTransactionLimit?: number;
  duplicateCheckDuration?: number;
}

export interface IProductResource {
  name: string;
  friendlyName: string | null;
  description: string | null;
  enabled: boolean;
  billable: boolean;
}

export interface IPaymentRail {
  name: string;
  friendlyName: string | null;
  description: string | null;
  paymentRailSettings: IPaymentRailSettings;
  enabled: boolean;
  billable: boolean;
}

export interface IPaymentRailSettings {
  transactionLimit: number;
  cumulativeTransactionLimit: number;
  duplicateCheckDuration: number;
  allowedCreditAccountList?: IAllowedCreditAccount[];
}

export interface IAllowedCreditAccount {
  accountNumber: string;
  routingNumber: string | null;
}

export interface ICustomerContact {
  contactName: string;
  contactTitle: string;
  contactPhone: string;
  contactPhoneType: string;
  contactEmail: string;
  contactPreferredType: string;
  contactType: string;
  enabled: boolean;
}

export interface IcustomerProps {
  customer: ICustomerData;
  setCustomer: React.Dispatch<React.SetStateAction<ICustomerData>>;
  disabled: boolean;
  gatewayCustomerId?: any;
  mode?: any;
}



export const CustomerProfile = () => {
  const { myTasks, customers } = useCustomer();
  const location = useLocation();
  const navigate = useNavigate();

  const searchParams = new URLSearchParams(location.search);
  const mode = searchParams.get('mode');
  const role = searchParams.get('role');
  const isDraft = searchParams.get('draft');
  const gatewayCustomerId = searchParams.get('gatewayCustomerId');

  const isViewMode = !mode || mode === 'view';

  const customerData = isDraft ? myTasks : customers;

  const initialCustomerData: ICustomerData = {
    gatewayCustomerId: gatewayCustomerId || '',
    name: '',
    cisNumbers: [],
    enabled: false,
    customerSettings: {
      transactionLimit: 0,
      cumulativeTransactionLimit: 0,
      processingWindow: '00:00-23:59',
      processingWindowTimezone: 'CST',
    },
    customerType: 'COMMERCIAL',
    virtualAcctCustomer: false,
    demoCustomer: false,
    customerAccounts: [],
    customerProducts: [],
    billingCustomerId: '',
    createdBy: '',
    customerContacts: [],
  };

  const [customer, setCustomer] = useState<ICustomerData>(initialCustomerData);
  const [loading, setLoading] = useState<boolean>(true); 

  // Redirect to home if invalid query params
  useEffect(() => {
    if (
      !mode ||
      !role ||
      (role === 'viewer' && mode !== 'view') ||
      (role === 'editor' && mode === 'create')
    ) {
      navigate('/');
    }
  }, [mode, role, navigate]);

  // Fetch customer data
  useEffect(() => {
    const fetchCustomer = async () => {
      try {
        if ((role === 'viewer' || role === 'editor') && gatewayCustomerId) {
          const customerServiceApi = new CustomerService();
          const traceId = uuidv4();

          customerServiceApi.addGetCustomerHeader(gatewayCustomerId, traceId);
          const response = await customerServiceApi.getCustomer();

          if (!response?.data) {
            navigate('/');
            return;
          }

          setCustomer(response.data as ICustomerData);
        } else if (mode === 'edit' && gatewayCustomerId) {
          const existingCustomer = customerData.find(
            (c: any) => c.gatewayCustomerId === gatewayCustomerId
          );
          if (existingCustomer) {
            setCustomer(existingCustomer as any);
          } else {
            navigate('/');
            return;
          }
        }
      } catch (error) {
        console.error('Failed to fetch customer data:', error);
        navigate('/');
      } finally {
        setLoading(false); 
      }
    };

    fetchCustomer();
  }, [role, gatewayCustomerId, mode, customerData, navigate]);

  
  if (loading) {
    return <div style={{ padding: '2rem' }}><PageSpinner/></div>; 
  }

  // Approver view
  if (role === 'approver' && mode === 'edit') {
    return <ApproverView />;
  }

  // Main profile form
  return (
    <Box className="main-profile">
      <CustomerInfo
        customer={customer}
        setCustomer={setCustomer}
        disabled={isViewMode}
        mode={mode}
        gatewayCustomerId={gatewayCustomerId}
      />
      <BusinessContact
        customer={customer}
        setCustomer={setCustomer}
        disabled={isViewMode}
      />
      <TechnicalContact
        customer={customer}
        setCustomer={setCustomer}
        disabled={isViewMode}
      />
      <TransactionLimit
        customer={customer}
        setCustomer={setCustomer}
        disabled={isViewMode}
      />
      <ApiCustomerConfig
        customer={customer}
        setCustomer={setCustomer}
        disabled={isViewMode}
      />
      <AccountPreference
        customer={customer}
        setCustomer={setCustomer}
        disabled={isViewMode}
      />
      {!isViewMode && (
        <ActionButton
          customer={customer}
          setCustomer={setCustomer}
          disabled={isViewMode}
          gatewayCustomerId={gatewayCustomerId}
        />
      )}
    </Box>
  );
};




{
    "gatewayCustomerId": "755387fb-e20f-4b32-b99b-9c6f242a021e",
    "name": "Charizard02",
    "cisNumbers": [
        "2999921105"
    ],
    "enabled": true,
    "customerSettings": {
        "transactionLimit": 10000,
        "cumulativeTransactionLimit": null,
        "processingWindow": null,
        "processingWindowTimezone": null
    },
    "customerType": "COMMERCIAL",
    "virtualAcctCustomer": true,
    "demoCustomer": false,
    "customerAccounts": [
        {
            "number": "7857200085",
            "name": "5setHoganAccount9",
            "routingNumber": "072000096",
            "bankCode": "10002",
            "cisNumbers": [
                "2999921105"
            ],
            "accountSettings": {
                "transactionLimit": null,
                "cumulativeTransactionLimit": null
            },
            "billingAccount": false,
            "enabled": true,
            "products": [
                {
                    "id": null,
                    "name": "INSTANT_PAYMENTS_API",
                    "friendlyName": "Instant Payments API",
                    "shortName": "IPA",
                    "description": "Provides various resources to manage instant payments api",
                    "productSettings": {},
                    "enabled": true,
                    "billable": true,
                    "resources": [
                        {
                            "name": "CREATE_CREDIT_TRANSFER",
                            "friendlyName": "Create Credit Transfer",
                            "description": "Help to initiate credit transfer request",
                            "enabled": true,
                            "billable": true
                        },
                        {
                            "name": "RETRIEVE_CREDIT_TRANSFER",
                            "friendlyName": "Retrieve Credit Transfer",
                            "description": "Help to retrieve the status of the credit transfer request",
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
                                "duplicateCheckDuration": 0,
                                "allowedCreditAccountList": [
                                    {
                                        "accountNumber": "84324377",
                                        "routingNumber": "072000096"
                                    },
                                    {
                                        "accountNumber": "943243277",
                                        "routingNumber": "121137522"
                                    }
                                ]
                            },
                            "enabled": true,
                            "billable": true
                        },
                        {
                            "name": "FEDNOW",
                            "friendlyName": null,
                            "description": null,
                            "paymentRailSettings": {
                                "duplicateCheckDuration": 0
                            },
                            "enabled": true,
                            "billable": true
                        }
                    ]
                }
            ]
        },
        {
            "number": "7857200086",
            "name": "5setHoganAccount9",
            "routingNumber": "072000096",
            "bankCode": "10002",
            "cisNumbers": [
                "2999921105"
            ],
            "accountSettings": {
                "transactionLimit": null,
                "cumulativeTransactionLimit": null
            },
            "billingAccount": false,
            "enabled": true,
            "products": [
                {
                    "id": null,
                    "name": "INSTANT_PAYMENTS_API",
                    "friendlyName": "Instant Payments API",
                    "shortName": "IPA",
                    "description": "Provides various resources to manage instant payments api",
                    "productSettings": {},
                    "enabled": true,
                    "billable": true,
                    "resources": [
                        {
                            "name": "CREATE_CREDIT_TRANSFER",
                            "friendlyName": "Create Credit Transfer",
                            "description": "Help to initiate credit transfer request",
                            "enabled": true,
                            "billable": true
                        },
                        {
                            "name": "RETRIEVE_CREDIT_TRANSFER",
                            "friendlyName": "Retrieve Credit Transfer",
                            "description": "Help to retrieve the status of the credit transfer request",
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
                                "duplicateCheckDuration": 0,
                                "allowedCreditAccountList": [
                                    {
                                        "accountNumber": "84324377",
                                        "routingNumber": "072000096"
                                    },
                                    {
                                        "accountNumber": "943243277",
                                        "routingNumber": "121137522"
                                    }
                                ]
                            },
                            "enabled": true,
                            "billable": true
                        },
                        {
                            "name": "FEDNOW",
                            "friendlyName": null,
                            "description": null,
                            "paymentRailSettings": {
                                "duplicateCheckDuration": 0
                            },
                            "enabled": true,
                            "billable": true
                        }
                    ]
                }
            ]
        },
        {
            "number": "7857200087",
            "name": "5setHoganAccount9",
            "routingNumber": "072000096",
            "bankCode": "10002",
            "cisNumbers": [
                "2999921105"
            ],
            "accountSettings": {
                "transactionLimit": null,
                "cumulativeTransactionLimit": null
            },
            "billingAccount": false,
            "enabled": true,
            "products": [
                {
                    "id": null,
                    "name": "INSTANT_PAYMENTS_API",
                    "friendlyName": "Instant Payments API",
                    "shortName": "IPA",
                    "description": "Provides various resources to manage instant payments api",
                    "productSettings": {},
                    "enabled": true,
                    "billable": true,
                    "resources": [
                        {
                            "name": "CREATE_CREDIT_TRANSFER",
                            "friendlyName": "Create Credit Transfer",
                            "description": "Help to initiate credit transfer request",
                            "enabled": true,
                            "billable": true
                        },
                        {
                            "name": "RETRIEVE_CREDIT_TRANSFER",
                            "friendlyName": "Retrieve Credit Transfer",
                            "description": "Help to retrieve the status of the credit transfer request",
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
                                "duplicateCheckDuration": 0,
                                "allowedCreditAccountList": [
                                    {
                                        "accountNumber": "84324377",
                                        "routingNumber": "072000096"
                                    },
                                    {
                                        "accountNumber": "943243277",
                                        "routingNumber": "121137522"
                                    }
                                ]
                            },
                            "enabled": true,
                            "billable": true
                        },
                        {
                            "name": "FEDNOW",
                            "friendlyName": null,
                            "description": null,
                            "paymentRailSettings": {
                                "duplicateCheckDuration": 0
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
            "name": "INSTANT_PAYMENTS_API",
            "friendlyName": "Instant Payments API",
            "shortName": "IPA",
            "description": "Provides various resources to manage instant payments api",
            "productSettings": {},
            "enabled": true,
            "billable": true,
            "resources": [
                {
                    "name": "CREATE_CREDIT_TRANSFER",
                    "friendlyName": "Create Credit Transfer",
                    "description": "Help to initiate credit transfer request",
                    "enabled": true,
                    "billable": true
                },
                {
                    "name": "RETRIEVE_CREDIT_TRANSFER",
                    "friendlyName": "Retrieve Credit Transfer",
                    "description": "Help to retrieve the status of the credit transfer request",
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
                        "duplicateCheckDuration": 0,
                        "allowedCreditAccountList": [
                            {
                                "accountNumber": "84324377",
                                "routingNumber": "072000096"
                            },
                            {
                                "accountNumber": "943243277",
                                "routingNumber": "121137522"
                            }
                        ]
                    },
                    "enabled": true,
                    "billable": true
                },
                {
                    "name": "FEDNOW",
                    "friendlyName": null,
                    "description": null,
                    "paymentRailSettings": {
                        "duplicateCheckDuration": 0
                    },
                    "enabled": true,
                    "billable": true
                }
            ]
        }
    ],
    "billingCustomerId": "IPABILL234",
    "createdBy": "CreateCustomerAPI",
    "customerContacts": []
}
