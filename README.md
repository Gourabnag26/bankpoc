import * as React from 'react';
import { Router } from '../shared/components/router/router';
import { IRouter } from '../shared/model';
import { IPARoutes } from '../shared/routes';
import PageLayout from './pages/page-layout';
import PageNotFound from './pages/page-not-found';
import CustomerSearch from './pages/customer-search';
import CustomerProfile from './pages/customer-profile';
import MyTasks from './pages/my-tasks';
import { useLocation } from 'react-router-dom';
import { Security } from '@okta/okta-react';
import {LoginCallback} from '@okta/okta-react';
import { useOktaAuthService } from 'shared/hooks/use-okta-authentication';
import {SecureRoute} from '../shared/components/router/secure-route/secure-route'

export const scene: IRouter[] = [
  {
    element: <SecureRoute /> ,
    path: IPARoutes.myTasks,
    title: 'layout',
    childRoutes: [
      {
        element: <MyTasks />,
        path: IPARoutes.myTasks,
        title: 'mytask',
      },
      {
        element: <CustomerSearch/>,
        path: IPARoutes.customerSearch,
        title: 'customersearch',
      },
      {
        element: <CustomerProfile />,
        path:  IPARoutes.customerProfile,
        title: 'Customer-profile',
      },
    ],
  },
  
  {
    element: <PageNotFound />,
    path: '*',
    title: '404page',
  },
  {
    element: <LoginCallback/>,
    path: '/login/callback',
    title: '404page',
  },
];

export function AppBanner() {
  const location = useLocation();
  const searchParams = new URLSearchParams(location.search);
  const role= searchParams.get("role");
  const mode= searchParams.get("mode");

  const titleMap: Record <string, string> = {
    [IPARoutes.error]: "Error",
    [IPARoutes.myTasks]: 'My Tasks' ,
    [IPARoutes.customerSearch]: "Customer Search",
    [IPARoutes.customerProfile]: (IPARoutes.customerProfile === "/customer-profile" && role ==="approver" && mode === "edit") ? `Customer Profile - Approval View` : IPARoutes.customerProfile === "/customer-profile" ? `Customer Profile - ${mode?.toUpperCase() || 'VIEW'}` : '',
    //Add path after creation of new page to update banner
  }
  return titleMap;
}

const IPAPages: React.FC = () => {
  const { getOktaConfig, restoreOktaOriginalUri } = useOktaAuthService();

  return (
    <Security
      oktaAuth={getOktaConfig()}
      restoreOriginalUri={restoreOktaOriginalUri}
    >
      <PageLayout>
        <Router pages={scene} />
      </PageLayout>
      
    </Security>
  );
};

export default IPAPages;




Okwidp-dev
400
Bad Request
Your request resulted in an error. The 'redirect_uri' parameter must be a Login redirect URI in the client app settings: https://okwidp-dev-admin.oktapreview.com/admin/app/oidc_client/instance/0oaqsv5cn7lUDGuDO1d7#tab-general
