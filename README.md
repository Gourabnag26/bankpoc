import React, { createContext, ReactNode, useContext, useState } from 'react';
import { Constants } from 'shared';


interface AuthContextProps {
  currentUser: Object;
  isLoggedIn: boolean;
  isShadowing: boolean;
  navigationTabs: NavigationTabsType[];
  entitlements: ExceptionEntitlements[];
  setEntitlements: (entitlements: ExceptionEntitlements[]) => void;
  setNavigationTabs: (user: any) => void;
  setLoggedinUser: (user: any) => void;

}

const AuthContext = createContext<AuthContextProps>({
  currentUser: {},
  isLoggedIn: false,
  isShadowing: false,
  navigationTabs: [],
  entitlements: [],
  setEntitlements: (entitlements: ExceptionEntitlements[]) => {},
  setNavigationTabs: (user: any) => {},
  setLoggedinUser: (user: any) => {},

});

interface AuthProviderProps {
  children: ReactNode;
}

export interface ExceptionEntitlements {
  accountNumber: string;
  reversePositivePay: {
    view: boolean;
    update: boolean;
    reports: boolean;
    approval: boolean;
  };
}

export interface NavigationTabsType {
  id: string;
  title: string;
  path: string;
}

export const AuthProvider: React.FC<AuthProviderProps> = ({
  children,
}: any) => {
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [isShadowing, setIsShadowing] = useState(false);
  const [navigationTabs, setNavigationTabs] = useState<NavigationTabsType[]>(
    []
  );

  const [currentUser, setCurrentUser] = useState<any>(
    JSON.parse(sessionStorage.getItem(Constants.USER_INFO_KEY)!!)
  );
  const [entitlements, setEntitlements] = useState<ExceptionEntitlements[]>([]);



  const setLoggedinUser = (user: any) => {
    setCurrentUser(user);
  };

  return (
    <AuthContext.Provider
      value={{
        currentUser,
        isLoggedIn,
        isShadowing,
        navigationTabs,
        entitlements,
        setEntitlements,
        setNavigationTabs,
        setLoggedinUser,
 
      }}
    >
      {children}
    </AuthContext.Provider>
  );
};

export const useAuth = () => {
  return useContext(AuthContext);
};
