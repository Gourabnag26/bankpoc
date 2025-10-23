import React from 'react';
import { IcustomerProps } from '../../customer-profile';
import { Box, Input, Select, Typography } from '@ucl/ui-components';
import RadioGroup from '../../../../components/radio-group/radio-group';
import '../../customer-profile.css';

const CustomerInfo = ({
  customer,
  setCustomer,
  disabled,
  gatewayCustomerId,
  mode
}: IcustomerProps) => {

  // Handle field updates immutably
  const handleChange = (field: string, value: any) => {
    setCustomer((prev) => ({
      ...prev,
      [field]: value,
    }));
  };

  const handleCisNumberChange = (value: string) => {
    setCustomer((prev) => ({
      ...prev,
      cisNumbers: [value],
    }));
  };

  const handleDemoChange = (value: boolean) => {
    setCustomer((prev) => ({
      ...prev,
      demoCustomer: value,
    }));
  };

  return (
    <Box className="section">
      <Typography variant="h3" className="main-header" fontStyle="italic">
        Customer Info
      </Typography>

      <Box className="sub-section">
        <Typography variant="subtitle1" fontStyle="italic" className="sub-head">
          {`Gateway Customer Id : ${gatewayCustomerId || 'N/A'}`}
        </Typography>

        {/* --- Customer Name and CIS --- */}
        <Box className="main-container">
          <Input
            className="main-input"
            titleLabel="Customer Name"
            value={customer?.name || ''}
            placeholder="Customer Name"
            disabled={mode === 'view' || mode === 'edit'}
            onChange={(e: any) => handleChange('name', e.target.value)}
          />

          <Input
            className="main-input"
            titleLabel="CIS Number"
            placeholder="CIS Number"
            value={customer?.cisNumbers?.[0] || ''}
            disabled={mode === 'view' || mode === 'edit'}
            onChange={(e: any) => handleCisNumberChange(e.target.value)}
          />
        </Box>

        {/* --- Customer Type and Billing Profile --- */}
        <Box className="main-container">
          <Select
            className="main-input"
            value={customer?.customerType || ''}
            options={[
              { key: 'COMMERCIAL', value: 'COMMERCIAL', text: 'COMMERCIAL' },
              { key: 'INTERNAL', value: 'INTERNAL', text: 'INTERNAL' },
            ]}
            title="Customer Type"
            disabled={disabled}
            onChange={(val: any) => handleChange('customerType', val)}
          />

          <Input
            className="main-input"
            titleLabel="Billing Profile Id"
            placeholder="Billing profile Id"
            value={customer?.billingCustomerId || ''}
            disabled={disabled}
            onChange={(e: any) => handleChange('billingCustomerId', e.target.value)}
          />
        </Box>

        {/* --- Demo Customer --- */}
        <Box className="main-checkgroup">
          <Typography variant="body1">Demo Customer :</Typography>
          <RadioGroup
            name="DemoCustomer"
            selectedValue={customer?.demoCustomer}
            options={[
              { label: 'Yes', value: true },
              { label: 'No', value: false },
            ]}
            disabled={disabled}
            onChange={handleDemoChange}
          />
        </Box>
      </Box>
    </Box>
  );
};

export default CustomerInfo;
