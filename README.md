import React from 'react';
import { IcustomerProps } from '../../customer-profile';
import { Box, Input, Typography, Select } from '@ucl/ui-components';
import '../../customer-profile.css';
import { useState } from 'react';
import TimeInput from '../../../../components/time-input/time-input';
const TransactionLimit = ({
  customer,
  setCustomer,
  disabled,
}: IcustomerProps) => {
  const [startTime, setStartTime] = useState(
    customer?.customerSettings?.processingWindow?.split('-')[0] || ''
  );
  const [endTime, setEndTime] = useState(
    customer?.customerSettings?.processingWindow?.split('-')[1] || ''
  );
  return (
    <Box className="section">
      <Typography variant="h3" className="main-header" fontStyle="italic">
        Limits
      </Typography>
      <Box className="sub-section">
        <Box className="main-container">
          <Input
            className="main-input"
            titleLabel="Transaction Limit"
            value={customer?.customerSettings?.transactionLimit}
            placeholder="Transaction Limit"
            disabled={disabled}
          />
          <Input
            className="main-input"
            titleLabel="Daily Limit"
            placeholder="Daily Limit"
            disabled={disabled}
          />
        </Box>
        <Box className="main-container">
          <Box>
            <Typography className="sub-header">Processing Window</Typography>
            <Box className="time-container">
              <TimeInput
                time={startTime}
                setTime={setStartTime}
                disabled={disabled}
              />
              <span> - </span>
              <TimeInput
                time={endTime}
                setTime={setEndTime}
                disabled={disabled}
              />
              <Select
                className="time-input"
                value={customer?.customerSettings?.processingWindowTimezone || "GMT"}
                options={[
                  { key: 'GMT', value: 'GMT', text: 'GMT' },
                  { key: 'UTC', value: 'UTC', text: 'UTC' },
                  { key: 'IST', value: 'IST', text: 'IST' },
                  { key: 'CST', value: 'CST', text: 'CST' },
                ]}
                disabled={disabled}
              />
            </Box>
          </Box>
        </Box>
      </Box>
    </Box>
  );
};
export default TransactionLimit;
