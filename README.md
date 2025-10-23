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
const AccountSearchTable = ({
  customer,
  setCustomer,
  disabled,
}: IcustomerProps) => {
  const [accountName, setAccountName] = useState('');
  const [accountNumber, setAccountNumber] = useState('');
  const [accountPopup, setAccountPopup] = useState(false);
  const [isView, setIsView] = useState(false);
  const [account, setAccount] = useState({});
  const [filters, setFilters] = useState({
    accountName: '',
    accountNumber: '',
  });
  const setAppliedFilter = (obj: any) => {
    setFilters(obj);
  };
  const tableHeaders: any = [
    'Account Name ',
    'Account  Number',
    'Routing Number',
    'Action',
  ];
  const FilteredData = useMemo(() => {
    const data: any = customer.customerAccounts;
    return data.filter((row: any) => {
      return (
        (!filters.accountName ||
          row.name.toLowerCase().includes(filters.accountName.toLowerCase())) &&
        (!filters.accountNumber ||
          row.number
            .toLowerCase()
            .includes(filters.accountNumber.toLowerCase()))
      );
    });
  }, [filters]);

  return (
    <Box sx={{ marginTop: '30px' }}>
      <Box className="main-container">
        <Input
          titleLabel="Account Name"
          value={accountName}
          className="main-input"
          onChange={(e) => {
            setAccountName(e.target.value);
          }}
        />
        <Input
          titleLabel="Account Number"
          value={accountNumber}
          className="main-input"
          onChange={(e) => {
            setAccountNumber(e.target.value);
          }}
        />
        <Button
          variant="primary"
          className="button"
          onClick={() => {
            setAppliedFilter({
              accountName: accountName,
              accountNumber: accountNumber,
            });
          }}
          children="Search"
        />
      </Box>
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
            {FilteredData?.map((data: any) => (
              <TableRow key={data.account_name}>
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
                    }}
                    children={
                      <span>
                        <ShowIcon />
                      </span>
                    }
                  />
                  {!disabled && (
                    <>
                      <Tooltip
                        title="Edit"
                        onClick={() => {
                          setAccountPopup(true);
                          setAccount(data);
                          setIsView(false);
                        }}
                        children={
                          <span>
                            <EditIcon />
                          </span>
                        }
                      />
                      <Tooltip
                        title="Delete"
                        children={
                          <span>
                            <DeleteIcon />
                          </span>
                        }
                      />
                    </>
                  )}
                </TableCell>
              </TableRow>
            ))}
          </TableBody>
        </Table>
      </TableContainer>
      <Dialog
        dialogContent={
          <AccountPopup
            disabled={isView}
            account={account}
            customer={customer}
            setCustomer={setCustomer}
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
