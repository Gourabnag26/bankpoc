import React, { useState } from 'react';
import { IcustomerProps } from '../../customer-profile';
import { Box, Button, Dialog, Typography } from '@ucl/ui-components';
import AccountSearchTable from '../../../../components/account-table/account-table';
import AccountPopup from '../../../../components/account-popup/account-popup';
import '../../customer-profile.css';

const AccountPreference = ({ customer, setCustomer, disabled }: IcustomerProps) => {
  const [accountPopup, setAccountPopup] = useState(false);
  const [selectedAccount, setSelectedAccount] = useState<any>(null);
  const [selectedIndex, setSelectedIndex] = useState<number | null>(null);
  const [isView, setIsView] = useState(false);

  // Add a new account
  const handleAddAccount = (newAccount: any) => {
    setCustomer((prev) => ({
      ...prev,
      customerAccounts: [...(prev.customerAccounts || []), newAccount],
    }));
    setAccountPopup(false);
  };

  // Update an existing account
  const handleUpdateAccount = (index: number, updatedAccount: any) => {
    setCustomer((prev) => {
      const accounts = [...(prev.customerAccounts || [])];
      accounts[index] = { ...accounts[index], ...updatedAccount };
      return { ...prev, customerAccounts: accounts };
    });
    setAccountPopup(false);
  };

  // Delete an account
  const handleDeleteAccount = (index: number) => {
    setCustomer((prev) => {
      const accounts = [...(prev.customerAccounts || [])];
      accounts.splice(index, 1);
      return { ...prev, customerAccounts: accounts };
    });
  };

  // Open the popup for Add or Edit/View
  const openPopup = (account: any = null, index: number | null = null, view = false) => {
    setSelectedAccount(account);
    setSelectedIndex(index);
    setIsView(view);
    setAccountPopup(true);
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
            onClick={() => openPopup()}
          />
        )}

        {/* Account Popup */}
        <Dialog
          dialogContent={
            <AccountPopup
              account={selectedAccount}
              disabled={isView || disabled}
              customer={customer}
              onSave={(data) => {
                if (selectedIndex !== null) {
                  handleUpdateAccount(selectedIndex, data);
                } else {
                  handleAddAccount(data);
                }
              }}
              onCancel={() => setAccountPopup(false)}
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
        onUpdateAccount={(index: number, data: any) => handleUpdateAccount(index, data)}
        onDeleteAccount={(index: number) => handleDeleteAccount(index)}
        onEditAccount={(account: any, index: number) => openPopup(account, index, false)}
        onViewAccount={(account: any, index: number) => openPopup(account, index, true)}
      />
    </Box>
  );
};

export default AccountPreference;
