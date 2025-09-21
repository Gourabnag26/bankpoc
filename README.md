import React, { useEffect } from 'react';
import { useState } from 'react';
import { IcustomerProps } from '../../customer-profile';
import { Box, Checkbox, Select, Typography } from '@ucl/ui-components';
import CardCheckbox from '../../../../components/checkbox-menu/checkbox-menu';
import '../../customer-profile.css';
const ApiCustomerConfig = ({ customer, setCustomer }: IcustomerProps) => {
  const api_general_options = [
    { id: 'acc_balance', label: 'Account Balance', value: 'Account Balance' },
  ];
  const api_rtp_options = [
    {
      id: 'ipa_create_credit',
      label: 'Create Credit',
      value: 'Instant Payments - Create Credit',
    },
    {
      id: 'ipa_recieve_credit',
      label: 'Retrieve Status',
      value: 'Instant Payments - Receive Credit',
    },
  ];
  const api_fednow_options = [
    {
      id: 'fednow_createcredit',
      label: 'Create Credit',
      value: 'FedNow - Create Credit',
    },
    {
      id: 'fednow_recievecredit',
      label: 'Retrieve Status',
      value: 'FedNow - Receive Credit',
    },
  ];
  const api_wire_options = [
    {
      id: 'wire_create_credit',
      label: 'Create Credit',
      value: 'Wire - Create Credit',
    },
    {
      id: 'wire_retieve_credit',
      label: 'Retrieve Status',
      value: 'Wire - Retrieve Credit',
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
  }, [apiGeneralValues, apiRTPValues, apiFedNowValues, apiWiresValues]);

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
          checked={isAllSelected}
          onChange={(e: any) => handleSelectAll(e.target.checked)}
        />
      </Box>
      <Box className="checkbox-container sub-section">
        <CardCheckbox
          title="General"
          checkboxes={api_general_options}
          selectedValues={apiGeneralValues}
          onChange={setApiGeneralValues}
        />
        <CardCheckbox
          title="US RTP"
          checkboxes={api_rtp_options}
          selectedValues={apiRTPValues}
          onChange={setApiRTPValues}
        />
        <CardCheckbox
          title="FedNow"
          checkboxes={api_fednow_options}
          selectedValues={apiFedNowValues}
          onChange={setApiFedNowValues}
        />
        <CardCheckbox
          title="Wire"
          checkboxes={api_wire_options}
          selectedValues={apiWiresValues}
          onChange={setApiWiresValues}
        />
      </Box>
    </Box>
  );
};
export default ApiCustomerConfig;
