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
  setCustomer?: any;
}

type ApiSelectionStateItem = {
  selectedValue: string[];
  disabled: boolean;
  title: string;
  options: any[];
}

type ApiSelectionState = {
  general: ApiSelectionStateItem;
  rtp: ApiSelectionStateItem;
  fednow: ApiSelectionStateItem;
  wires: ApiSelectionStateItem;
}


const AccountPopup = ({ account, disabled, customer }: AccountPopupProps) => {
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
      label: 'Create Credit & Retrieve Status',
      value: 'Wire - Create Credit',
    },

  
  ];
  
  const [apiSelection, setApiSelection] = useState<ApiSelectionState>({
    general: {
      selectedValue: [],
      disabled: true,
      title: "General",
      options: api_general_options,
    },
    rtp: {
      selectedValue: [],
      disabled: true,
      title: "US RTP",
      options: api_rtp_options,
    },
    fednow: {
      selectedValue: [],
      disabled: true,
      title: "FedNow",
      options: api_fednow_options,
    },
    wires: {
      selectedValue: [],
      disabled: true,
      title: "Wire",
      options: api_wire_options,
    },
  })
  const [isAllSelected, setIsAllSelected] = useState(false);

  useEffect(() => {
    if (!account?.products) return;
    console.log({account, customer});
    let general = true;
    let rtp = true;
    let fednow = true;
    let wires = true;

    let disableGeneral = true;
    let disableRtp = true;
    let disableFedNow = true;
    let disableWires = true;

    customer?.customerProducts.forEach((product: any) => {
      if (product.name === 'ACCOUNT_BALANCE_API') {
        const retrieve = product.resources?.some(
          (r: any) => r.name === 'RETRIEVE_ACCOUNT_BALANCE' && r.enabled
        );
        if (retrieve) disableGeneral = false;
      }

      if (product.name === 'INSTANT_PAYMENTS_API') {
        const createTransfer = product.resources?.some(
          (r : any) => r.name === 'CREATE_CREDIT_TRANSFER' && r.enabled
        );
        if (createTransfer && product.paymentRails?.length) {
          product.paymentRails.forEach((rail : any) => {
            if (rail.enabled) {
              if (rail.name === 'RTP') disableRtp = false;
              if (rail.name === 'FEDNOW') disableFedNow = false;
              if (rail.name === 'WIRES') disableWires = false;
            }
          });
        }
      }
    });

    account?.products?.forEach((product: any) => {
      // GENERAL
      if (product.name === 'ACCOUNT_BALANCE_API') {
        const retrieve = product.resources?.some(
          (r: any) => r.name === 'RETRIEVE_ACCOUNT_BALANCE' && r.enabled
        );
        if (retrieve) general = false;
      }

      // RTP / FEDNOW / WIRES (INSTANT_PAYMENTS_API)
      if (product.name === 'INSTANT_PAYMENTS_API') {
        const createTransfer = product.resources?.some(
          (r: any) => r.name === 'CREATE_CREDIT_TRANSFER' && r.enabled
        );

        if (createTransfer && product.paymentRails?.length) {
          product.paymentRails.forEach((rail: any) => {
            if (rail.enabled) {
              if (rail.name === 'RTP') rtp = false;
              if (rail.name === 'FEDNOW') fednow = false;
              if (rail.name === 'WIRES') wires = false;
            }
          });
        }
      }
    });

    setApiSelection((prev) => ({
      general: {
      ...prev.general,
      selectedValue: general ? [] : api_general_options.map((o) => o.id),
      disabled: disableGeneral,
    },
    rtp: {
      ...prev.rtp,
      selectedValue: rtp ? [] : api_rtp_options.map((o) => o.id),
      disabled: disableRtp,
    },
    fednow: {
      ...prev.fednow,
      selectedValue: fednow ? [] : api_fednow_options.map((o) => o.id),
      disabled: disableFedNow,
    },
    wires: {
      ...prev.wires,
      selectedValue: wires ? [] : api_wire_options.map((o) => o.id),
      disabled: disableWires,
    },
    }));

  }, [account, customer]);

  useEffect(() => {
    const filteredData = Object.values(apiSelection).filter((api) => !api.disabled)
    const allSelected =
      filteredData.length > 0
        ? filteredData.every((api) => api.selectedValue.length > 0)
        : false;
    setIsAllSelected(allSelected);
  }, [apiSelection]);

  const handleSelectAllChange = (checked: boolean) => {
    setApiSelection((prev) => {
      const newSelection = {...prev};
      Object.keys(newSelection).forEach((key) => {
        const api = newSelection[key as keyof ApiSelectionState];
        if(api.disabled) return;
          api.selectedValue = checked ? api.options.map((o) => o.id) : [];
        });
      return newSelection;
    });
    setIsAllSelected(checked);
  };

  const handleChange = (target: keyof ApiSelectionState) => {
    return (value: string[]) => {
      setApiSelection((prev) => ({
        ...prev,
        [target]: {
          ...prev[target],
          selectedValue: value,
        }
      }));
    };
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
            <Checkbox
              disabled={disabled}
              checked={account?.billingAccount}
              sx={sx}
              label="Billing Account"
            />
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

      {/* ✅ API Section */}
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
            disabled={disabled}
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
            )
          })}
        </Box>
      </Box>

      {!disabled && (
        <Box
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
        </Box>
      )}
    </>
  );
};

export default AccountPopup;
