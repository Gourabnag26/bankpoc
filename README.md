import { Button, Container, Input, Notification, NotificationState, NotificationVariant } from '@ucl/ui-components';
import { Constants, DemoServiceApi } from 'shared';
import { useRole } from '../../../shared/utils/role-manager';
import { CustomerSearchTable } from 'src/components/customer-search-table/customer-search-table';
import data from '../../../shared/dummy-json/customer-search.json';
import { useState, ChangeEvent } from 'react';
import { useNavigate } from 'react-router-dom';
import { useCustomer } from 'src/context/customer-context';
import axios from 'axios'
const { v4: uuidv4 } = require('uuid');


export const CustomerSearch = () => {
  const [filters, setFilters] = useState({
    customerName: '',
    gatewayCustomerId: '',
    cisNumber: '',
    accountNumber: '',
  });
  const {customers} = useCustomer();
  const [tableData, setTableData] = useState(customers);
  const [showNotification, setShowNotification] = useState(false);
  const role = useRole();

  const onChangeHandler = (e: ChangeEvent<HTMLInputElement>) => {
    setFilters({ ...filters, [e.target.name]: e.target.value });
  };

  const navigate = useNavigate();

  const onAddCustomerHandler = () => {
    const gatewayCustomerId = uuidv4();
    navigate(
      `/customer-profile/?role=creator&mode=create&gatewayCustomerId=${gatewayCustomerId}`
    );
  };
  const onSearchHandler= async()=>{
    const demoServiceApi= new DemoServiceApi()
     demoServiceApi?.addHeader()
     const fetchEntitlements = await demoServiceApi?.getReports()
  
  }
  // const onSearchHandler = () => {
  //   const hasFilter =
  //     filters.customerName ||
  //     filters.cisNumber ||
  //     filters.gatewayCustomerId ||
  //     filters.accountNumber;

  //   if (!hasFilter) {
  //     setShowNotification(true);
  //     return;
  //   }

  //   const filtered = data.tableBody.filter((customerInfo) => {
  //     return (
  //       (filters.customerName === '' ||
  //         customerInfo.customerName
  //           .toLowerCase()
  //           .includes(filters.customerName.toLowerCase())) &&
  //       (filters.gatewayCustomerId === '' ||
  //         customerInfo.gatewayCustomerId
  //           .toLowerCase()
  //           .includes(filters.gatewayCustomerId.toLowerCase())) &&
  //       (filters.cisNumber === '' ||
  //         customerInfo.cisNumber
  //           .toLowerCase()
  //           .includes(filters.cisNumber.toLowerCase())) &&
  //       (filters.accountNumber === '' ||
  //         customerInfo.accountNumber
  //           .toLowerCase()
  //           .includes(filters.accountNumber.toLowerCase()))
  //     );
  //   });
  //   setTableData(filtered);
  // };

  const onCancelHandler = () => {
    setFilters({
      customerName: '',
      gatewayCustomerId: '',
      cisNumber: '',
      accountNumber: '',
    });
    setTableData(customers);
  };

  return (
    <Container sx={{ padding: '20px' }}>
      <div
        style={{
          display: 'flex',
          flexWrap: 'wrap',
          flexDirection: 'column',
          alignItems: 'center',
        }}
      >
        {showNotification && (
            <Notification
            open={showNotification}
            notificationState={NotificationState.WARNING}
            notificationVariant={NotificationVariant.BANNER}
            handleCloseIcon={() => setShowNotification(false)}
            autoHideDuration={3000}
            message="Please enter at least one field before searching."
            />
          )}
        <div style={{ display: 'flex', gap: '120px' }}>
          <Input
            name="customerName"
            titleLabel="Customer Name"
            placeholder="Customer Name"
            inputProps={{ value: filters.customerName }}
            onChange={onChangeHandler}
            sx={{ width: '300px' }}
          />
          <Input
            name="cisNumber"
            titleLabel="CIS Number"
            placeholder="CIS Number"
            inputProps={{ value: filters.cisNumber }}
            onChange={onChangeHandler}
            sx={{ width: '300px' }}
          />
        </div>

        <div style={{ display: 'flex', gap: '120px' }}>
          <Input
            name="gatewayCustomerId"
            titleLabel="Gateway Customer ID"
            placeholder="Gateway Customer ID"
            inputProps={{ value: filters.gatewayCustomerId }}
            onChange={onChangeHandler}
            sx={{ width: '300px' }}
          />
          <Input
            name="accountNumber"
            titleLabel="Account Number"
            placeholder="Account Number"
            inputProps={{ value: filters.accountNumber }}
            onChange={onChangeHandler}
            sx={{ width: '300px' }}
          />
        </div>
        <div style={{ display: 'flex', gap: '120px', paddingBottom: '12px' }}>
          {role === Constants.ROLES.CREATOR && (
            <Button
              sx={{ width: '170px' }}
              className="button"
              variant="primary"
              onClick={onAddCustomerHandler}
            >
              Add Customer
            </Button>
          )}
          <Button
            sx={{ width: '170px' }}
            onClick={onSearchHandler}
            className="button"
            variant="primary"
          >
            Search
          </Button>
          <Button
            sx={{ width: '170px' }}
            className="button"
            variant="primary"
            onClick={onCancelHandler}
          >
            Cancel
          </Button>
        </div>
      </div>
      <CustomerSearchTable currentRole={role} data={tableData} />
    </Container>
  );
};
