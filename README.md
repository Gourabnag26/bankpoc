import React, { useEffect, useState } from 'react';
import { Box } from '@ucl/ui-components';
import { useLocation } from 'react-router-dom';
import CustomerInfo from './components/customer-info/customer-info';
import BusinessContact from './components/business-contact/business-contact';
import TechnicalContact from './components/technical-contact/technical-contact';
import TransactionLimit from './components/transaction-limit/transaction-limit';
import ApiCustomerConfig from './components/api-customer/api-customer';
import AccountPreference from './components/account-preference/account-preference';
import ActionButton from './components/action-button/action-button';
import { useCustomer } from '../../context/customer-context';
import ApproverView from './approver-view/approver-view';
import { useNavigate } from 'react-router-dom';
import { CustomerService } from 'shared';
import { get } from 'node:http';
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
  const { myTasks, setMyTasks, customers } = useCustomer();
  const location = useLocation();
  const searchParams = new URLSearchParams(location.search);
  const mode = searchParams.get('mode');
  const role = searchParams.get('role');
  const isDraft = searchParams.get('draft');
  const isViewMode = !mode || mode === 'view';
  const gatewayCustomerId = searchParams.get('gatewayCustomerId');
  const customerData = isDraft ? myTasks : customers;
  const navigate = useNavigate();

  const currentCustomer = customerData.find(
    (customer: any) => customer.gatewayCustomerId === gatewayCustomerId
  );

  // testing view mode with real time data
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

  const apiCustomerData: ICustomerData = {
    gatewayCustomerId: 'e659aa04-65fc-42f4-8133-32b9cf9040e8',
    name: 'Test',
    cisNumbers: ['100100100155'],
    enabled: true,
    customerSettings: {
      transactionLimit: 1000000,
      cumulativeTransactionLimit: 5000000,
      processingWindow: '00:00-23:59',
      processingWindowTimezone: 'CST',
    },
    customerType: 'COMMERCIAL',
    virtualAcctCustomer: false,
    demoCustomer: true,
    customerAccounts: [
      {
        number: '1000100010055',
        name: 'CBNK00001_00008',
        routingNumber: '072000096',
        bankCode: '10002',
        cisNumbers: ['100100100155'],
        accountSettings: {
          transactionLimit: 100,
          cumulativeTransactionLimit: 10000,
        },
        billingAccount: true,
        enabled: true,
        products: [
          {
            id: null,
            name: 'INSTANT_PAYMENTS_API',
            friendlyName: null,
            shortName: null,
            description: null,
            productSettings: null,
            enabled: true,
            billable: true,
            resources: [
              {
                name: 'CREATE_CREDIT_TRANSFER',
                friendlyName: null,
                description: null,
                enabled: true,
                billable: true,
              },
              {
                name: 'RETRIEVE_CREDIT_TRANSFER',
                friendlyName: null,
                description: null,
                enabled: true,
                billable: true,
              },
            ],
            paymentRails: [
              {
                name: 'RTP',
                friendlyName: null,
                description: null,
                paymentRailSettings: {
                  transactionLimit: 100,
                  cumulativeTransactionLimit: 10000,
                  duplicateCheckDuration: 0,
                  allowedCreditAccountList: [
                    {
                      accountNumber:
                        '{"accountNumber": "123456789", "routingNumber": "123456789"}',
                      routingNumber: null,
                    },
                  ],
                },
                enabled: true,
                billable: true,
              },
            ],
          },
        ],
      },
    ],
    customerProducts: [
      {
        id: null,
        name: 'ACCOUNT_BALANCE_API',
        friendlyName: null,
        shortName: null,
        description: null,
        productSettings: null,
        enabled: true,
        billable: true,
        resources: [
          {
            name: 'RETRIEVE_ACCOUNT_BALANCE',
            friendlyName: null,
            description: null,
            enabled: true,
            billable: true,
          },
        ],
        paymentRails: null,
      },
      {
        id: null,
        name: 'INSTANT_PAYMENTS_API',
        friendlyName: null,
        shortName: null,
        description: null,
        productSettings: null,
        enabled: true,
        billable: true,
        resources: [
          {
            name: 'CREATE_CREDIT_TRANSFER',
            friendlyName: null,
            description: null,
            enabled: true,
            billable: true,
          },
          {
            name: 'RETRIEVE_CREDIT_TRANSFER',
            friendlyName: null,
            description: null,
            enabled: true,
            billable: true,
          },
        ],
        paymentRails: [
          {
            name: 'RTP',
            friendlyName: null,
            description: null,
            paymentRailSettings: {
              transactionLimit: 500000,
              cumulativeTransactionLimit: 2000000,
              duplicateCheckDuration: 0,
            },
            enabled: true,
            billable: true,
          },
          {
            name: 'FEDNOW',
            friendlyName: null,
            description: null,
            paymentRailSettings: {
              transactionLimit: 500000,
              cumulativeTransactionLimit: 2000000,
              duplicateCheckDuration: 0,
            },
            enabled: true,
            billable: true,
          },
        ],
      },
    ],
    billingCustomerId: 'SHB1001005',
    createdBy: 'IPA',
    customerContacts: [
      {
        contactName: 'James',
        contactTitle: 'QA Engineer',
        contactPhone: '129-000-0002',
        contactPhoneType: 'MOBILE',
        contactEmail: 'james.murphy@comerica.com',
        contactPreferredType: 'Email',
        contactType: 'SECONDARY',
        enabled: false,
      },
      {
        contactName: 'Ben Reynold',
        contactTitle: 'Developer',
        contactPhone: '5309999999 ext 2345',
        contactPhoneType: 'WORK',
        contactEmail: 'ben.reynold@comerica.com',
        contactPreferredType: 'Phone',
        contactType: 'TERTIARY',
        enabled: false,
      },
    ],
  };
  const [customer, setCustomer] = useState<ICustomerData>(() => {
    if (role === 'viewer' || role === 'editor') {
      return apiCustomerData;
    } else {
      return initialCustomerData;
    }
  });
  const getCustomer=async()=>{
    const CustomerServiceApi= new CustomerService()
    const traceId= uuidv4();
    CustomerServiceApi.addGetCustomerHeader(gatewayCustomerId,traceId)
    const getCustomerDetails= await  CustomerServiceApi.getCustomer()
    if(!getCustomerDetails.data)
    {
      navigate('/')
    }
    return(getCustomerDetails)
  }
  useEffect(()=>{
    let CustomerData=getCustomer()
    
  },[])
  useEffect(() => {
    if (
      !mode ||
      !role ||
      (role === 'viewer' && mode !== 'view') ||
      (role === 'editor' && mode === 'create')
    ) {
      navigate('/');
    }
  }, [mode,role,navigate]);

  return (
    (role === 'approver' && mode === 'edit') 
    ? <ApproverView /> 
    : <Box className="main-profile">
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
