import React, { useState } from "react";
import {
  Box,
  Typography,
  Button,
  Dialog,
} from "@ucl/ui-components";
import { useLocation } from "react-router-dom";
import CustomerDetail from "./components/customer-info/customer-info";
import BusinessContact from "./components/business-contact/business-contact";
import TechnicalContact from "./components/technical-contact/technical-contact";
import TransactionLimit from "./components/transaction-limit/transaction-limit";
import ApiCustomerConfig from "./components/api-customer/api-customer";
import AccountPopup from "../../components/account-popup/account-popup";
import AccountSearchTable from "../../components/account-table/account-table";

export const CustomerProfile = () => {
  const location = useLocation();
  const searchParams = new URLSearchParams(location.search);
  const role = searchParams.get("role");

  const isViewMode = role === "view"; // true if ?role=view

  const initialCustomerData = {
    customerName: "",
    cisNumber: 1,
    customerType: "Commericial",
    billingProfileId: "",
    demoCustomer: "Yes",
    businessContactName: "",
    businessContactEmail: "",
    businessContactPhone: "",
    businessContactPreferredMethod: "Email",
    technologyContactName: "",
    technologyContactEmail: "",
    technologyContactPhone: "",
    technologyContactPreferredMethod: "Email",
    transactionLimit: 0,
    dailyLimit: 0,
    processingWindowStartTime: "00:00",
    processingWindowEndTime: "24:00",
    processingWindowZone: "GMT",
  };

  const [customer, setCustomer] = useState(initialCustomerData);
  const [accountPopup, setAccountPopup] = useState(false);

  return (
    <Box className="main-profile">
      <CustomerDetail customer={customer} setCustomer={setCustomer} disabled={isViewMode} />
      <BusinessContact customer={customer} setCustomer={setCustomer} disabled={isViewMode} />
      <TechnicalContact customer={customer} setCustomer={setCustomer} disabled={isViewMode} />
      <TransactionLimit customer={customer} setCustomer={setCustomer} disabled={isViewMode} />
      <ApiCustomerConfig customer={customer} setCustomer={setCustomer} disabled={isViewMode} />

      <Box className="section">
        <Box className="group-head">
          <Typography variant="h3" className="main-header" fontStyle="italic">
            Account Access & Preferences
          </Typography>
          <Button
            className="button"
            variant="primary"
            children="Add Account"
            onClick={() => setAccountPopup(true)}
            disabled={isViewMode} // disable button in view mode
          />
          <Dialog
            dialogContent={<AccountPopup />}
            dialogTitle="TIST"
            open={accountPopup}
            onClose={() => setAccountPopup(false)}
            fullWidth
          />
        </Box>
        <AccountSearchTable />
      </Box>
    </Box>
  );
};
