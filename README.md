import React, { useState, useEffect } from 'react';
import { IcustomerProps } from '../../customer-profile';
import { Box, Input, Typography, Select } from '@ucl/ui-components';
import '../../customer-profile.css';
import TimeInput from '../../../../components/time-input/time-input';

const TransactionLimit = ({ customer, setCustomer, disabled }: IcustomerProps) => {
  const settings = customer?.customerSettings || {};

  const [transactionLimit, setTransactionLimit] = useState(settings?.transactionLimit || '');
  const [dailyLimit, setDailyLimit] = useState(settings?.cumulativeTransactionLimit || '');
  const [startTime, setStartTime] = useState(settings?.processingWindow?.split('-')[0] || '');
  const [endTime, setEndTime] = useState(settings?.processingWindow?.split('-')[1] || '');
  const [timezone, setTimezone] = useState(settings?.processingWindowTimezone || 'GMT');

  // 🧠 Sync local state to customer whenever fields change
  useEffect(() => {
    setCustomer((prev) => ({
      ...prev,
      customerSettings: {
        ...prev.customerSettings,
        transactionLimit,
        cumulativeTransactionLimit: dailyLimit,
        processingWindow:
          startTime && endTime ? `${startTime}-${endTime}` : prev.customerSettings?.processingWindow,
        processingWindowTimezone: timezone,
      },
    }));
  }, [transactionLimit, dailyLimit, startTime, endTime, timezone]);

  return (
    <Box className="section">
      <Typography variant="h3" className="main-header" fontStyle="italic">
        Limits
      </Typography>

      <Box className="sub-section">
        {/* Transaction and Daily Limit */}
        <Box className="main-container">
          <Input
            className="main-input"
            titleLabel="Transaction Limit"
            value={transactionLimit}
            placeholder="Transaction Limit"
            disabled={disabled}
            onChange={(e: any) => setTransactionLimit(e.target.value)}
          />
          <Input
            className="main-input"
            titleLabel="Daily Limit"
            placeholder="Daily Limit"
            value={dailyLimit}
            disabled={disabled}
            onChange={(e: any) => setDailyLimit(e.target.value)}
          />
        </Box>

        {/* Processing Window */}
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
                value={timezone}
                options={[
                  { key: 'GMT', value: 'GMT', text: 'GMT' },
                  { key: 'UTC', value: 'UTC', text: 'UTC' },
                  { key: 'IST', value: 'IST', text: 'IST' },
                  { key: 'CST', value: 'CST', text: 'CST' },
                ]}
                disabled={disabled}
                onChange={(e: any) => setTimezone(e.target.value)}
              />
            </Box>
          </Box>
        </Box>
      </Box>
    </Box>
  );
};

export default TransactionLimit;
