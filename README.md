import React, { useState } from 'react';
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
  const [demoCustomer, setDemoCustomer] = useState<string>('Yes');
  return (
    <Box className="section">
      <Typography variant="h3" className="main-header" fontStyle="italic">
        Customer Info
      </Typography>
      <Box className="sub-section">
        <Typography variant="subtitle1" fontStyle="italic" className="sub-head">
          {`Gateway Customer Id : ${gatewayCustomerId}`}
        </Typography>
        <Box className="main-container">
          <Input
            className="main-input"
            titleLabel="Customer Name"
            value={customer?.name}
            placeholder="Customer Name"
            disabled={mode ==="view" || mode==="edit"}
          />
          <Input
            className="main-input"
            titleLabel="CIS Number"
            placeholder="CIS Number"
            value={customer?.cisNumbers[0]}
            disabled={mode ==="view" || mode==="edit"}
          />
        </Box>
        <Box className="main-container">
          <Select
            className="main-input"
            value={customer?.customerType}
            options={[
              { key: 'COMMERCIAL', value: 'COMMERCIAL', text: 'COMMERCIAL' },
              { key: 'INTERNAL', value: 'INTERNAL', text: 'INTERNAL' },
            ]}
            title="Customer Type"
            disabled={disabled}
          />
          <Input
            className="main-input"
            titleLabel="Billing Profile Id"
            placeholder="Billing profile Id"
            value={customer?.billingCustomerId}
            disabled={disabled}
          />
        </Box>
        <Box className="main-checkgroup">
          <Typography variant="body1">Demo Customer :</Typography>
          <RadioGroup
            name="DemoCustomer"
            selectedValue={customer?.demoCustomer}
            options={[
              {
                label: 'Yes',
                value: true,
              },
              { label: 'No', value: false },
            ]}
            disabled={disabled}
            onChange={setDemoCustomer}
          />
        </Box>
      </Box>
    </Box>
  );
};
export default CustomerInfo;
