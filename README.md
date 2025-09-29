import { AxiosRequestConfig, InternalAxiosRequestConfig } from 'axios';
import { HttpService } from '../../http'; // ✅ your existing HttpService

// ✅ Hardcoded Base URL for testing
const BASE_URL = 'https://api.openweathermap.org/data/2.5';

// ✅ Sample endpoint
const WEATHER_ENDPOINT = '/weather';

export class UserInitServiceApi {
  static TOKEN = Symbol('UserInitApi');
  private readonly httpService: HttpService;

  constructor() {
    this.httpService = new HttpService(BASE_URL);
  }

  /**
   * Dynamically adds API key to every outgoing request
   * Call this once, for example at app startup or when user logs in
   */
  addAuthHeader(apiKey: string) {
    return this.httpService.getAxiosInstance().interceptors.request.use(
      async (request: InternalAxiosRequestConfig) => {
        request.headers = request.headers || {};

        if (apiKey) {
          // Add API key to headers
          request.headers['x-api-key'] = apiKey; 
        }

        return request;
      },
      (error: any) => Promise.reject(error)
    );
  }

  /**
   * Example GET request
   */
  async getWeatherByCity(city: string, options?: AxiosRequestConfig) {
    return this.httpService.get(
      `${WEATHER_ENDPOINT}?q=${city}&units=metric`,
      options
    );
  }
}

// ✅ Quick test
(async () => {
  const API_KEY = 'YOUR_API_KEY_HERE'; // <-- Replace with your actual API key
  const api = new UserInitServiceApi();

  // Dynamically add the key to headers
  api.addAuthHeader(API_KEY);

  try {
    const response = await api.getWeatherByCity('London');
    console.log('Weather Data for London:', response.data);
  } catch (error) {
    console.error('Error fetching weather data:', error);
  }
})();
