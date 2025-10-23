import React, { useState } from 'react';
import { IcustomerProps } from '../../customer-profile';
import { Box, Button, Dialog, Typography } from '@ucl/ui-components';
import AccountSearchTable from '../../../../components/account-table/account-table';
import AccountPopup from '../../../../components/account-popup/account-popup';
import '../../customer-profile.css';

const AccountPreference = ({ customer, setCustomer, disabled }: IcustomerProps) => {
  const [accountPopup, setAccountPopup] = useState(false);

  // 🧠 Add new account
  const handleAddAccount = (newAccount: any) => {
    setCustomer((prev) => ({
      ...prev,
      customerAccounts: [...(prev.customerAccounts || []), newAccount],
    }));
    setAccountPopup(false);
  };

  // 🧠 Update an existing account by index or ID
  const handleUpdateAccount = (index: number, updatedAccount: any) => {
    setCustomer((prev) => {
      const accounts = [...(prev.customerAccounts || [])];
      accounts[index] = { ...accounts[index], ...updatedAccount };
      return { ...prev, customerAccounts: accounts };
    });
  };

  // 🧠 Remove account
  const handleDeleteAccount = (index: number) => {
    setCustomer((prev) => {
      const accounts = [...(prev.customerAccounts || [])];
      accounts.splice(index, 1);
      return { ...prev, customerAccounts: accounts };
    });
  };

  return (
    <Box className="section">
      <Box className="group-head">
        <Typography variant="h3" className="main-header" fontStyle="italic">
          Account Access & Preferences
        </Typography>

        {/* Add Account Button */}
        {!disabled && (
          <Button
            className="button"
            variant="primary"
            children="Add Account"
            onClick={() => setAccountPopup(true)}
          />
        )}

        {/* Account Add/Edit Dialog */}
        <Dialog
          dialogContent={
            <AccountPopup
              onSave={handleAddAccount} // callback to add
              onCancel={() => setAccountPopup(false)}
              disabled={disabled}
            />
          }
          dialogTitle="Account Access & Preferences"
          open={accountPopup}
          onClose={() => setAccountPopup(false)}
          fullWidth
          maxWidth="md"
        />
      </Box>

      {/* Account Table */}
      <AccountSearchTable
        customer={customer}
        setCustomer={setCustomer}
        disabled={disabled}
        onUpdateAccount={handleUpdateAccount}
        onDeleteAccount={handleDeleteAccount}
      />
    </Box>
  );
};

export default AccountPreference;
