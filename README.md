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
