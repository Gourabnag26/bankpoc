import React, { useEffect, useState } from 'react';
import { Box } from '@ucl/ui-components';
import { useLocation, useNavigate } from 'react-router-dom';
import CustomerInfo from './components/customer-info/customer-info';
import BusinessContact from './components/business-contact/business-contact';
import TechnicalContact from './components/technical-contact/technical-contact';
import TransactionLimit from './components/transaction-limit/transaction-limit';
import ApiCustomerConfig from './components/api-customer/api-customer';
import AccountPreference from './components/account-preference/account-preference';
import ActionButton from './components/action-button/action-button';
import { useCustomer } from '../../context/customer-context';
import ApproverView from './approver-view/approver-view';
import { CustomerService } from 'shared';
const { v4: uuidv4 } = require('uuid');

// Interfaces omitted for brevity but should remain the same.

export const CustomerProfile = () => {
  const { myTasks, customers } = useCustomer();
  const location = useLocation();
  const navigate = useNavigate();

  const searchParams = new URLSearchParams(location.search);
  const mode = searchParams.get('mode');
  const role = searchParams.get('role');
  const isDraft = searchParams.get('draft');
  const gatewayCustomerId = searchParams.get('gatewayCustomerId');

  const isViewMode = !mode || mode === 'view';

  // Use appropriate context data
  const customerData = isDraft ? myTasks : customers;

  // Default initial structure
  const initialCustomerData: ICustomerData = {
    gatewayCustomerId: gatewayCustomerId || '',
    name: '',
    cisNumbers: [],
    enabled: false,
    customerSettings: {
      transactionLimit: 0,
      cumulativeTransactionLimit: 0,
      processingWindow: '00:00-23:59',
      processingWindowTimezone: 'CST',
    },
    customerType: 'COMMERCIAL',
    virtualAcctCustomer: false,
    demoCustomer: false,
    customerAccounts: [],
    customerProducts: [],
    billingCustomerId: '',
    createdBy: '',
    customerContacts: [],
  };

  const [customer, setCustomer] = useState<ICustomerData>(initialCustomerData);

  // Redirect to home if invalid query params
  useEffect(() => {
    if (
      !mode ||
      !role ||
      (role === 'viewer' && mode !== 'view') ||
      (role === 'editor' && mode === 'create')
    ) {
      navigate('/');
    }
  }, [mode, role, navigate]);

  // Fetch customer data from API (for viewer/editor roles)
  useEffect(() => {
    const fetchCustomer = async () => {
      try {
        const customerServiceApi = new CustomerService();
        const traceId = uuidv4();

        customerServiceApi.addGetCustomerHeader(gatewayCustomerId, traceId);

        const response = await customerServiceApi.getCustomer();

        if (!response?.data) {
          navigate('/');
          return;
        }

        setCustomer(response.data); // ✅ Set customer from API
      } catch (error) {
        console.error('Failed to fetch customer data:', error);
        navigate('/');
      }
    };

    if ((role === 'viewer' || role === 'editor') && gatewayCustomerId) {
      fetchCustomer();
    } else if (mode === 'edit' && gatewayCustomerId) {
      const existingCustomer = customerData.find(
        (c: any) => c.gatewayCustomerId === gatewayCustomerId
      );
      if (existingCustomer) {
        setCustomer(existingCustomer);
      } else {
        navigate('/');
      }
    }
  }, [role, gatewayCustomerId, mode, customerData, navigate]);

  // Approver view override
  if (role === 'approver' && mode === 'edit') {
    return <ApproverView />;
  }

  return (
    <Box className="main-profile">
      <CustomerInfo
        customer={customer}
        setCustomer={setCustomer}
        disabled={isViewMode}
        mode={mode}
        gatewayCustomerId={gatewayCustomerId}
      />
      <BusinessContact
        customer={customer}
        setCustomer={setCustomer}
        disabled={isViewMode}
      />
      <TechnicalContact
        customer={customer}
        setCustomer={setCustomer}
        disabled={isViewMode}
      />
      <TransactionLimit
        customer={customer}
        setCustomer={setCustomer}
        disabled={isViewMode}
      />
      <ApiCustomerConfig
        customer={customer}
        setCustomer={setCustomer}
        disabled={isViewMode}
      />
      <AccountPreference
        customer={customer}
        setCustomer={setCustomer}
        disabled={isViewMode}
      />
      {!isViewMode && (
        <ActionButton
          customer={customer}
          setCustomer={setCustomer}
          disabled={isViewMode}
          gatewayCustomerId={gatewayCustomerId}
        />
      )}
    </Box>
  );
};
