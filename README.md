@GetMapping("/customer")
    public Customer getCustomer(@Schema(description = "Gateway customer id") @RequestHeader(CustomHeaders.GATEWAY_CUSTOMER_ID) String gatewayCustomerId,
                                @Schema(description = "Trace id") @RequestHeader(CustomHeaders.TRACE_ID) UUID traceId) {
        log.info("Retrieving Customer info for : {}", gatewayCustomerId);
        return customerConfigurationService.getCustomer(gatewayCustomerId);
    }

    @Event(operationName = ApiOperation.RETRIEVE_BILLING_TRANSACTIONS)
    @GetMapping("/customer/billingTransaction")
    public List<CustomerBillingTransaction> getBillingTransaction(@Schema(description = "Gateway customer id") @RequestHeader(CustomHeaders.GATEWAY_CUSTOMER_ID) String gatewayCustomerId,
                                                                  @Schema(description = "Trace id") @RequestHeader(value = CustomHeaders.TRACE_ID) UUID traceId,
                                                                  @Schema(description = "Billing account Number") @RequestHeader(value = CustomHeaders.BILLING_ACCOUNT_NUMBER, required = false) String billingAccountNumber,
                                                                  @Schema(description = "Transaction Account Number") @RequestHeader(value = CustomHeaders.TRANSCATION_ACCOUNT_NUMBER, required = false) String accountNumber,
                                                                  @Schema(description = "From Date") @RequestHeader(value = CustomHeaders.FROM_DATE, required = false) LocalDateTime fromDate,
                                                                  @Schema(description = "To Date") @RequestHeader(value = CustomHeaders.TO_DATE, required = false) LocalDateTime toDate) {
        log.info("Retrieving billing transaction info for : {}", gatewayCustomerId);
        return customerBillingTransactionsService.findBillingTransactions(gatewayCustomerId, billingAccountNumber, accountNumber, fromDate, toDate);
    }

    @Event(operationName = ApiOperation.UPDATE_BILLING_TRANSACTION)
    @PostMapping("/customer/billingTransaction")
    public CustomerBillingTransaction persistBillingTransaction(@Schema(description = "Gateway customer id") @RequestHeader(CustomHeaders.GATEWAY_CUSTOMER_ID) String gatewayCustomerId,
                                                                @Schema(description = "Trace id") @RequestHeader(value = CustomHeaders.TRACE_ID) UUID traceId,
                                                                @Schema(description = "Customer Billing Transaction bean") @Valid @RequestBody CustomerBillingTransaction customerBillingTransaction) {
        return customerBillingTransactionsService.persistCustomerBillingTransactions(customerBillingTransaction, traceId.toString());
    }
