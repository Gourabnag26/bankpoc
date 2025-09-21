import {
  Box,
  Container,
  Typography,
  Input,
  Select,
  Radio,
  RadioGroup,
  InputCounter,
  Checkbox,
  Button,
} from '@ucl/ui-components';
import TimeInput from '../../components/time-input/time-input';
import { Dialog } from '@ucl/ui-components';
import CardCheckbox from '../../components/checkbox-menu/checkbox-menu';
import './customer-profile.css';
import { Dispatch, useState } from 'react';
import AccountSearchTable from '../../components/account-table/account-table';
import AccountPopup from '../../components/account-popup/account-popup';
import CustomerDetail from './components/customer-info/customer-info';
import BusinessContact from './components/business-contact/business-contact';
import TechnicalContact from './components/technical-contact/technical-contact';
import TransactionLimit from './components/transaction-limit/transaction-limit';
import ApiCustomerConfig from './components/api-customer/api-customer';

export interface ICustomerData{
    customerName: string;
    cisNumber: number;
    customerType :'Commericial' | 'Internal';
    billingProfileId: string;
    demoCustomer: 'Yes' | 'No';
    businessContactName :string;
    businessContactEmail : string;
    businessContactPhone : string;
    businessContactPreferredMethod: 'Email' | 'Phone';
    technologyContactName :string;
    technologyContactEmail : string;
    technologyContactPhone : string;
    technologyContactPreferredMethod: 'Email' | 'Phone';
    transactionLimit:number;
    dailyLimit :number;
    processingWindowStartTime: string
    processingWindowEndTime : string
    processingWindowZone : 'GMT' |'UTC' | 'CST' | 'IST'


}

export interface IcustomerProps {
  customer :ICustomerData,
  setCustomer : React.Dispatch<React.SetStateAction<ICustomerData>>
}
export const CustomerProfile = () => {

  

  const [accountPopup, setAccountPopup] = useState<boolean>(false);

  const intitalCustomerData : ICustomerData ={
    customerName: "",
    cisNumber: 1,
    customerType :"Commericial",
    billingProfileId: "",
    demoCustomer: "Yes",
    businessContactName : "",
    businessContactEmail : "",
    businessContactPhone : "",
    businessContactPreferredMethod: "Email",
    technologyContactName : "",
    technologyContactEmail : "",
    technologyContactPhone : "",
    technologyContactPreferredMethod: "Email",
    transactionLimit:0,
    dailyLimit :0,
    processingWindowStartTime:"00:00",
    processingWindowEndTime : "24:00",
    processingWindowZone : 'GMT'
  } 
  const [customer,setCustomer]=useState<ICustomerData>(intitalCustomerData)
  return (
    <Box className="main-profile">
      <CustomerDetail customer={customer} setCustomer={setCustomer}/>
      <BusinessContact customer={customer} setCustomer={setCustomer}/>
      <TechnicalContact customer={customer} setCustomer={setCustomer}/>
      <TransactionLimit customer={customer} setCustomer={setCustomer}/>
      <ApiCustomerConfig   customer={customer} setCustomer={setCustomer}/>

      <Box className="section">
        <Box className="group-head">
          <Typography variant="h3" className="main-header" fontStyle="italic">
            Account Access& Preferences
          </Typography>
          <Button
            className="button"
            variant="primary"
            children="Add Account"
            onClick={() => {
              setAccountPopup(true);
            }}
          />
          <Dialog
            dialogContent={<AccountPopup />}
            dialogTitle="TIST"
            open={accountPopup}
            dialogActions={
              <>
                {() => {
                  console.log('hi');
                }}
              </>
            }
            onClose={() => {
              setAccountPopup(false);
            }}
            fullWidth
          />
        </Box>
        <AccountSearchTable />
      </Box>
    </Box>
  );
};
