import { useState } from 'react';
import { useCustomer } from '../../context/customer-context';
import MyTasksTable from '../../components/my-tasks-table/my-tasks-table';
import MyTasksTableData from '../../../shared/dummy-json/my-tasks.json';
import { useRole } from '../../../shared/utils/role-manager';

export const MyTasks = () => {
  const [tableData] = useState(MyTasksTableData);

  const role = useRole();

  const { myTasks } = useCustomer();

  return (

      <MyTasksTable
        tableHeaders={tableData.tableHeaders}
        tableBody={myTasks}
        currentRole={role}
      />
    
  );
};


{
    "draftList": [
        {
            "taskId": 2,
            "gatewayCustomerId": "ff8555cc-e370-4aba-ae92-dc513843199q",
            "status": "PENDING_APPROVAL",
            "customerName": "Test Customer1",
            "customerConfig": "{\"name\": \"Test Customer1\", \"enabled\": true, \"createdBy\": \"LIQUIBASE\", \"cisNumbers\": [\"6666999999\"], \"customerType\": \"COMMERCIAL\", \"demoCustomer\": false, \"customerAccounts\": [{\"name\": \"Test Customer1Account1\", \"number\": \"1001223559\", \"enabled\": true, \"bankCode\": \"88833\", \"cisNumbers\": [\"6666999999\"], \"routingNumber\": \"771666333\", \"billingAccount\": true, \"accountSettings\": {\"transactionLimit\": 2000, \"cumulativeTransactionLimit\": 10000}}, {\"name\": \"Test Customer1Account2\", \"number\": \"1002587325\", \"enabled\": true, \"bankCode\": \"66655\", \"cisNumbers\": [\"6666999999\"], \"routingNumber\": \"771666222\", \"billingAccount\": false, \"accountSettings\": {\"transactionLimit\": 5000, \"cumulativeTransactionLimit\": 20000}}], \"customerProducts\": [{\"name\": \"ACCOUNT_BALANCE_API\", \"enabled\": true, \"billable\": true, \"resources\": [{\"name\": \"RETRIEVE_ACCOUNT_BALANCE\", \"enabled\": true, \"billable\": true, \"description\": \"Service to retrieve account balance details\", \"friendlyName\": \"Retrieve Account Balance\"}], \"shortName\": \"ABA\", \"description\": \"Provides an api resource to perform account balance inquiry\", \"friendlyName\": \"Account Balance API\", \"productSettings\": {}}, {\"name\": \"INSTANT_PAYMENTS_API\", \"enabled\": true, \"billable\": true, \"resources\": [{\"name\": \"CREATE_CREDIT_TRANSFER\", \"enabled\": true, \"billable\": true, \"description\": \"Help to initiate credit transfer request\", \"friendlyName\": \"Create Credit Transfer\"}, {\"name\": \"RETRIEVE_CREDIT_TRANSFER\", \"enabled\": true, \"billable\": true, \"description\": \"Help to retrieve the status of the credit transfer request\", \"friendlyName\": \"Retrieve Credit Transfer\"}, {\"name\": \"RECEIVE_CREDIT_TRANSFER\", \"enabled\": true, \"billable\": true, \"description\": \"Help to receive the credit transfer request\", \"friendlyName\": \"Receive Credit Transfer\"}], \"shortName\": \"IPA\", \"description\": \"Provides various resources to manage instant payments api\", \"friendlyName\": \"Instant Payments API\", \"paymentRails\": [{\"name\": \"RTP\", \"enabled\": true, \"billable\": true, \"description\": \"Real Time Payments\", \"friendlyName\": \"Real Time Payments\", \"paymentRailSettings\": {\"networkLimit\": 1000000, \"duplicateCheckFields\": [\"CUSTOMER_REF_ID\", \"DEBTOR_ACCT_NUM\", \"AMOUNT\", \"CURRENCY\", \"CREDITOR_ACCT_NUM\", \"CREDITOR_ACCT_FI_ID\", \"CREDITOR_ADDRESS\"], \"duplicateCheckDuration\": 30}}], \"productSettings\": {\"processingWindow\": \"08:00-20:00\", \"processingWindowTimezone\": \"EST\"}}], \"customerSettings\": {\"processingWindow\": \"08:00-19:00\", \"processingWindowTimezone\": \"EST\"}, \"billingCustomerId\": \"IPABILLIN7\", \"gatewayCustomerId\": \"ff8555cc-e370-4aba-ae92-dc513843199q\", \"virtualAcctCustomer\": true}",
            "createdBy": "User1",
            "updatedBy": "User1"
        },
        {
            "taskId": 1,
            "gatewayCustomerId": "8b7af1ae-53f2-4e34-85cd-a86c896970df",
            "status": "DRAFT",
            "customerName": "Customer198",
            "customerConfig": "{\"name\": \"Customer198\", \"enabled\": true, \"createdBy\": \"LIQUIBASE\", \"cisNumbers\": [\"5132445678\"], \"customerType\": \"COMMERCIAL\", \"demoCustomer\": false, \"customerAccounts\": [{\"name\": \"197Account3\", \"number\": \"7005984461\", \"enabled\": true, \"bankCode\": \"77231\", \"cisNumbers\": [\"5132445678\"], \"routingNumber\": \"772666322\", \"billingAccount\": true, \"accountSettings\": {\"transactionLimit\": 100, \"cumulativeTransactionLimit\": 30000}}, {\"name\": \"197Account4\", \"number\": \"7006219180\", \"enabled\": true, \"bankCode\": \"45144\", \"cisNumbers\": [\"5132445678\"], \"routingNumber\": \"772666422\", \"billingAccount\": false, \"accountSettings\": {\"transactionLimit\": 1000, \"cumulativeTransactionLimit\": 5000}}, {\"name\": \"197Account5\", \"number\": \"7009421783\", \"enabled\": true, \"bankCode\": \"11116\", \"cisNumbers\": [\"5132445678\"], \"routingNumber\": \"772665422\", \"billingAccount\": false, \"accountSettings\": {}}], \"customerProducts\": [{\"name\": \"ACCOUNT_BALANCE_API\", \"enabled\": true, \"billable\": true, \"resources\": [{\"name\": \"RETRIEVE_ACCOUNT_BALANCE\", \"enabled\": true, \"billable\": true, \"description\": \"Service to retrieve account balance details\", \"friendlyName\": \"Retrieve Account Balance\"}], \"shortName\": \"ABA\", \"description\": \"Provides an api resource to perform account balance inquiry\", \"friendlyName\": \"Account Balance API\", \"productSettings\": {}}, {\"name\": \"INSTANT_PAYMENTS_API\", \"enabled\": true, \"billable\": true, \"resources\": [{\"name\": \"CREATE_CREDIT_TRANSFER\", \"enabled\": true, \"billable\": true, \"description\": \"Help to initiate credit transfer request\", \"friendlyName\": \"Create Credit Transfer\"}, {\"name\": \"RETRIEVE_CREDIT_TRANSFER\", \"enabled\": true, \"billable\": true, \"description\": \"Help to retrieve the status of the credit transfer request\", \"friendlyName\": \"Retrieve Credit Transfer\"}, {\"name\": \"RECEIVE_CREDIT_TRANSFER\", \"enabled\": true, \"billable\": true, \"description\": \"Help to receive the credit transfer request\", \"friendlyName\": \"Receive Credit Transfer\"}], \"shortName\": \"IPA\", \"description\": \"Provides various resources to manage instant payments api\", \"friendlyName\": \"Instant Payments API\", \"paymentRails\": [{\"name\": \"RTP\", \"enabled\": true, \"billable\": true, \"description\": \"Real Time Payments\", \"friendlyName\": \"Real Time Payments\", \"paymentRailSettings\": {\"networkLimit\": 5000000, \"duplicateCheckFields\": [\"CUSTOMER_REF_ID\", \"DEBTOR_ACCT_NUM\", \"AMOUNT\", \"CURRENCY\", \"CREDITOR_ACCT_NUM\", \"CREDITOR_ACCT_FI_ID\", \"CREDITOR_ADDRESS\"], \"duplicateCheckDuration\": 30, \"allowedCreditAccountList\": [{\"accountNumber\": \"{\\\"accountNumber\\\": \\\"8989776655\\\", \\\"routingNumber\\\": \\\"777775555588\\\"}\"}]}}], \"productSettings\": {\"processingWindow\": \"01:00-23:00\", \"processingWindowTimezone\": \"EST\"}}], \"customerSettings\": {\"processingWindow\": \"11:00-15:00\", \"transactionLimit\": 500, \"processingWindowTimezone\": \"EST\", \"cumulativeTransactionLimit\": 5000}, \"billingCustomerId\": \"IPABILLIN2\", \"gatewayCustomerId\": \"8b7af1ae-53f2-4e34-85cd-a86c896970df\", \"virtualAcctCustomer\": true}",
            "createdBy": "User1",
            "updatedBy": "User1",
            "approvedBy": "<null>"
        }
    ],
    "completedList": [
        {
            "taskId": 3,
            "gatewayCustomerId": "c59e7d0b-b54d-429c-a000-ad260a370e7c",
            "status": "APPROVED",
            "customerName": "Customer503",
            "customerConfig": "{\"name\": \"Customer503\", \"enabled\": true, \"createdBy\": \"LIQUIBASE\", \"cisNumbers\": [\"1866535234\"], \"customerType\": \"COMMERCIAL\", \"demoCustomer\": false, \"customerAccounts\": [{\"name\": \"Customer503\", \"number\": \"1890623604\", \"enabled\": true, \"bankCode\": \"10002\", \"cisNumbers\": [\"1866535234\"], \"routingNumber\": \"111000753\", \"billingAccount\": true, \"accountSettings\": {\"transactionLimit\": 11000, \"cumulativeTransactionLimit\": 15000}}], \"customerProducts\": [{\"name\": \"ACCOUNT_BALANCE_API\", \"enabled\": false, \"billable\": true, \"resources\": [{\"name\": \"RETRIEVE_ACCOUNT_BALANCE\", \"enabled\": true, \"billable\": true, \"description\": \"Service to retrieve account balance details\", \"friendlyName\": \"Retrieve Account Balance\"}], \"shortName\": \"ABA\", \"description\": \"Provides an api resource to perform account balance inquiry\", \"friendlyName\": \"Account Balance API\", \"productSettings\": {}}, {\"name\": \"INSTANT_PAYMENTS_API\", \"enabled\": false, \"billable\": true, \"resources\": [{\"name\": \"CREATE_CREDIT_TRANSFER\", \"enabled\": true, \"billable\": true, \"description\": \"Help to initiate credit transfer request\", \"friendlyName\": \"Create Credit Transfer\"}, {\"name\": \"RETRIEVE_CREDIT_TRANSFER\", \"enabled\": true, \"billable\": true, \"description\": \"Help to retrieve the status of the credit transfer request\", \"friendlyName\": \"Retrieve Credit Transfer\"}, {\"name\": \"RECEIVE_CREDIT_TRANSFER\", \"enabled\": true, \"billable\": true, \"description\": \"Help to receive the credit transfer request\", \"friendlyName\": \"Receive Credit Transfer\"}], \"shortName\": \"IPA\", \"description\": \"Provides various resources to manage instant payments api\", \"friendlyName\": \"Instant Payments API\", \"paymentRails\": [{\"name\": \"RTP\", \"enabled\": true, \"billable\": true, \"description\": \"Real Time Payments\", \"friendlyName\": \"Real Time Payments\", \"paymentRailSettings\": {\"networkLimit\": 20000, \"duplicateCheckFields\": [\"AMOUNT\", \"CURRENCY\"], \"duplicateCheckDuration\": 30000, \"allowedCreditAccountList\": [{\"accountNumber\": \"{\\\"accountNumber\\\": \\\"8989776654\\\", \\\"routingNumber\\\": \\\"777775555588\\\"}\"}]}}], \"productSettings\": {\"processingWindowTimezone\": \"EST\"}}], \"customerSettings\": {\"processingWindow\": \"08:00-09:00\", \"transactionLimit\": 1000, \"processingWindowTimezone\": \"CST\", \"cumulativeTransactionLimit\": 25000}, \"billingCustomerId\": \"IPABILL012\", \"gatewayCustomerId\": \"c59e7d0b-b54d-429c-a000-ad260a370e7c\", \"virtualAcctCustomer\": true}",
            "createdBy": "User1",
            "updatedBy": "User1",
            "approvedBy": "Approver1"
        }
    ],
    "approveList": []
}



