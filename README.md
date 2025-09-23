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





//data

{
"tableBody": [
    {
        "customerName":"Clark Kent",
        "gatewayCustomerId":"6a4ea94-b029-4a59-94b4-54291f72ccfc",
        "cisNumber": "1234567890",
        "accountNumber": "8763678946"
    },
    { 
        "customerName":"Anthony Stark",
        "gatewayCustomerId":"32d7a2fa-fd51-4e15-af76-78a5c4eb1e6a",
        "cisNumber": "1234567880",
        "accountNumber": "8475629375"
    },
    {     
        "customerName":"Steve Rogers",
        "gatewayCustomerId":"77883a9e-070a-4dc5-aefc-e794fe4a2381",
        "cisNumber": "1234567385",
        "accountNumber": "9374579459"
    },
    {     
        "customerName":"Bruce Wayne",
        "gatewayCustomerId":"b9d4cb10-4b62-45a7-849d-82fbe7b9a761",
        "cisNumber": "1234567550",
        "accountNumber": "7539753874"
    },
    { 
        "customerName":"Peter Parker",
        "gatewayCustomerId":"ebc0ae64-cb26-4deb-b7c6-3d0a5acac1b7",
        "cisNumber": "12449098768",
        "accountNumber": "4627482947"
    }
    
]}

role = creator,editor, viewer ,approver current role can be any of them
