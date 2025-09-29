import { AxiosRequestConfig } from 'axios';
import { InternalAxiosRequestConfig } from 'axios';
import { OktaAuth } from '@okta/okta-auth-js';
import { HttpService } from '../../http';
import { environment } from 'src/environments/environment';

export const LOGIN_ENDPOINT = environment.services.login.URL_PATH;
export const ENTITLEMENTS_ENDPOINT = environment.services.entitlements.URL_PATH;
export const LOGOUT_ENDPOINT = environment.services.logout.URL_PATH;
export const EXTEND_SESSION_ENDPOINT = environment.services.extend.URL_PATH;

const BASE_URL = process.env.BASE_URL || '';

export class UserInitServiceApi {
  static TOKEN = Symbol('UserInitApi');
  private readonly httpService: HttpService;

  constructor() {
    this.httpService = new HttpService(BASE_URL);
  }

  async login(data: any, options?: AxiosRequestConfig) {
    return this.httpService.post(LOGIN_ENDPOINT, data, options);
  }

  async logout(options?: AxiosRequestConfig) {
    return this.httpService.delete(LOGOUT_ENDPOINT, options);
  }

  async extendSession(data: any, options?: AxiosRequestConfig) {
    return this.httpService.patch(EXTEND_SESSION_ENDPOINT, data, options);
  }

  async getEntitlements(options?: AxiosRequestConfig) {
    return this.httpService.get(ENTITLEMENTS_ENDPOINT, options);
  }

  addAuthHeader(oktaAuth: OktaAuth) {
    return this.httpService.getAxiosInstance().interceptors.request.use(
      async (request: InternalAxiosRequestConfig) => {
        request.headers = request.headers || {};
        if (oktaAuth) {
          request.headers['Authorization'] = oktaAuth.getAccessToken();
        }
        return request;
      },
      (error: any) => {
        return Promise.reject(error);
      }
    );
  }
}
