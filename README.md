import React, { useMemo, useState } from 'react';
import '../../pages/customer-profile/customer-profile.css';
import {
  Box,
  Button,
  DeleteIcon,
  Dialog,
  EditIcon,
  Input,
  ShowIcon,
  Table,
  TableBody,
  TableCell,
  TableContainer,
  TableHead,
  TableRow,
  Tooltip,
} from '@ucl/ui-components';
import { IcustomerProps } from '../../pages/customer-profile/customer-profile';
import AccountPopup from '../account-popup/account-popup';

interface AccountSearchTableProps extends IcustomerProps {
  onUpdateAccount?: (index: number, updated: any) => void;
  onDeleteAccount?: (index: number) => void;
}

const AccountSearchTable = ({
  customer,
  setCustomer,
  disabled,
  onUpdateAccount,
  onDeleteAccount,
}: AccountSearchTableProps) => {
  const [accountName, setAccountName] = useState('');
  const [accountNumber, setAccountNumber] = useState('');
  const [accountPopup, setAccountPopup] = useState(false);
  const [isView, setIsView] = useState(false);
  const [account, setAccount] = useState<any>({});
  const [selectedIndex, setSelectedIndex] = useState<number>(-1);
  const [filters, setFilters] = useState({
    accountName: '',
    accountNumber: '',
  });

  const setAppliedFilter = (obj: any) => setFilters(obj);

  const tableHeaders: any = ['Account Name', 'Account Number', 'Routing Number', 'Action'];

  const FilteredData = useMemo(() => {
    const data: any = customer.customerAccounts || [];
    return data.filter((row: any) => {
      return (
        (!filters.accountName ||
          row.name.toLowerCase().includes(filters.accountName.toLowerCase())) &&
        (!filters.accountNumber ||
          row.number.toLowerCase().includes(filters.accountNumber.toLowerCase()))
      );
    });
  }, [filters, customer.customerAccounts]);

  const handleSave = (updatedAccount: any) => {
    if (selectedIndex >= 0 && onUpdateAccount) {
      onUpdateAccount(selectedIndex, updatedAccount);
    } else if (setCustomer) {
      // Add new account if needed
      setCustomer((prev: any) => ({
        ...prev,
        customerAccounts: [...(prev.customerAccounts || []), updatedAccount],
      }));
    }
    setAccountPopup(false);
  };

  const handleDelete = (index: number) => {
    if (onDeleteAccount) {
      onDeleteAccount(index);
    }
  };

  return (
    <Box sx={{ marginTop: '30px' }}>
      {/* Filters */}
      <Box className="main-container">
        <Input
          titleLabel="Account Name"
          value={accountName}
          className="main-input"
          onChange={(e) => setAccountName(e.target.value)}
        />
        <Input
          titleLabel="Account Number"
          value={accountNumber}
          className="main-input"
          onChange={(e) => setAccountNumber(e.target.value)}
        />
        <Button
          variant="primary"
          className="button"
          onClick={() =>
            setAppliedFilter({ accountName: accountName, accountNumber: accountNumber })
          }
        >
          Search
        </Button>
      </Box>

      {/* Table */}
      <TableContainer>
        <Table>
          <TableHead>
            <TableRow>
              {tableHeaders.map((head: any, key: any) => (
                <TableCell key={key}>{head}</TableCell>
              ))}
            </TableRow>
          </TableHead>
          <TableBody>
            {FilteredData?.map((data: any, idx: number) => (
              <TableRow key={data.account_name || idx}>
                <TableCell>{data.name}</TableCell>
                <TableCell>{data.number}</TableCell>
                <TableCell>{data.routingNumber}</TableCell>
                <TableCell style={{ display: 'flex', gap: '5px' }}>
                  <Tooltip
                    title="View"
                    onClick={() => {
                      setAccountPopup(true);
                      setAccount(data);
                      setIsView(true);
                      setSelectedIndex(idx);
                    }}
                  >
                    <span>
                      <ShowIcon />
                    </span>
                  </Tooltip>

                  {!disabled && (
                    <>
                      <Tooltip
                        title="Edit"
                        onClick={() => {
                          setAccountPopup(true);
                          setAccount(data);
                          setIsView(false);
                          setSelectedIndex(idx);
                        }}
                      >
                        <span>
                          <EditIcon />
                        </span>
                      </Tooltip>

                      <Tooltip
                        title="Delete"
                        onClick={() => handleDelete(idx)}
                      >
                        <span>
                          <DeleteIcon />
                        </span>
                      </Tooltip>
                    </>
                  )}
                </TableCell>
              </TableRow>
            ))}
          </TableBody>
        </Table>
      </TableContainer>

      {/* Account Popup */}
      <Dialog
        dialogContent={
          <AccountPopup
            disabled={isView || disabled}
            account={account}
            customer={customer}
            onSave={handleSave}
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
  );
};

export default AccountSearchTable;
