import React from "react";
import { IcustomerProps } from "../../customer-profile";
import { Box, Input, Radio, RadioGroup, Select, Typography } from "@ucl/ui-components";
import "../../customer-profile.css";
const CustomerDetail=({customer,setCustomer}:IcustomerProps)=>{
   return(
     <Box className="section">
        <Typography variant="h3" className="main-header" fontStyle="italic">
          Customer Info
        </Typography>
        <Box className="sub-section">
          <Box className="main-container">
            <Input
              className="main-input"
              titleLabel="Customer Name"
              placeholder="Customer Name"
            />
            <Input
              className="main-input"
              titleLabel="CIS Number"
              placeholder="CIS Number"
            />
          </Box>
          <Box className="main-container">
            <Select
              className="main-input"
              value="Commercial"
              options={[
                { key: 'Commercial', value: 'Commercial', text: 'Commercial' },
                { key: 'Internal', value: 'Internal', text: 'Internal' },
              ]}
              title="Customer Type"
            />
            <Input
              className="main-input"
              titleLabel="Billing Profile Id"
              placeholder="Billing profile Id"
            />
          </Box>
          <Box className="main-checkgroup">
            <Typography variant="body1">Demo Customer :</Typography>
            <RadioGroup
              className="check-group"
              defaultValue="Yes"
              errorText=""
              helperText=""
            >
              <Radio label="Yes" value="Yes" />
              <Radio label="No" value="No" />
            </RadioGroup>
          </Box>
        </Box>
      </Box>
   )
}
export default CustomerDetail
