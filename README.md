import { useEffect, useState } from "react";
import { useCustomer } from "../../context/customer-context";
import MyTasksTable from "../../components/my-tasks-table/my-tasks-table";
import MyTasksTableData from "../../../shared/dummy-json/my-tasks.json";
import { useRole } from "../../../shared/utils/role-manager";
import DataLoader from "../../components/data-loader/data-loader";
import { CustomerService } from "shared";

export const MyTasks = () => {
  const [tableData] = useState(MyTasksTableData);
  const role = useRole();
  const { myTasks, setMyTasks } = useCustomer();
  const [loading, setLoading] = useState(true);
  const myTaskApi = new CustomerService();

  useEffect(() => {
    const fetchTasks = async () => {
      try {
        setLoading(true);

        // ✅ Actual API call
        const response = await myTaskApi.getMyTasks(); 
        // (Adjust method name/params as per your backend implementation)

        if (response?.data) {
          // Assuming API returns { draftList: [], completedList: [], approveList: [] }
          const { draftList = [], completedList = [], approveList = [] } = response.data;

          const combinedList = [
            ...draftList,
            ...completedList,
            ...approveList,
          ];

          setMyTasks(combinedList);
        } else {
          setMyTasks([]);
        }
      } catch (error) {
        console.error("Error fetching tasks:", error);
        setMyTasks([]);
      } finally {
        setLoading(false);
      }
    };

    fetchTasks();

    // ❌ Commenting the dummy simulation
    /*
    const fetchDummyTasks = () => {
      setTimeout(() => {
        const dummyApiResponse = { ... }; // old dummy data
        const combinedList = [
          ...dummyApiResponse.draftList,
          ...dummyApiResponse.completedList,
          ...dummyApiResponse.approveList,
        ];
        setMyTasks(combinedList);
        setLoading(false);
      }, 1500);
    };
    fetchDummyTasks();
    */
  }, [setMyTasks, myTaskApi]);

  return (
    <DataLoader loading={loading}>
      <MyTasksTable
        tableHeaders={tableData.tableHeaders}
        tableBody={myTasks}
        currentRole={role}
      />
    </DataLoader>
  );
};
