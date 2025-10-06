export interface ICustomer {
  gatewayCustomerId: string;
  name: string;
  cisNumbers: string[];
  enabled: boolean;
  customerSettings: ICustomerSettings;
  customerType: string;
  virtualAcctCustomer: boolean;
  demoCustomer: boolean;
  customerAccounts: ICustomerAccount[];
  customerProducts: ICustomerProduct[];
  billingCustomerId: string;
  createdBy: string;
  customerContacts: ICustomerContact[];
}

export interface ICustomerSettings {
  transactionLimit: number;
  cumulativeTransactionLimit: number;
  processingWindow: string;
  processingWindowTimezone: string;
}

export interface ICustomerAccount {
  number: string;
  name: string;
  routingNumber: string;
  bankCode: string;
  cisNumbers: string[];
  accountSettings: IAccountSettings;
  billingAccount: boolean;
  enabled: boolean;
  products: ICustomerProduct[];
}

export interface IAccountSettings {
  transactionLimit: number;
  cumulativeTransactionLimit: number;
}

export interface ICustomerProduct {
  id: string | null;
  name: string;
  friendlyName: string | null;
  shortName: string | null;
  description: string | null;
  productSettings: IProductSettings | null;
  enabled: boolean;
  billable: boolean;
  resources: IProductResource[];
  paymentRails: IPaymentRail[] | null;
}

export interface IProductSettings {
  transactionLimit?: number;
  cumulativeTransactionLimit?: number;
  duplicateCheckDuration?: number;
}

export interface IProductResource {
  name: string;
  friendlyName: string | null;
  description: string | null;
  enabled: boolean;
  billable: boolean;
}

export interface IPaymentRail {
  name: string;
  friendlyName: string | null;
  description: string | null;
  paymentRailSettings: IPaymentRailSettings;
  enabled: boolean;
  billable: boolean;
}

export interface IPaymentRailSettings {
  transactionLimit: number;
  cumulativeTransactionLimit: number;
  duplicateCheckDuration: number;
  allowedCreditAccountList?: IAllowedCreditAccount[];
}

export interface IAllowedCreditAccount {
  accountNumber: string;
  routingNumber: string | null;
}

export interface ICustomerContact {
  contactName: string;
  contactTitle: string;
  contactPhone: string;
  contactPhoneType: string;
  contactEmail: string;
  contactPreferredType: string;
  contactType: string;
  enabled: boolean;
}
