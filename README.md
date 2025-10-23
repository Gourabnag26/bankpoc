import React, { useState } from 'react';
import { IcustomerProps } from '../../customer-profile';
import {
  Box,
  Input,
  Typography,
} from '@ucl/ui-components';
import RadioGroup from '../../../../components/radio-group/radio-group';
import '../../customer-profile.css';
const BusinessContact = ({ customer, setCustomer,disabled }: IcustomerProps) => {
  const [selectMethod,setSelectedMethod]=useState<string>("Email")
  return (
    <Box className="section">
      <Typography variant="h3" className="main-header" fontStyle="italic">
        Business Contact
      </Typography>
      <Box className="sub-section">
        <Box className="main-container">
          <Input className="main-input" titleLabel="Name" placeholder="Name"  value={customer?.customerContacts[0]?.contactName} disabled={disabled}/>
          <Input
            className="main-input"
            titleLabel="Phone"
            placeholder="Phone Number"
            value={customer?.customerContacts[0]?.contactPhone}
            disabled={disabled}
          />
        </Box>
        <Box className="main-container">
          <Input
            className="main-input"
            titleLabel="Email"
            placeholder="Email"
            value={customer?.customerContacts[0]?.contactEmail}
            disabled={disabled}
          />
        </Box>
        <Box className="main-checkgroup">
          <Typography variant="body1">Preferred Method :</Typography>
           <RadioGroup
            name="contactMethodBusiness"
            selectedValue={customer?.customerContacts[0]?.contactPreferredType}
            disabled={disabled}
            options={[
              {
                label: 'Email',
                value: 'Email',
              },
              { label: 'Phone', value: 'Phone' },
            ]}
            onChange={setSelectedMethod}
          />
        </Box>
      </Box>
    </Box>
  );
};
export default BusinessContact;
