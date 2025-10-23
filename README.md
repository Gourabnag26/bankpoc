// Add this helper inside your component
const updateAccountProducts = (category: keyof ApiSelectionState, selectedIds: string[]) => {
  setAccountState(prev => {
    let products = [...(prev.products || [])];

    if (category === 'general') {
      const hasSelection = selectedIds.includes('acc_balance');
      const existing = products.find(p => p.name === 'ACCOUNT_BALANCE_API');

      if (hasSelection && !existing) {
        products.push({
          id: null,
          name: 'ACCOUNT_BALANCE_API',
          resources: [{ name: 'RETRIEVE_ACCOUNT_BALANCE', enabled: true }],
          paymentRails: null,
        });
      } else if (!hasSelection && existing) {
        products = products.filter(p => p.name !== 'ACCOUNT_BALANCE_API');
      }
    } else {
      const railNameMap = { rtp: 'RTP', fednow: 'FEDNOW', wires: 'WIRES' };
      const railName = railNameMap[category];

      let ipaProduct = products.find(p => p.name === 'INSTANT_PAYMENTS_API');
      if (!ipaProduct && selectedIds.length > 0) {
        ipaProduct = { id: null, name: 'INSTANT_PAYMENTS_API', resources: [], paymentRails: [] };
        products.push(ipaProduct);
      }

      if (ipaProduct) {
        if (selectedIds.length > 0 && !ipaProduct.resources.some(r => r.name === 'CREATE_CREDIT_TRANSFER')) {
          ipaProduct.resources.push({ name: 'CREATE_CREDIT_TRANSFER', enabled: true });
        }

        ipaProduct.paymentRails = ipaProduct.paymentRails || [];
        if (selectedIds.length > 0) {
          if (!ipaProduct.paymentRails.some(r => r.name === railName)) {
            ipaProduct.paymentRails.push({ name: railName, enabled: true });
          }
        } else {
          ipaProduct.paymentRails = ipaProduct.paymentRails.filter(r => r.name !== railName);
        }

        if (ipaProduct.paymentRails.length === 0 && ipaProduct.resources.length === 0) {
          products = products.filter(p => p.name !== 'INSTANT_PAYMENTS_API');
        }
      }
    }

    return { ...prev, products };
  });
};

// Update handleChange to also update accountState
const handleChange = (target: keyof ApiSelectionState) => (value: string[]) => {
  setApiSelection(prev => ({
    ...prev,
    [target]: { ...prev[target], selectedValue: value }
  }));
  updateAccountProducts(target, value);
};

// Update handleSelectAllChange to sync accountState
const handleSelectAllChange = (checked: boolean) => {
  setApiSelection(prev => {
    const newSelection = { ...prev };
    Object.keys(newSelection).forEach(key => {
      const api = newSelection[key as keyof ApiSelectionState];
      if (!api.disabled) {
        api.selectedValue = checked ? api.options.map(o => o.id) : [];
        updateAccountProducts(key as keyof ApiSelectionState, api.selectedValue); // sync
      }
    });
    return newSelection;
  });
  setIsAllSelected(checked);
};
