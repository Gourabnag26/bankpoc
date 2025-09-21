import React from 'react';
import { IcustomerProps } from '../../customer-profile';
import { Box, Input,Typography,Select } from '@ucl/ui-components';
import '../../customer-profile.css';
import { useState } from 'react';
import TimeInput from 'src/components/time-input/time-input';
const TransactionLimit = ({ customer, setCustomer }: IcustomerProps) => {
  const [startTime, setStartTime] = useState('00:00');
  const [endTime, setEndTime] = useState('00:00');
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
            placeholder="Transaction Limit"
          />
          <Input
            className="main-input"
            titleLabel="Daily Limit"
            placeholder="Daily Limit"
          />
        </Box>
        <Box className="main-container">
          <Box>
            <Typography className="sub-header">Processing Window</Typography>
            <Box className="time-container">
              <TimeInput time={startTime} setTime={setStartTime} />
              <span> - </span>
              <TimeInput time={endTime} setTime={setEndTime} />
              <Select
                className="time-input"
                value="GMT"
                options={[
                  { key: 'GMT', value: 'GMT', text: 'GMT' },
                  { key: 'UTC', value: 'UTC', text: 'UTC' },
                  { key: 'IST', value: 'IST', text: 'IST' },
                  { key: 'CST', value: 'CST', text: 'CST' },
                ]}
              />
            </Box>
          </Box>
        </Box>
      </Box>
    </Box>
  );
};
export default TransactionLimit;