import React, { createContext, ReactNode, useContext, useEffect, useState } from 'react';
import { Constants } from 'shared';
import MyTasksTableData from '../../shared/dummy-json/my-tasks.json'; 
// import CustomerTableData from '../../shared/dummy-json/customer-search.json'

interface Task {
  customerName: string;
  gatewayCustomerId: string;
  cisNumber: string;
  tmccCase: string;
  customerType: string;
  status: string;
}

interface Customer {
  name: string,
  gatewayCustomerId: string,
  cisNumbers: Array<string>,
  customerAccounts: Array<object>
}

interface CustomerContextProps {
  myTasks: Task[];
  setMyTasks: React.Dispatch<React.SetStateAction<Task[]>>;
  customers : Customer[];
  setCustomers: React.Dispatch<React.SetStateAction<Customer[]>>;
  role: string;
}

const initialTasks: Task[] = MyTasksTableData.tableBody;
const initialCustomerData : Customer[] = [];
const CustomerContext = createContext<CustomerContextProps>({
  myTasks: initialTasks,
  setMyTasks: () => {
    /* empty */
  },
  customers: initialCustomerData,
  setCustomers: () => {
    /* empty */
  },
  role: Constants.ROLES.CREATOR,
});

export const CustomerProvider = ({ children }: { children: ReactNode }) => {
  const [myTasks, setMyTasks] = useState<Task[]>(initialTasks);
  const [customers, setCustomers] = useState<Customer[]>(initialCustomerData);
  
  useEffect(() => {
    setCustomers(customers)
  }, [customers])

  const role = Constants.ROLES.CREATOR;

  return (
    <CustomerContext.Provider value={{ myTasks, setMyTasks, customers, setCustomers, role }}>
      {children}
    </CustomerContext.Provider>
  );
};

export const useCustomer = () => {
  return useContext(CustomerContext);
};
