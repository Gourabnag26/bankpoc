import React from 'react';
import { IcustomerProps } from '../../customer-profile';
import { Box, Input, Typography } from '@ucl/ui-components';
import RadioGroup from '../../../../components/radio-group/radio-group';
import '../../customer-profile.css';

const BusinessContact = ({ customer, setCustomer, disabled }: IcustomerProps) => {
  const existingContact = customer?.customerContacts?.find(
    (c) => c.contactType === 'BUSINESS'
  );

  // Update the contact info immutably
  const handleContactChange = (field: string, value: string) => {
    setCustomer((prev) => {
      const contacts = [...(prev.customerContacts || [])];
      const index = contacts.findIndex((c) => c.contactType === 'BUSINESS');

      if (index >= 0) {
        contacts[index] = { ...contacts[index], [field]: value };
      } else {
        contacts.push({
          contactType: 'BUSINESS',
          contactName: '',
          contactTitle: '',
          contactPhone: '',
          contactPhoneType: '',
          contactEmail: '',
          contactPreferredType: '',
          enabled: true,
          [field]: value,
        });
      }

      return {
        ...prev,
        customerContacts: contacts,
      };
    });
  };

  const handlePreferredChange = (value: string) => {
    handleContactChange('contactPreferredType', value);
  };

  return (
    <Box className="section">
      <Typography variant="h3" className="main-header" fontStyle="italic">
        Business Contact
      </Typography>

      <Box className="sub-section">
        {/* Name and Phone */}
        <Box className="main-container">
          <Input
            className="main-input"
            titleLabel="Name"
            placeholder="Name"
            value={existingContact?.contactName || ''}
            disabled={disabled}
            onChange={(e: any) => handleContactChange('contactName', e.target.value)}
          />

          <Input
            className="main-input"
            titleLabel="Phone"
            placeholder="Phone Number"
            value={existingContact?.contactPhone || ''}
            disabled={disabled}
            onChange={(e: any) => handleContactChange('contactPhone', e.target.value)}
          />
        </Box>

        {/* Email */}
        <Box className="main-container">
          <Input
            className="main-input"
            titleLabel="Email"
            placeholder="Email"
            value={existingContact?.contactEmail || ''}
            disabled={disabled}
            onChange={(e: any) => handleContactChange('contactEmail', e.target.value)}
          />
        </Box>

        {/* Preferred Contact Method */}
        <Box className="main-checkgroup">
          <Typography variant="body1">Preferred Method :</Typography>
          <RadioGroup
            name="contactMethodBusiness"
            selectedValue={existingContact?.contactPreferredType || 'Email'}
            disabled={disabled}
            options={[
              { label: 'Email', value: 'Email' },
              { label: 'Phone', value: 'Phone' },
            ]}
            onChange={handlePreferredChange}
          />
        </Box>
      </Box>
    </Box>
  );
};

export default BusinessContact;
