import React, { useEffect, useState } from 'react';
import { IcustomerProps } from '../../customer-profile';
import { Box, Checkbox, Typography } from '@ucl/ui-components';
import CardCheckbox from '../../../../components/checkbox-menu/checkbox-menu';
import '../../customer-profile.css';

const ApiCustomerConfig = ({
  disabled,
  customer,
  setCustomer,
}: IcustomerProps) => {
  const api_general_options = [
    { id: 'acc_balance', label: 'Account Balance', value: 'Account Balance' },
  ];

  const api_rtp_options = [
    {
      id: 'ipa_create_credit',
      label: 'Create Credit & Retrieve Status',
      value: 'Instant Payments - Create Credit',
    },
  ];

  const api_fednow_options = [
    {
      id: 'fednow_createcredit',
      label: 'Create Credit & Retrieve Status',
      value: 'FedNow - Create Credit',
    },
  ];

  const api_wire_options = [
    {
      id: 'wire_create_credit',
      label: 'Create Credit  & Retrieve Status',
      value: 'Wire - Create Credit',
    },
    {
      id: 'wire_webhook',
      label: 'Webhook Status Update',
      value: 'Wire - Webhook Retrieve',
    },
  ];

  const [apiGeneralValues, setApiGeneralValues] = useState<string[]>([]);
  const [apiRTPValues, setApiRTPValues] = useState<string[]>([]);
  const [apiFedNowValues, setApiFedNowValues] = useState<string[]>([]);
  const [apiWiresValues, setApiWiresValues] = useState<string[]>([]);
  const [isAllSelected, setIsAllSelected] = useState<boolean>(false);

  useEffect(() => {
    if (!customer?.customerProducts) return;

    let general = false;
    let rtp = false;
    let fednow = false;
    let wires = false;

    customer?.customerProducts?.forEach((product: any) => {
      // GENERAL (ACCOUNT_BALANCE_API)
      if (product.name === 'ACCOUNT_BALANCE_API') {
        const retrieve = product.resources?.some(
          (r: any) => r.name === 'RETRIEVE_ACCOUNT_BALANCE' && r.enabled
        );
        if (retrieve) general = true;
      }

      // RTP / FEDNOW / WIRES (INSTANT_PAYMENTS_API)
      if (product.name === 'INSTANT_PAYMENTS_API') {
        const createTransfer = product.resources?.some(
          (r: any) => r.name === 'CREATE_CREDIT_TRANSFER' && r.enabled
        );

        if (createTransfer && product.paymentRails?.length) {
          product.paymentRails.forEach((rail: any) => {
            if (rail.enabled) {
              if (rail.name === 'RTP') rtp = true;
              if (rail.name === 'FEDNOW') fednow = true;
              if (rail.name === 'WIRES') wires = true;
            }
          });
        }
      }
    });

    setApiGeneralValues(general ? api_general_options.map((o) => o.id) : []);
    setApiRTPValues(rtp ? api_rtp_options.map((o) => o.id) : []);
    setApiFedNowValues(fednow ? api_fednow_options.map((o) => o.id) : []);
    setApiWiresValues(wires ? api_wire_options.map((o) => o.id) : []);
  }, [customer]);

  useEffect(() => {
    const allSelected =
      apiGeneralValues.length === api_general_options.length &&
      apiRTPValues.length === api_rtp_options.length &&
      apiFedNowValues.length === api_fednow_options.length &&
      apiWiresValues.length === api_wire_options.length;

    setIsAllSelected(allSelected);
  }, [
    apiGeneralValues,
    apiRTPValues,
    apiFedNowValues,
    apiWiresValues,
    api_general_options.length,
    api_rtp_options.length,
    api_fednow_options.length,
    api_wire_options.length,
  ]);

  const handleSelectAll = (checked: boolean) => {
    if (checked) {
      setApiGeneralValues(api_general_options.map((c) => c.id));
      setApiRTPValues(api_rtp_options.map((c) => c.id));
      setApiFedNowValues(api_fednow_options.map((c) => c.id));
      setApiWiresValues(api_wire_options.map((c) => c.id));
    } else {
      setApiGeneralValues([]);
      setApiRTPValues([]);
      setApiFedNowValues([]);
      setApiWiresValues([]);
    }
    setIsAllSelected(checked);
  };

  const handleChange =
    (setter: React.Dispatch<React.SetStateAction<string[]>>) =>
    (values: string[]) => {
      setter(values);
    };

  return (
    <Box className="section">
      <Box className="group-head">
        <Typography variant="h3" className="main-header" fontStyle="italic">
          Api
        </Typography>
        <Checkbox
          label="Select all"
          disabled={disabled}
          checked={isAllSelected}
          onChange={(e: any) => handleSelectAll(e.target.checked)}
        />
      </Box>

      <Box className="checkbox-container sub-section">
        <CardCheckbox
          title="General"
          checkboxes={api_general_options}
          disabled={disabled}
          selectedValues={apiGeneralValues}
          onChange={handleChange(setApiGeneralValues)}
        />
        <CardCheckbox
          title="US RTP"
          checkboxes={api_rtp_options}
          disabled={disabled}
          selectedValues={apiRTPValues}
          onChange={handleChange(setApiRTPValues)}
        />
        <CardCheckbox
          title="FedNow"
          disabled={disabled}
          checkboxes={api_fednow_options}
          selectedValues={apiFedNowValues}
          onChange={handleChange(setApiFedNowValues)}
        />
        <CardCheckbox
          title="Wire"
          disabled={disabled}
          checkboxes={api_wire_options}
          selectedValues={apiWiresValues}
          onChange={handleChange(setApiWiresValues)}
        />
      </Box>
    </Box>
  );
};

export default ApiCustomerConfig;


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
