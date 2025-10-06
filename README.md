import React, { useEffect, useState } from 'react';
import { IcustomerProps } from '../../customer-profile';
import { Box, Checkbox, Typography } from '@ucl/ui-components';
import CardCheckbox from '../../../../components/checkbox-menu/checkbox-menu';
import '../../customer-profile.css';

const ApiCustomerConfig = ({ disabled }: IcustomerProps) => {
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
    }
   
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
          onChange={setApiGeneralValues}
        />
        <CardCheckbox
          title="US RTP"
          checkboxes={api_rtp_options}
          disabled={disabled}
          selectedValues={apiRTPValues}
          onChange={setApiRTPValues}
        />
        <CardCheckbox
          title="FedNow"
          disabled={disabled}
          checkboxes={api_fednow_options}
          selectedValues={apiFedNowValues}
          onChange={setApiFedNowValues}
        />
        <CardCheckbox
          title="Wire"
          disabled={disabled}
          checkboxes={api_wire_options}
          selectedValues={apiWiresValues}
          onChange={setApiWiresValues}
        />
      </Box>
    </Box>
  );
};
export default ApiCustomerConfig;




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
