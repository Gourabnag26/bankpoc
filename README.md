import { AxiosRequestConfig } from 'axios';
import { HttpService } from '../../http';
import { InternalAxiosRequestConfig } from 'axios';

const BASE_URL = '';
export const DUMMY_API_URL =
  'https://api2.ipa.dev1.r53comerica.net/api/customer/v1/customer/';

export class DemoServiceApi {
  private readonly httpService: HttpService;

  constructor() {
    this.httpService = new HttpService(BASE_URL);
  }

  async getReports(options?: AxiosRequestConfig) {
    try {
      return await this.httpService.get(DUMMY_API_URL,options);
    } catch (error) {
      console.error(
        'Network error in getExceptionData, falling back to mock data',
        error
      );
    }
  }

    addHeader() {
    return this.httpService.getAxiosInstance().interceptors.request.use(
      async (request: InternalAxiosRequestConfig) => {
        request.headers = request.headers || {};
          request.headers['gatewayCustomerId'] = "96689312-7417-47dc-b207-831a88a31620";
          request.headers['traceId'] ="cadc981b-18fe-4b46-994c-43f839f6912b";
        return request;
      },
      (error: any) => {
        return Promise.reject(error);
      }
    );
  }
   
}
