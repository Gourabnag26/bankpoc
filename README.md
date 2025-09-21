import React from 'react';
import { useState } from 'react';
import {
  Box,
  Input,
  Typography,
  Radio,
  Checkbox,
  Select,
  Button
} from '@ucl/ui-components';
import CardCheckbox from '../checkbox-menu/checkbox-menu';


const AccountPopup = () => {
     const api_general_options = [{ id: 'acc_balance', label: 'Account Balance' ,value :'Account Balance' }];
     const api_rtp_options = [
       { id: 'ipa_create_credit', label: 'Create Credit' ,value : 'Instant Payments - Create Credit' },
       { id: 'ipa_recieve_credit', label: 'Retrieve Status' ,value : 'Instant Payments - Receive Credit'},
     ];
     const api_fednow_options = [
       { id: 'fednow_createcredit', label: 'Create Credit',value : 'FedNow - Create Credit' },
       { id: 'fednow_recievecredit', label: 'Retrieve Status',value : 'FedNow - Receive Credit' },
     ];
     const api_wire_options = [
       { id: 'wire_create_credit', label: 'Create Credit',value : 'Wire - Create Credit' },
       { id: 'wire_retieve_credit', label: 'Retrieve Status',value : 'Wire - Retrieve Credit'},
       { id: 'wire_webhook', label: 'Webhook Status Update' ,value : 'Wire - Webhook Retrieve'},
     ];
     
     
       const [apiGeneralValues, setApiGeneralValues] = useState<string[]>([]);
       const [apiRTPValues, setApiRTPValues] = useState<string[]>([]);
       const [apiFedNowValues, setApiFedNowValues] = useState<string[]>([]);
       const [apiWiresValues, setApiWiresValues] = useState<string[]>([]);
       const [isAllSelected,setisAllSelected] =useState<boolean>(false)
  return (
    <>
      <Box className="section">
        <Typography
          variant="h3"
          className="main-header"
          fontStyle="italic"
          sx={{
            marginBottom: '10px',
            fontSize: '21px',
            marginButtom: '10px',
            fontStyle: 'italic',
          }}
        >
          Customer Info
        </Typography>
        <Box sx={{ padding: '10px' }}>
          <Box
            className="main-container"
            sx={{ display: 'flex', alignItems: 'center', gap: '50px' }}
          >
            <Input
              className="main-input"
              titleLabel="Customer Name"
              placeholder="Customer Name"
              sx={{ width: '250px', height: '40px', marginTop: '10px' }}
            />
            <Checkbox  sx={{width:"250px"}} label="Billing Account" />
          </Box>

          <Box
            className="main-container"
            sx={{
              display: 'flex',
              alignItems: 'center',
              justifyContent: 'space-between',
            }}
          >
            <Input
              className="main-input"
              titleLabel="Account Number"
              placeholder="Account Number"
              sx={{ width: '250px', height: '40px', marginTop: '10px' }}
            />

            <Select
              className="main-input"
              value="10002"
              title="Bank Code"
              sx={{ width: '250px', height: '40px', marginTop: '15px' }}
              options={[{ key: '10002', value: '10002', text: '10002' }]}
            />
          </Box>
          <Box>
            <Input
              className="main-input"
              titleLabel="Router Number"
              placeholder="Router Number"
              sx={{ width: '250px', height: '40px', marginTop: '10px' }}
            />
            <Input
              className="main-input"
              titleLabel="Authorized Transaction Account Routing Number"
              placeholder="Authorized Transaction Account Routing Number"
              sx={{ width: '250px', height: '40px', marginTop: '10px' }}
            />
          </Box>
        </Box>
      </Box>
      <Box className="section">
        <Typography
          variant="h3"
          className="main-header"
          fontStyle="italic"
          sx={{
            marginBottom: '10px',
            fontSize: '21px',
            marginButtom: '10px',
            fontStyle: 'italic',
          }}
        >
          Limits
        </Typography>
        <Box sx={{ padding: '10px' }}>
          <Box
            className="main-container"
            sx={{
              display: 'flex',
              alignItems: 'center',
              justifyContent: 'space-between',
            }}
          >
            <Input
              className="main-input"
              titleLabel="Transaction Limit"
              placeholder="Transaction Limit"
              sx={{ width: '250px', height: '40px', marginTop: '10px' }}
            />

            <Input
              className="main-input"
              titleLabel="Daily Limit"
              placeholder="Daily Limit"
              sx={{ width: '250px', height: '40px', marginTop: '10px' }}
            />
          </Box>
        </Box>
      </Box>
      <Box className="section">
        <Box
          className="group-head"
          sx={{
            display: 'flex',
            flexDirection: 'row',
            gap: '30px',
            alignItems: 'center',
          }}
        >
          <Typography variant="h3" className="main-header" fontStyle="italic"
           sx={{        
            fontSize: '21px',
            fontStyle: 'italic',
          }}
          >
            Api
          </Typography>
          <Checkbox label="Select all" />
        </Box>
        <Box className="checkbox-container sub-section" sx={{ 
          display:"flex",
          flexDirection:"row",
          gap:"10px",
          padding:"10px",
          alignItems:"center",
          flexWrap:"wrap"}}>
          <CardCheckbox
            title="General"
            checkboxes={api_general_options}
            selectedValues={apiGeneralValues}
            onChange={setApiGeneralValues}
          />
          <CardCheckbox
            title="US RTP"
            checkboxes={api_rtp_options}
            selectedValues={apiRTPValues}
            onChange={setApiRTPValues}
          />
          <CardCheckbox
            title="FedNow"
            checkboxes={api_fednow_options}
            selectedValues={apiFedNowValues}
            onChange={setApiFedNowValues}
          />
          <CardCheckbox
            title="Wire"
            checkboxes={api_wire_options}
            selectedValues={apiWiresValues}
            onChange={setApiWiresValues}
          />
        </Box>
      </Box>
      <Box sx={{display:"flex",justifyContent:"center",gap:"10px",alignItems:"center",marginTop:"10px"}}>
        <Button sx={{      
          border:"1px solid black",
          background:"#F5F5F5",
          padding:"10px",
          fontSize:"12px",
          color:"black",
          width:"150px",
          borderRadius:"5px"}}
          children="save"/>
        <Button
        sx={{      
          border:"1px solid black",
          background:"#F5F5F5",
          padding:"10px",
          fontSize:"12px",
          color:"black",
          width:"150px",
          borderRadius:"5px"}}
        children="cancel"/>
      </Box>
    </>
  );
};

export default AccountPopup;
