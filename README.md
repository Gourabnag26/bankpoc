import React, { useEffect } from 'react';
import { useOktaAuth } from '@okta/okta-react';
import { toRelativeUrl } from '@okta/okta-auth-js';
import { Outlet } from 'react-router-dom';
import { PageSpinner } from '@ucl/ui-components';


export const SecureRoute: React.FC = () => {

  const { oktaAuth, authState } = useOktaAuth();

  useEffect(() => {
    if (!authState) {
      return;
    }

    if (!authState?.isAuthenticated) {
      const originalUri = toRelativeUrl(window.location.href, window.location.origin);
      oktaAuth.setOriginalUri(originalUri);
      oktaAuth.signInWithRedirect();
    }
    
  }, [oktaAuth, authState?.isAuthenticated, authState]);

  if (!authState || !authState?.isAuthenticated) {
    return (
      <PageSpinner
        isLoading
        spinnerTestId="loading-spinner"
      />
    );
  }

  return (<Outlet />);
};





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


import { Outlet } from 'react-router-dom';
import { useLocation } from 'react-router-dom';
import { Banner } from '../../components/banner/banner';
import TopNavigation from '../../components/top-navigation/top-navigation';
import { AppBanner } from '../../scene';
import { CustomerProvider } from '../../context/customer-context';
import './page-layout.css';

export interface PageLayoutProps {
  children?: React.ReactNode;
}

export const PageLayout = (props: PageLayoutProps) => {
  const { pathname } = useLocation();
  const appBanner = AppBanner();

  let bannerTitle = '';

  for (const [key, titleValue] of Object.entries(appBanner)) {
    if (pathname === '/' && key === '/') {
      bannerTitle = titleValue;
    }
    if (pathname === '/customer-search' && key === '/customer-search') {
      bannerTitle = titleValue;
    }
    if (pathname === '/customer-profile/' && key === '/customer-profile') {
      bannerTitle = titleValue;
    }
  }

  return (
    <CustomerProvider>
      <TopNavigation />
      <Banner title={bannerTitle} />
      <div className="page-layout">
        <Outlet />
      </div>
    </CustomerProvider>
  );
};



