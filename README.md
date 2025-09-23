import React from "react";
import { useSearchParams } from "react-router-dom";
import { IcustomerProps } from "../../customer-profile";
import { Box, Input, Button } from "@ucl/ui-components";
import { useTaskContext } from "../../context/TaskContext"; // adjust path

import "../../customer-profile.css";

const ActionButton = ({ customer, setCustomer }: IcustomerProps) => {
  const [searchParams] = useSearchParams();
  const customerId = searchParams.get("customerId");

  const { myTasks, setMyTasks } = useTaskContext();

  const handleStatusChange = (status: string) => {
    if (!customerId) return;

    const updatedTasks = myTasks.map((task) =>
      task.customerId === customerId ? { ...task, status } : task
    );

    setMyTasks(updatedTasks);
  };

  return (
    <Box className="main-container">
      <Input titleLabel="TMCC Case" sx={{ width: "160px" }} />

      <Button
        sx={{ width: "170px" }}
        className="button"
        variant="primary"
        onClick={() => handleStatusChange("Draft")}
      >
        Save Draft
      </Button>

      <Button
        sx={{ width: "170px" }}
        className="button"
        variant="primary"
        onClick={() => handleStatusChange("Draft")}
      >
        Save and Exit
      </Button>

      <Button
        sx={{ width: "170px" }}
        className="button"
        variant="primary"
        onClick={() => handleStatusChange("Pending Approval")}
      >
        Request for Approval
      </Button>

      <Button
        sx={{ width: "170px" }}
        className="button"
        variant="primary"
        onClick={() => handleStatusChange("Deleted")}
      >
        Delete Draft
      </Button>
    </Box>
  );
};

export default ActionButton;
