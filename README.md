import { useEffect, useState } from "react";
import { useCustomer } from "../../context/customer-context";
import MyTasksTable from "../../components/my-tasks-table/my-tasks-table";
import MyTasksTableData from "../../../shared/dummy-json/my-tasks.json";
import { useRole } from "../../../shared/utils/role-manager";

export const MyTasks = () => {
  const [tableData] = useState(MyTasksTableData);
  const role = useRole();
  const { myTasks, setMyTasks } = useCustomer();
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Simulated API call
    const fetchDummyTasks = () => {
      setTimeout(() => {
        const dummyApiResponse = {
          draftList: [
            {
              taskId: 2,
              gatewayCustomerId: "ff8555cc-e370-4aba-ae92-dc513843199q",
              status: "PENDING_APPROVAL",
              customerName: "Test Customer1",
              customerConfig:
                '{"name": "Test Customer1", "enabled": true, "createdBy": "LIQUIBASE", "cisNumbers": ["6666999999"], "customerType": "COMMERCIAL", "demoCustomer": false, "customerAccounts": [{"name": "Test Customer1Account1", "number": "1001223559", "enabled": true, "bankCode": "88833", "cisNumbers": ["6666999999"], "routingNumber": "771666333", "billingAccount": true, "accountSettings": {"transactionLimit": 2000, "cumulativeTransactionLimit": 10000}}, {"name": "Test Customer1Account2", "number": "1002587325", "enabled": true, "bankCode": "66655", "cisNumbers": ["6666999999"], "routingNumber": "771666222", "billingAccount": false, "accountSettings": {"transactionLimit": 5000, "cumulativeTransactionLimit": 20000}}]}',
              createdBy: "User1",
              updatedBy: "User1",
            },
            {
              taskId: 1,
              gatewayCustomerId: "8b7af1ae-53f2-4e34-85cd-a86c896970df",
              status: "DRAFT",
              customerName: "Customer198",
              customerConfig:
                '{"name": "Customer198", "enabled": true, "createdBy": "LIQUIBASE", "cisNumbers": ["5132445678"], "customerType": "COMMERCIAL", "demoCustomer": false, "customerAccounts": [{"name": "197Account3", "number": "7005984461", "enabled": true, "bankCode": "77231", "cisNumbers": ["5132445678"], "routingNumber": "772666322", "billingAccount": true, "accountSettings": {"transactionLimit": 100, "cumulativeTransactionLimit": 30000}}, {"name": "197Account4", "number": "7006219180", "enabled": true, "bankCode": "45144", "cisNumbers": ["5132445678"], "routingNumber": "772666422", "billingAccount": false, "accountSettings": {"transactionLimit": 1000, "cumulativeTransactionLimit": 5000}}]}',
              createdBy: "User1",
              updatedBy: "User1",
            },
          ],
          completedList: [
            {
              taskId: 3,
              gatewayCustomerId: "c59e7d0b-b54d-429c-a000-ad260a370e7c",
              status: "APPROVED",
              customerName: "Customer503",
              customerConfig:
                '{"name": "Customer503", "enabled": true, "createdBy": "LIQUIBASE", "cisNumbers": ["1866535234"], "customerType": "COMMERCIAL", "demoCustomer": false, "customerAccounts": [{"name": "Customer503", "number": "1890623604", "enabled": true, "bankCode": "10002", "cisNumbers": ["1866535234"], "routingNumber": "111000753", "billingAccount": true, "accountSettings": {"transactionLimit": 11000, "cumulativeTransactionLimit": 15000}}]}',
              createdBy: "User1",
              updatedBy: "User1",
              approvedBy: "Approver1",
            },
          ],
          approveList: [],
        };

        // Combine all lists into a single array
        const combinedList = [
          ...dummyApiResponse.draftList,
          ...dummyApiResponse.completedList,
          ...dummyApiResponse.approveList,
        ];

        // Store it in context
        setMyTasks(combinedList);
        setLoading(false);
      }, 1500); // 1.5s delay to mimic API
    };

    fetchDummyTasks();
  }, [setMyTasks]);

  if (loading) {
    return <div>Loading tasks, please wait...</div>;
  }

  return (
    <MyTasksTable
      tableHeaders={tableData.tableHeaders}
      tableBody={myTasks}
      currentRole={role}
    />
  );
};
