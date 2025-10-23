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
  setCustomer?: React.Dispatch<React.SetStateAction<ICustomerData>>;
  onClose?: () => void;
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

const AccountPopup = ({ account, disabled, customer, setCustomer, onClose }: AccountPopupProps) => {
  const api_general_options = [{ id: 'acc_balance', label: 'Account Balance', value: 'Account Balance' }];
  const api_rtp_options = [{ id: 'ipa_create_credit', label: 'Create Credit & Retrieve Status', value: 'Instant Payments - Create Credit' }];
  const api_fednow_options = [{ id: 'fednow_createcredit', label: 'Create Credit & Retrieve Status', value: 'FedNow - Create Credit' }];
  const api_wire_options = [{ id: 'wire_create_credit', label: 'Create Credit & Retrieve Status', value: 'Wire - Create Credit' }];

  const [accountState, setAccountState] = useState(account || {});
  const [apiSelection, setApiSelection] = useState<ApiSelectionState>({
    general: { selectedValue: [], disabled: true, title: "General", options: api_general_options },
    rtp: { selectedValue: [], disabled: true, title: "US RTP", options: api_rtp_options },
    fednow: { selectedValue: [], disabled: true, title: "FedNow", options: api_fednow_options },
    wires: { selectedValue: [], disabled: true, title: "Wire", options: api_wire_options },
  });
  const [isAllSelected, setIsAllSelected] = useState(false);

  // Init account values
  useEffect(() => {
    if (!account?.products) return;

    let general = true, rtp = true, fednow = true, wires = true;
    let disableGeneral = true, disableRtp = true, disableFedNow = true, disableWires = true;

    customer?.customerProducts.forEach((product: any) => {
      if (product.name === 'ACCOUNT_BALANCE_API' && product.resources?.some((r: any) => r.name === 'RETRIEVE_ACCOUNT_BALANCE' && r.enabled)) disableGeneral = false;
      if (product.name === 'INSTANT_PAYMENTS_API' && product.resources?.some((r: any) => r.name === 'CREATE_CREDIT_TRANSFER' && r.enabled) && product.paymentRails?.length) {
        product.paymentRails.forEach((rail: any) => {
          if (!rail.enabled) return;
          if (rail.name === 'RTP') disableRtp = false;
          if (rail.name === 'FEDNOW') disableFedNow = false;
          if (rail.name === 'WIRES') disableWires = false;
        });
      }
    });

    account?.products?.forEach((product: any) => {
      if (product.name === 'ACCOUNT_BALANCE_API' && product.resources?.some((r: any) => r.name === 'RETRIEVE_ACCOUNT_BALANCE' && r.enabled)) general = false;
      if (product.name === 'INSTANT_PAYMENTS_API' && product.resources?.some((r: any) => r.name === 'CREATE_CREDIT_TRANSFER' && r.enabled) && product.paymentRails?.length) {
        product.paymentRails.forEach((rail: any) => {
          if (!rail.enabled) return;
          if (rail.name === 'RTP') rtp = false;
          if (rail.name === 'FEDNOW') fednow = false;
          if (rail.name === 'WIRES') wires = false;
        });
      }
    });

    setApiSelection((prev) => ({
      general: { ...prev.general, selectedValue: general ? [] : api_general_options.map((o) => o.id), disabled: disableGeneral },
      rtp: { ...prev.rtp, selectedValue: rtp ? [] : api_rtp_options.map((o) => o.id), disabled: disableRtp },
      fednow: { ...prev.fednow, selectedValue: fednow ? [] : api_fednow_options.map((o) => o.id), disabled: disableFedNow },
      wires: { ...prev.wires, selectedValue: wires ? [] : api_wire_options.map((o) => o.id), disabled: disableWires },
    }));
  }, [account, customer]);

  // Select All logic
  useEffect(() => {
    const filteredData = Object.values(apiSelection).filter((api) => !api.disabled);
    setIsAllSelected(filteredData.length > 0 ? filteredData.every((api) => api.selectedValue.length > 0) : false);
  }, [apiSelection]);

  const handleSelectAllChange = (checked: boolean) => {
    setApiSelection((prev) => {
      const newSelection = { ...prev };
      Object.keys(newSelection).forEach((key) => {
        const api = newSelection[key as keyof ApiSelectionState];
        if (api.disabled) return;
        api.selectedValue = checked ? api.options.map((o) => o.id) : [];
      });
      return newSelection;
    });
    setIsAllSelected(checked);
  };

  const handleChange = (target: keyof ApiSelectionState) => {
    return (value: string[]) => setApiSelection((prev) => ({ ...prev, [target]: { ...prev[target], selectedValue: value } }));
  };

  // ✅ Save account to customer
  const handleSave = () => {
    if (!setCustomer) return;

    setCustomer((prev) => {
      const accounts = [...(prev.customerAccounts || [])];
      const index = accounts.findIndex((a) => a.number === accountState.number);

      if (index >= 0) {
        // Update existing
        accounts[index] = { ...accounts[index], ...accountState };
      } else {
        // Add new
        accounts.push(accountState);
      }
      return { ...prev, customerAccounts: accounts };
    });

    if (onClose) onClose();
  };

  const sx = { width: '250px', height: '40px', margin: '10px' };

  return (
    <>
      <Box className="section">
        <Typography variant="body1" className="main-header" fontStyle="italic">Account Info</Typography>
        <Box sx={{ padding: '10px', display: 'flex' }}>
          <Input titleLabel="Account Name" value={accountState?.name || ''} disabled={disabled} sx={sx} onChange={(e: any) => setAccountState({ ...accountState, name: e.target.value })}/>
          <Input titleLabel="Account Number" value={accountState?.number || ''} disabled={disabled} sx={sx} onChange={(e: any) => setAccountState({ ...accountState, number: e.target.value })}/>
          <Select title="Bank Code" value={accountState?.bankCode || ''} disabled={disabled} sx={sx} options={[{ key: '10002', value: '10002', text: '10002' }]} onChange={(val: any) => setAccountState({ ...accountState, bankCode: val })}/>
        </Box>
        <Box sx={{ display: 'flex', alignItems: 'center' }}>
          <Input titleLabel="Routing Number" value={accountState?.routingNumber || ''} disabled={disabled} sx={sx} onChange={(e: any) => setAccountState({ ...accountState, routingNumber: e.target.value })}/>
          <Checkbox label="Billing Account" checked={accountState?.billingAccount} disabled={disabled} onChange={(e: any) => setAccountState({ ...accountState, billingAccount: e.target.checked })} sx={sx}/>
        </Box>
      </Box>

      <Box className="section">
        <Typography variant="body1" className="main-header" fontStyle="italic">Limits</Typography>
        <Box sx={{ padding: '10px', display: 'flex' }}>
          <Input titleLabel="Transaction Limit" value={accountState?.accountSettings?.transactionLimit || ''} disabled={disabled} sx={sx} onChange={(e: any) => setAccountState({ ...accountState, accountSettings: { ...accountState.accountSettings, transactionLimit: e.target.value } })}/>
          <Input titleLabel="Daily Limit" value={accountState?.accountSettings?.cumulativeTransactionLimit || ''} disabled={disabled} sx={sx} onChange={(e: any) => setAccountState({ ...accountState, accountSettings: { ...accountState.accountSettings, cumulativeTransactionLimit: e.target.value } })}/>
        </Box>
      </Box>

      <Box className="section">
        <Box sx={{ display: 'flex', alignItems: 'center', gap: '30px' }}>
          <Typography variant="body1" className="main-header" fontStyle="italic">Api</Typography>
          <Checkbox label="Select all" checked={isAllSelected} onChange={(e: any) => handleSelectAllChange(e.target.checked)} disabled={disabled}/>
        </Box>
        <Box className="checkbox-container sub-section" sx={{ display: 'flex', flexWrap: 'wrap', gap: '10px', padding: '10px' }}>
          {Object.keys(apiSelection).map((key) => {
            const api = apiSelection[key as keyof ApiSelectionState];
            return (
              <CardCheckbox
                title={api.title}
                checkboxes={api.options}
                selectedValues={api.selectedValue}
                disabled={api.disabled || disabled}
                onChange={handleChange(key as keyof ApiSelectionState)}
              />
            );
          })}
        </Box>
      </Box>

      {!disabled && (
        <Box sx={{ display: 'flex', justifyContent: 'center', gap: '10px', marginTop: '10px' }}>
          <Button onClick={handleSave} sx={{ width: '150px' }}>Save</Button>
          <Button onClick={onClose} sx={{ width: '150px' }}>Cancel</Button>
        </Box>
      )}
    </>
  );
};

export default AccountPopup;
