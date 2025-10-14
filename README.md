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

  // INITIALIZE UI FROM CUSTOMER DATA
  useEffect(() => {
    if (!customer?.customerProducts) return;

    let general = false;
    let rtp = false;
    let fednow = false;
    let wires = false;

    customer.customerProducts.forEach((product) => {
      if (product.name === 'ACCOUNT_BALANCE_API') {
        const retrieve = product.resources?.some(
          (r) => r.name === 'RETRIEVE_ACCOUNT_BALANCE' && r.enabled
        );
        if (retrieve) general = true;
      }

      if (product.name === 'INSTANT_PAYMENTS_API') {
        const createTransfer = product.resources?.some(
          (r) => r.name === 'CREATE_CREDIT_TRANSFER' && r.enabled
        );
        if (createTransfer && product.paymentRails?.length) {
          product.paymentRails.forEach((rail) => {
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

  const updateCustomerProducts = (
    category: 'GENERAL' | 'RTP' | 'FEDNOW' | 'WIRES',
    selected: string[]
  ) => {
    setCustomer((prev) => {
      let updatedProducts = [...prev.customerProducts];

      const hasSelection = selected.length > 0;

      if (category === 'GENERAL') {
        if (hasSelection) {
          if (!updatedProducts.some((p) => p.name === 'ACCOUNT_BALANCE_API')) {
            updatedProducts.push({
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
            });
          }
        } else {
          updatedProducts = updatedProducts.filter(
            (p) => p.name !== 'ACCOUNT_BALANCE_API'
          );
        }
      } else {
        let ipaProduct = updatedProducts.find(
          (p) => p.name === 'INSTANT_PAYMENTS_API'
        );

        if (!ipaProduct && hasSelection) {
          ipaProduct = {
            id: null,
            name: 'INSTANT_PAYMENTS_API',
            friendlyName: null,
            shortName: null,
            description: null,
            productSettings: null,
            enabled: true,
            billable: true,
            resources: [],
            paymentRails: [],
          };
          updatedProducts.push(ipaProduct);
        }

        if (ipaProduct) {
          const railName =
            category === 'RTP'
              ? 'RTP'
              : category === 'FEDNOW'
              ? 'FEDNOW'
              : 'WIRES';

          ipaProduct.paymentRails = ipaProduct.paymentRails || [];

          // Add CREATE_CREDIT_TRANSFER resource if missing
          if (
            hasSelection &&
            !ipaProduct.resources.some(
              (r) => r.name === 'CREATE_CREDIT_TRANSFER'
            )
          ) {
            ipaProduct.resources.push({
              name: 'CREATE_CREDIT_TRANSFER',
              friendlyName: null,
              description: null,
              enabled: true,
              billable: true,
            });
          }

          if (!hasSelection) {
            ipaProduct.paymentRails = ipaProduct.paymentRails.filter(
              (rail) => rail.name !== railName
            );
          } else {
            const existingRail = ipaProduct.paymentRails.find(
              (rail) => rail.name === railName
            );
            if (!existingRail) {
              ipaProduct.paymentRails.push({
                name: railName,
                friendlyName: null,
                description: null,
                enabled: true,
                billable: true,
                paymentRailSettings: {
                  transactionLimit: 0,
                  cumulativeTransactionLimit: 0,
                  duplicateCheckDuration: 0,
                },
              });
            }
          }

          // Clean up if IPA has no rails or resources left
          if (
            ipaProduct.paymentRails.length === 0 &&
            ipaProduct.resources.length === 0
          ) {
            updatedProducts = updatedProducts.filter(
              (p) => p.name !== 'INSTANT_PAYMENTS_API'
            );
          }
        }
      }

      return {
        ...prev,
        customerProducts: updatedProducts,
      };
    });
  };

  const handleSelectAll = (checked: boolean) => {
    const general = checked ? api_general_options.map((c) => c.id) : [];
    const rtp = checked ? api_rtp_options.map((c) => c.id) : [];
    const fednow = checked ? api_fednow_options.map((c) => c.id) : [];
    const wires = checked ? api_wire_options.map((c) => c.id) : [];

    setApiGeneralValues(general);
    setApiRTPValues(rtp);
    setApiFedNowValues(fednow);
    setApiWiresValues(wires);

    updateCustomerProducts('GENERAL', general);
    updateCustomerProducts('RTP', rtp);
    updateCustomerProducts('FEDNOW', fednow);
    updateCustomerProducts('WIRES', wires);

    setIsAllSelected(checked);
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
          onChange={(e) => handleSelectAll(e.target.checked)}
        />
      </Box>

      <Box className="checkbox-container sub-section">
        <CardCheckbox
          title="General"
          checkboxes={api_general_options}
          disabled={disabled}
          selectedValues={apiGeneralValues}
          onChange={(values) => {
            setApiGeneralValues(values);
            updateCustomerProducts('GENERAL', values);
          }}
        />
        <CardCheckbox
          title="US RTP"
          checkboxes={api_rtp_options}
          disabled={disabled}
          selectedValues={apiRTPValues}
          onChange={(values) => {
            setApiRTPValues(values);
            updateCustomerProducts('RTP', values);
          }}
        />
        <CardCheckbox
          title="FedNow"
          checkboxes={api_fednow_options}
          disabled={disabled}
          selectedValues={apiFedNowValues}
          onChange={(values) => {
            setApiFedNowValues(values);
            updateCustomerProducts('FEDNOW', values);
          }}
        />
        <CardCheckbox
          title="Wire"
          checkboxes={api_wire_options}
          disabled={disabled}
          selectedValues={apiWiresValues}
          onChange={(values) => {
            setApiWiresValues(values);
            updateCustomerProducts('WIRES', values);
          }}
        />
      </Box>
    </Box>
  );
};

export default ApiCustomerConfig;
