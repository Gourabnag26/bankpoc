import React, { useState, useEffect } from "react";
import {
  Box,
  Input,
  Typography,
  Radio,
  Checkbox,
  Select,
  Button,
} from "@ucl/ui-components";
import CardCheckbox from "../checkbox-menu/checkbox-menu";

const AccountPopup = () => {
  const api_general_options = [
    { id: "acc_balance", label: "Account Balance", value: "Account Balance" },
  ];
  const api_rtp_options = [
    {
      id: "ipa_create_credit",
      label: "Create Credit",
      value: "Instant Payments - Create Credit",
    },
    {
      id: "ipa_recieve_credit",
      label: "Retrieve Status",
      value: "Instant Payments - Receive Credit",
    },
  ];
  const api_fednow_options = [
    { id: "fednow_createcredit", label: "Create Credit", value: "FedNow - Create Credit" },
    { id: "fednow_recievecredit", label: "Retrieve Status", value: "FedNow - Receive Credit" },
  ];
  const api_wire_options = [
    { id: "wire_create_credit", label: "Create Credit", value: "Wire - Create Credit" },
    { id: "wire_retieve_credit", label: "Retrieve Status", value: "Wire - Retrieve Credit" },
    { id: "wire_webhook", label: "Webhook Status Update", value: "Wire - Webhook Retrieve" },
  ];

  // individual checkbox states
  const [apiGeneralValues, setApiGeneralValues] = useState<string[]>([]);
  const [apiRTPValues, setApiRTPValues] = useState<string[]>([]);
  const [apiFedNowValues, setApiFedNowValues] = useState<string[]>([]);
  const [apiWiresValues, setApiWiresValues] = useState<string[]>([]);

  // select all state
  const [isAllSelected, setIsAllSelected] = useState(false);

  // Flatten all API ids for "Select all"
  const allApiIds = [
    ...api_general_options.map((c) => c.id),
    ...api_rtp_options.map((c) => c.id),
    ...api_fednow_options.map((c) => c.id),
    ...api_wire_options.map((c) => c.id),
  ];

  // Whenever any individual checkbox changes, update select all state
  useEffect(() => {
    const allSelected =
      apiGeneralValues.length === api_general_options.length &&
      apiRTPValues.length === api_rtp_options.length &&
      apiFedNowValues.length === api_fednow_options.length &&
      apiWiresValues.length === api_wire_options.length;
    setIsAllSelected(allSelected);
  }, [apiGeneralValues, apiRTPValues, apiFedNowValues, apiWiresValues]);

  // Handle select all
  const handleSelectAllChange = (checked: boolean) => {
    if (checked) {
      setApiGeneralValues(api_general_options.map((c) => c.id));
      setApiRTPValues(api_rtp_options.map((c) => c.id));
      setApiFedNowValues(api_fednow_options.map((c) => c.id));
      setApiWiresValues(api_wire_options.map((c) => c.id));
    } else {
      setApiGeneralValues([]);
      setApiRTPValues([]);
      setApiFedNowValues([]);
      setApiWiresValues([]);
    }
  };

  return (
    <>
      {/* Customer Info Section */}
      <Box className="section">
        <Typography variant="h3" className="main-header" fontStyle="italic">
          Customer Info
        </Typography>
        <Box sx={{ padding: "10px" }}>
          <Box sx={{ display: "flex", alignItems: "center", gap: "50px" }}>
            <Input
              className="main-input"
              titleLabel="Customer Name"
              placeholder="Customer Name"
              sx={{ width: "250px", height: "40px", marginTop: "10px" }}
            />
            <Checkbox sx={{ width: "250px" }} label="Billing Account" />
          </Box>
          <Box sx={{ display: "flex", alignItems: "center", justifyContent: "space-between" }}>
            <Input
              className="main-input"
              titleLabel="Account Number"
              placeholder="Account Number"
              sx={{ width: "250px", height: "40px", marginTop: "10px" }}
            />
            <Select
              className="main-input"
              value="10002"
              title="Bank Code"
              sx={{ width: "250px", height: "40px", marginTop: "15px" }}
              options={[{ key: "10002", value: "10002", text: "10002" }]}
            />
          </Box>
          <Box>
            <Input
              className="main-input"
              titleLabel="Router Number"
              placeholder="Router Number"
              sx={{ width: "250px", height: "40px", marginTop: "10px" }}
            />
            <Input
              className="main-input"
              titleLabel="Authorized Transaction Account Routing Number"
              placeholder="Authorized Transaction Account Routing Number"
              sx={{ width: "250px", height: "40px", marginTop: "10px" }}
            />
          </Box>
        </Box>
      </Box>

      {/* Limits Section */}
      <Box className="section">
        <Typography variant="h3" className="main-header" fontStyle="italic">
          Limits
        </Typography>
        <Box sx={{ padding: "10px" }}>
          <Box sx={{ display: "flex", alignItems: "center", justifyContent: "space-between" }}>
            <Input
              className="main-input"
              titleLabel="Transaction Limit"
              placeholder="Transaction Limit"
              sx={{ width: "250px", height: "40px", marginTop: "10px" }}
            />
            <Input
              className="main-input"
              titleLabel="Daily Limit"
              placeholder="Daily Limit"
              sx={{ width: "250px", height: "40px", marginTop: "10px" }}
            />
          </Box>
        </Box>
      </Box>

      {/* API Section */}
      <Box className="section">
        <Box sx={{ display: "flex", alignItems: "center", gap: "30px" }}>
          <Typography variant="h3" className="main-header" fontStyle="italic">
            Api
          </Typography>
          <Checkbox
            label="Select all"
            checked={isAllSelected}
            onChange={(e: any) => handleSelectAllChange(e.target.checked)}
          />
        </Box>
        <Box
          className="checkbox-container sub-section"
          sx={{
            display: "flex",
            flexDirection: "row",
            gap: "10px",
            padding: "10px",
            alignItems: "center",
            flexWrap: "wrap",
          }}
        >
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

      {/* Buttons */}
      <Box sx={{ display: "flex", justifyContent: "center", gap: "10px", marginTop: "10px" }}>
        <Button
          sx={{
            border: "1px solid black",
            background: "#F5F5F5",
            padding: "10px",
            fontSize: "12px",
            color: "black",
            width: "150px",
            borderRadius: "5px",
          }}
        >
          Save
        </Button>
        <Button
          sx={{
            border: "1px solid black",
            background: "#F5F5F5",
            padding: "10px",
            fontSize: "12px",
            color: "black",
            width: "150px",
            borderRadius: "5px",
          }}
        >
          Cancel
        </Button>
      </Box>
    </>
  );
};

export default AccountPopup;
