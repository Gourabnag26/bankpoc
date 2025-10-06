import React, { useEffect, useState } from 'react';
import {
  Box,
  Button,
  Checkbox,
  Input,
  Select,
  Typography,
} from '@ucl/ui-components';
import CardCheckbox from '../checkbox-menu/checkbox-menu';

const AccountPopup = ({account,disabled}:any) => {
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

  const [isAllSelected, setIsAllSelected] = useState(false);
  

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

  const handleSelectAllChange = (checked: boolean) => {
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

  const sx = {
    width: '250px',
    height: '40px',
    margin: '10px',
  };
  return (
    <>
      <Box className="section">
        <Typography
          variant="body1"
          className="main-header"
          fontStyle="italic"
          sx={{
            fontSize: '21px',
            fontStyle: 'italic',
          }}
        >
          Account Info
        </Typography>
        <Box sx={{ padding: '10px' }}>
          <Box
            sx={{
              display: 'flex',
            }}
          >
            <Input
              className="main-input"
              titleLabel="Account Name"
              value={account?.name}
              disabled={disabled}
              placeholder="Account Name"
              sx={sx}
            />
            <Input
              className="main-input"
              titleLabel="Account Number"
              placeholder="Account Number"
              value={account?.number}
              disabled={disabled}
              sx={sx}
            />
            <Select
              className="main-input"
              title="Bank Code"
              value={account?.bankCode}
              disabled={disabled}
              sx={sx}
              options={[{ key: '10002', value: '10002', text: '10002' }]}
            />
          </Box>
          <Box
            sx={{
              display: 'flex',
              alignItems: 'center',
            }}
          >
            <Input
              className="main-input"
              titleLabel="Routing Number"
              placeholder="Routing Number"
              value={account?.routingNumber}
              disabled={disabled}
              sx={sx}
            />
            <div style={{ marginLeft: '10px' }}></div>
            <Checkbox disabled={disabled} checked={account?.billingAccount} sx={sx} label="Billing Account" />
          </Box>
        </Box>
      </Box>

      <Box className="section">
        <Typography
          variant="body1"
          className="main-header"
          fontStyle="italic"
          sx={{
            fontSize: '21px',
            fontStyle: 'italic',
          }}
        >
          Limits
        </Typography>
        <Box sx={{ padding: '10px' }}>
          <Box
            sx={{
              display: 'flex',
            }}
          >
            <Input
              className="main-input"
              titleLabel="Transaction Limit"
              placeholder="Transaction Limit"
              value={account?.accountSettings?.cumulativeTransactionLimit}
              disabled={disabled}
              sx={sx}
            />
            <Input
              className="main-input"
              titleLabel="Daily Limit"
              placeholder="Daily Limit"
              value={account?.accountSettings?.transactionLimit}
              disabled={disabled}
              isOptionalLabel={true}
              sx={sx}
            />
          </Box>
        </Box>
      </Box>

      {/* API Section */}
      <Box className="section">
        <Box sx={{ display: 'flex', alignItems: 'center', gap: '30px' }}>
          <Typography
            variant="body1"
            className="main-header"
            fontStyle="italic"
            sx={{
              fontSize: '21px',
              fontStyle: 'italic',
            }}
          >
            Api
          </Typography>
          <Checkbox
            label="Select all"
            checked={isAllSelected}
            onChange={(e: any) => handleSelectAllChange(e.target.checked)}
          />
        </Box>
        <Box
          className="checkbox-container sub-section"
          sx={{
            display: 'flex',
            flexDirection: 'row',
            gap: '10px',
            padding: '10px',
            alignItems: 'center',
            flexWrap: 'wrap',
          }}
        >
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

      {/* Buttons */}
      {!disabled && <Box
        sx={{
          display: 'flex',
          justifyContent: 'center',
          gap: '10px',
          marginTop: '10px',
        }}
      >
        <Button
          sx={{
            border: '1px solid black',
            background: '#F5F5F5',
            padding: '10px',
            fontSize: '12px',
            color: 'black',
            width: '150px',
            borderRadius: '5px',
          }}
        >
          Save
        </Button>
        <Button
          sx={{
            border: '1px solid black',
            background: '#F5F5F5',
            padding: '10px',
            fontSize: '12px',
            color: 'black',
            width: '150px',
            borderRadius: '5px',
          }}
        >
          Cancel
        </Button>
      </Box>}
    </>
  );
};

export default AccountPopup;
