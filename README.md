import React, { useState } from 'react';
import { IcustomerProps } from '../../customer-profile';
import { Box, Button, Dialog, Typography } from '@ucl/ui-components';
import AccountSearchTable from '../../../../components/account-table/account-table';
import AccountPopup from '../../../../components/account-popup/account-popup';

const AccountPreference = ({
  customer,
  setCustomer,
  disabled,
}: IcustomerProps) => {
  const [accountPopup, setAccountPopup] = useState(false);
  return (
    <Box className="section">
      <Box className="group-head">
        <Typography variant="h3" className="main-header" fontStyle="italic">
          Account Access & Preferences
        </Typography>
        {!disabled && (
          <Button
            className="button"
            variant="primary"
            children="Add Account"
            onClick={() => setAccountPopup(true)}
          />
        )}
        <Dialog
          dialogContent={<AccountPopup />}
          dialogTitle="Account Access & Preferences"
          open={accountPopup}
          onClose={() => setAccountPopup(false)}
          fullWidth
          maxWidth="md"
        />
      </Box>
      <AccountSearchTable
        customer={customer}
        setCustomer={setCustomer}
        disabled={disabled}
      />
    </Box>
  );
};
export default AccountPreference;
