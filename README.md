import React from 'react';
import { IcustomerProps } from '../../customer-profile';
import {
  Box,
  Input,
  Radio,
  RadioGroup,
  Typography,
} from '@ucl/ui-components';
import '../../customer-profile.css';
const BusinessContact = ({ customer, setCustomer }: IcustomerProps) => {
  return (
    <Box className="section">
      <Typography variant="h3" className="main-header" fontStyle="italic">
        Business Contact
      </Typography>
      <Box className="sub-section">
        <Box className="main-container">
          <Input className="main-input" titleLabel="Name" placeholder="Name" />
          <Input
            className="main-input"
            titleLabel="Phone"
            placeholder="Phone Number"
          />
        </Box>
        <Box className="main-container">
          <Input
            className="main-input"
            titleLabel="Email"
            placeholder="Email"
          />
        </Box>
        <Box className="main-checkgroup">
          <Typography variant="body1">Preferred Method :</Typography>
          <RadioGroup
            className="check-group"
            defaultValue="Yes"
            errorText=""
            helperText=""
          >
            <Radio label="Email" value="Email" />
            <Radio label="Phone" value="No" />
          </RadioGroup>
        </Box>
      </Box>
    </Box>
  );
};
export default BusinessContact;
