import React, { useEffect, useState } from 'react';
import { IcustomerProps } from '../../customer-profile';
import { Box, Checkbox, Typography } from '@ucl/ui-components';
import CardCheckbox from '../../../../components/checkbox-menu/checkbox-menu';
import '../../customer-profile.css';

const ApiCustomerConfig = ({ disabled, data }: IcustomerProps & { data?: any }) => {
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

  // ✅ Load states from API JSON
  useEffect(() => {
    if (!data?.customerProducts) return;

    let general = false;
    let rtp = false;
    let fednow = false;
    let wires = false;

    data.customerProducts.forEach((product: any) => {
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

    // ✅ Apply checked states
    setApiGeneralValues(general ? api_general_options.map((o) => o.id) : []);
    setApiRTPValues(rtp ? api_rtp_options.map((o) => o.id) : []);
    setApiFedNowValues(fednow ? api_fednow_options.map((o) => o.id) : []);
    setApiWiresValues(wires ? api_wire_options.map((o) => o.id) : []);
  }, [data]);

  // ✅ Recalculate "Select All" whenever any group changes
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

  // ✅ Select All logic
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

  // ✅ Individual section toggle (keep Select All synced)
  const handleChange = (setter: React.Dispatch<React.SetStateAction<string[]>>) => (values: string[]) => {
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
