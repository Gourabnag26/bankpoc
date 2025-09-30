
Yes 👍 you’re right — you already have an addAuthHeader function in your UserInitServiceApi that attaches headers (originally for Okta, but we repurposed it earlier for API keys).

That means you don’t need to pass the API key inside searchCustomer each time — you can keep it clean.


---

🔑 Updated UserInitServiceApi.ts (using your addAuthHeader)

import { AxiosRequestConfig, InternalAxiosRequestConfig } from 'axios';
import { HttpService } from '../../http';

const BASE_URL = 'https://api2ipa.dev1.lf3scomerica.net';
const CUSTOMER_SEARCH_ENDPOINT = '/api/customer/v1/customer/search';

export class UserInitServiceApi {
  private readonly httpService: HttpService;

  constructor() {
    this.httpService = new HttpService(BASE_URL);
  }

  /**
   * Add API key header once
   */
  addAuthHeader(apiKey: string) {
    return this.httpService.getAxiosInstance().interceptors.request.use(
      async (request: InternalAxiosRequestConfig) => {
        request.headers = request.headers || {};
        request.headers['X-API-KEY'] = apiKey;   // ✅ attach your API key
        request.headers['Accept'] = 'application/json';
        request.headers['Content-Type'] = 'application/json';
        return request;
      },
      (error: any) => Promise.reject(error)
    );
  }

  /**
   * POST call for customer search
   */
  async searchCustomer(
    data: {
      gatewayCustomerId: string;
      customerName: string;
      accountNumber: string;
      cisNumber: string;
    },
    options?: AxiosRequestConfig
  ) {
    return this.httpService.post(CUSTOMER_SEARCH_ENDPOINT, data, options);
  }
}


---

✅ Example Usage in Component

import React from 'react';
import { UserInitServiceApi } from './services/UserInitServiceApi';

const CustomerSearchComponent: React.FC = () => {
  const handleSearch = async () => {
    const api = new UserInitServiceApi();

    // 🔑 set auth header ONCE (uses API key from .env)
    api.addAuthHeader(process.env.REACT_APP_CUSTOMER_API_KEY as string);

    try {
      const response = await api.searchCustomer({
        gatewayCustomerId: 'aacfc63-9728-4dcb-88a1-7119a81c17de',
        customerName: 'Customer197',
        accountNumber: '7001233559',
        cisNumber: '5132445677'
      });

      console.log('Customer Search Response:', response.data);
    } catch (err: any) {
      console.error('API Error:', err.message || err);
    }
  };

  return (
    <div>
      <button onClick={handleSearch}>Search Customer</button>
    </div>
  );
};

export default CustomerSearchComponent;


---

✅ This way:

addAuthHeader takes care of API key + headers once.

searchCustomer stays clean (only body data).

In your component, you only call api.addAuthHeader(...) once, then call the API.



---

⚠️ Reminder: if this Comerica API doesn’t enable CORS for browser requests, you’ll still see a CORS error. In that case you’ll need to proxy through a backend.

👉 Do you want me to also show how to detect quickly whether this API is CORS-enabled before you spend time debugging frontend calls?

