import React, { useState } from 'react';
import { IcustomerProps } from '../../customer-profile';
import { Box, Input, Typography, Button } from '@ucl/ui-components';

import '../../customer-profile.css';
const ActionButton = ({ customer, setCustomer }: IcustomerProps) => {
  return (
    <Box className="main-container">
      <Input titleLabel="TMCC Case" sx={{ width: '160px' }} />

      <Button
        sx={{ width: '170px' }}
        className="button"
        variant="primary"
        children="Save Draft"
      />
      <Button
        sx={{ width: '170px' }}
        className="button"
        variant="primary"
        children="Save and Exit"
      />
      <Button
        sx={{ width: '170px' }}
        className="button"
        variant="primary"
        children="Request for Approval"
      />
      <Button
        sx={{ width: '170px' }}
        className="button"
        variant="primary"
        children="Delete Draft"
      />
    </Box>
  );
};
export default ActionButton;
