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
import { ICustomerData } from 'src/pages/customer-profile/customer-profile';

type AccountPopupProps = {
  account?: any;
  disabled?: boolean;
  customer?: ICustomerData;
  onSave?: (updatedAccount: any) => void;
  onCancel?: () => void;
};

type ApiSelectionStateItem = {
  selectedValue: string[];
  disabled: boolean;
  title: string;
  options: any[];
};

type ApiSelectionState = {
  general: ApiSelectionStateItem;
  rtp: ApiSelectionStateItem;
  fednow: ApiSelectionStateItem;
  wires: ApiSelectionStateItem;
};

const AccountPopup = ({ account, disabled, customer, onSave, onCancel }: AccountPopupProps) => {
  const [accountState, setAccountState] = useState(account || {});

  const api_general_options = [{ id: 'acc_balance', label: 'Account Balance', value: 'Account Balance' }];
  const api_rtp_options = [{ id: 'ipa_create_credit', label: 'Create Credit & Retrieve Status', value: 'Instant Payments - Create Credit' }];
  const api_fednow_options = [{ id: 'fednow_createcredit', label: 'Create Credit & Retrieve Status', value: 'FedNow - Create Credit' }];
  const api_wire_options = [{ id: 'wire_create_credit', label: 'Create Credit & Retrieve Status', value: 'Wire - Create Credit' }];

  const [apiSelection, setApiSelection] = useState<ApiSelectionState>({
    general: { selectedValue: [], disabled: true, title: "General", options: api_general_options },
    rtp: { selectedValue: [], disabled: true, title: "US RTP", options: api_rtp_options },
    fednow: { selectedValue: [], disabled: true, title: "FedNow", options: api_fednow_options },
    wires: { selectedValue: [], disabled: true, title: "Wire", options: api_wire_options },
  });

  const [isAllSelected, setIsAllSelected] = useState(false);

  // Initialize API selection from customer and account
  useEffect(() => {
    if (!customer) return;

    const customerHasApi = (apiName: string, railName?: string) => {
      const product = customer.customerProducts?.find((p: any) => p.name === apiName);
      if (!product) return false;
      if (!railName) return product.resources?.some((r: any) => r.enabled) ?? false;
      return product.paymentRails?.some((r: any) => r.name === railName && r.enabled) ?? false;
    };

    const accountHasApi = (apiName: string, optionId: string, railName?: string) => {
      const product = accountState.products?.find((p: any) => p.name === apiName);
      if (!product) return false;
      if (!railName) return product.resources?.some((r: any) => r.name.toUpperCase().includes(optionId.toUpperCase()) && r.enabled) ?? false;
      const rail = product.paymentRails?.find((r: any) => r.name === railName && r.enabled);
      return !!rail;
    };

    setApiSelection({
      general: {
        ...apiSelection.general,
        disabled: !customerHasApi('ACCOUNT_BALANCE_API'),
        selectedValue: accountHasApi('ACCOUNT_BALANCE_API', 'acc_balance') ? ['acc_balance'] : [],
      },
      rtp: {
        ...apiSelection.rtp,
        disabled: !customerHasApi('INSTANT_PAYMENTS_API', 'RTP'),
        selectedValue: accountHasApi('INSTANT_PAYMENTS_API', 'ipa_create_credit', 'RTP') ? ['ipa_create_credit'] : [],
      },
      fednow: {
        ...apiSelection.fednow,
        disabled: !customerHasApi('INSTANT_PAYMENTS_API', 'FEDNOW'),
        selectedValue: accountHasApi('INSTANT_PAYMENTS_API', 'fednow_createcredit', 'FEDNOW') ? ['fednow_createcredit'] : [],
      },
      wires: {
        ...apiSelection.wires,
        disabled: !customerHasApi('INSTANT_PAYMENTS_API', 'WIRES'),
        selectedValue: accountHasApi('INSTANT_PAYMENTS_API', 'wire_create_credit', 'WIRES') ? ['wire_create_credit'] : [],
      },
    });
  }, [customer, accountState]);

  useEffect(() => {
    const filteredData = Object.values(apiSelection).filter(api => !api.disabled);
    setIsAllSelected(filteredData.length > 0 ? filteredData.every(api => api.selectedValue.length > 0) : false);
  }, [apiSelection]);

  const handleSelectAllChange = (checked: boolean) => {
    setApiSelection(prev => {
      const newSelection = { ...prev };
      Object.keys(newSelection).forEach(key => {
        const api = newSelection[key as keyof ApiSelectionState];
        if (!api.disabled) api.selectedValue = checked ? api.options.map(o => o.id) : [];
      });
      return newSelection;
    });
    setIsAllSelected(checked);
  };

  const handleChange = (target: keyof ApiSelectionState) => (value: string[]) => {
    setApiSelection(prev => ({
      ...prev,
      [target]: { ...prev[target], selectedValue: value }
    }));
  };

  const handleSave = () => {
    if (onSave) onSave(accountState);
    if (onCancel) onCancel?.();
  };

  const handleCancel = () => {
    if (onCancel) onCancel();
  };

  const sx = { width: '250px', height: '40px', margin: '10px' };

  return (
    <>
      <Box className="section">
        <Typography variant="body1" className="main-header" fontStyle="italic" sx={{ fontSize: '21px' }}>
          Account Info
        </Typography>
        <Box sx={{ padding: '10px', display: 'flex' }}>
          <Input titleLabel="Account Name" value={accountState?.name} disabled={disabled} sx={sx} onChange={(e) => setAccountState({ ...accountState, name: e.target.value })}/>
          <Input titleLabel="Account Number" value={accountState?.number} disabled={disabled} sx={sx} onChange={(e) => setAccountState({ ...accountState, number: e.target.value })}/>
          <Select title="Bank Code" value={accountState?.bankCode} disabled={disabled} sx={sx} options={[{ key: '10002', value: '10002', text: '10002' }]} onChange={(val) => setAccountState({ ...accountState, bankCode: val })}/>
        </Box>
        <Box sx={{ display: 'flex', alignItems: 'center' }}>
          <Input titleLabel="Routing Number" value={accountState?.routingNumber} disabled={disabled} sx={sx} onChange={(e) => setAccountState({ ...accountState, routingNumber: e.target.value })}/>
          <Checkbox disabled={disabled} checked={accountState?.billingAccount} sx={sx} label="Billing Account" onChange={(checked) => setAccountState({ ...accountState, billingAccount: checked })}/>
        </Box>
      </Box>

      <Box className="section">
        <Typography variant="body1" className="main-header" fontStyle="italic" sx={{ fontSize: '21px' }}>
          Limits
        </Typography>
        <Box sx={{ padding: '10px', display: 'flex' }}>
          <Input titleLabel="Transaction Limit" value={accountState?.accountSettings?.cumulativeTransactionLimit} disabled={disabled} sx={sx} onChange={(e) => setAccountState({ ...accountState, accountSettings: { ...accountState.accountSettings, cumulativeTransactionLimit: e.target.value }})}/>
          <Input titleLabel="Daily Limit" value={accountState?.accountSettings?.transactionLimit} disabled={disabled} sx={sx} onChange={(e) => setAccountState({ ...accountState, accountSettings: { ...accountState.accountSettings, transactionLimit: e.target.value }})}/>
        </Box>
      </Box>

      <Box className="section">
        <Box sx={{ display: 'flex', alignItems: 'center', gap: '30px' }}>
          <Typography variant="body1" className="main-header" fontStyle="italic" sx={{ fontSize: '21px' }}>Api</Typography>
          <Checkbox label="Select all" checked={isAllSelected} onChange={(e: any) => handleSelectAllChange(e.target.checked)} disabled={disabled}/>
        </Box>
        <Box className="checkbox-container sub-section" sx={{ display: 'flex', flexWrap: 'wrap', gap: '10px', padding: '10px' }}>
          {Object.keys(apiSelection).map(key => {
            const api = apiSelection[key as keyof ApiSelectionState];
            return (
              <CardCheckbox key={key} title={api.title} checkboxes={api.options} selectedValues={api.selectedValue} disabled={api.disabled || disabled} onChange={handleChange(key as keyof ApiSelectionState)}/>
            )
          })}
        </Box>
      </Box>

      {!disabled && (
        <Box sx={{ display: 'flex', justifyContent: 'center', gap: '10px', marginTop: '10px' }}>
          <Button sx={{ width: '150px' }} onClick={handleSave}>Save</Button>
          <Button sx={{ width: '150px' }} onClick={handleCancel}>Cancel</Button>
        </Box>
      )}
    </>
  );
};

export default AccountPopup;
