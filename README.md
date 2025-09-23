import React, { createContext, ReactNode, useContext, useState } from "react";

interface Task {
  customerName: string;
  gatewayCustomerId: string;
  cisNumber: string;
  accountNumber: string;
}

type Role = "creator" | "editor" | "viewer" | "approver";

interface MyTasksContextProps {
  myTasks: Task[];
  setMyTasks: React.Dispatch<React.SetStateAction<Task[]>>;
  role: Role;
}

const MyTasksContext = createContext<MyTasksContextProps | undefined>(undefined);

const initialTasks: Task[] = [
  {
    customerName: "Clark Kent",
    gatewayCustomerId: "6a4ea94-b029-4a59-94b4-54291f72ccfc",
    cisNumber: "1234567890",
    accountNumber: "8763678946",
  },
  {
    customerName: "Anthony Stark",
    gatewayCustomerId: "32d7a2fa-fd51-4e15-af76-78a5c4eb1e6a",
    cisNumber: "1234567880",
    accountNumber: "8475629375",
  },
  {
    customerName: "Steve Rogers",
    gatewayCustomerId: "77883a9e-070a-4dc5-aefc-e794fe4a2381",
    cisNumber: "1234567385",
    accountNumber: "9374579459",
  },
  {
    customerName: "Bruce Wayne",
    gatewayCustomerId: "b9d4cb10-4b62-45a7-849d-82fbe7b9a761",
    cisNumber: "1234567550",
    accountNumber: "7539753874",
  },
  {
    customerName: "Peter Parker",
    gatewayCustomerId: "ebc0ae64-cb26-4deb-b7c6-3d0a5acac1b7",
    cisNumber: "12449098768",
    accountNumber: "4627482947",
  },
];

export const MyTasksProvider = ({ children }: { children: ReactNode }) => {
  const [myTasks, setMyTasks] = useState<Task[]>(initialTasks);
  const role: Role = "creator"; // hardcoded role for now

  return (
    <MyTasksContext.Provider value={{ myTasks, setMyTasks, role }}>
      {children}
    </MyTasksContext.Provider>
  );
};

export const useMyTasks = () => {
  const context = useContext(MyTasksContext);
  if (!context) {
    throw new Error("useMyTasks must be used within a MyTasksProvider");
  }
  return context;
};
