import React, {

  useState,
  useMemo,
  
} from 'react';
import "../../pages/customer-profile/customer-profile.css"
import {
  Table,
  TableBody,
  TableCell,
  TableContainer,
  TableHead,
  TableRow,
  Input,
  Button,
  ShowIcon,
  EditIcon,
  DeleteIcon,
  Box
} from '@ucl/ui-components';

const AccountSearchTable = () => {
  const [accountName, setAccountName] = useState('');
  const [accountNumber, setAccountNumber] = useState('');
  const [filters, setFilters] = useState({
    accountName: '',
    accountNumber: '',
  });
  const setAppliedFilter = (obj: any) => {
    setFilters(obj);
  };
  const tableHeaders: any = ['Account Name ', 'Account  Number','Routing Number','Action'];
  const data: any = [
    {
      account_name: 'Checking Account one',
      account_number: '12231',
      routing_number: '1244',
    },
    {
      account_name: 'Checking Account two',
      account_number: '12232',
      routing_number: '1245',
    },
    {
      account_name: 'Checking Account three',
      account_number: '12233',
      routing_number: '1246',
    },
    {
      account_name: 'Checking Account four',
      account_number: '12234',
      routing_number: '1247',
    },
  ];
  const FilteredData = useMemo(() => {
    console.log('filters are', filters);
    return data.filter((row: any) => {
      return (
        (!filters.accountName ||
          row.account_name
            .toLowerCase()
            .includes(filters.accountName.toLowerCase())) &&
        (!filters.accountNumber ||
          row.account_number
            .toLowerCase()
            .includes(filters.accountNumber.toLowerCase()))
      );
    });
  }, [filters]);

  return (
    <Box sx={{marginTop:"30px"}}>
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
    <TableContainer >
        <Table>
          <TableHead>
            <TableRow>
              {tableHeaders.map((head: any, key: any) => (
                <TableCell key={key}>{head}</TableCell>
              ))}
            </TableRow>
          </TableHead>
          <TableBody>
            {FilteredData.map((data: any) => (
              <TableRow key={data.account_name}>
                <TableCell>{data.account_name}</TableCell>
                <TableCell>{data.account_number}</TableCell>

                <TableCell>{data.routing_number}</TableCell>
                <TableCell style={{ display: 'flex', gap: '5px' }}>
                  <ShowIcon />
                  <EditIcon />
                  <DeleteIcon />
                </TableCell>
              </TableRow>
            ))}
          </TableBody>
        </Table>
      </TableContainer>
    </Box>
  );
};
export default AccountSearchTable;
