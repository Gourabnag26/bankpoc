import { render, screen } from "@testing-library/react";
import MyTasksTable from "./MyTasksTable";
import { Constants } from "../../../shared/utils/constants";

Constants.LABEL = { NO_RECORD: "No Record Found" };
Constants.authStatusColorCodes = {
  approved: "green",
  pending_approval: "yellow",
  rejected: "red",
};

describe("MyTasksTable Component", () => {
  const tableHeaders = [
    "Customer Name",
    "Customer ID",
    "CIS",
    "Type",
    "Case",
    "Status",
    "Actions",
  ];

  const mockTableBody = [
    {
      customerName: "Alice",
      gatewayCustomerId: "G001",
      customerConfig: JSON.stringify({
        cisNumbers: ["CIS123"],
        customerType: "Retail",
      }),
      tmccCase: "TMCC001",
      status: "APPROVED",
    },
    {
      customerName: "Bob",
      gatewayCustomerId: "G002",
      customerConfig: JSON.stringify({
        cisNumbers: ["CIS456"],
        customerType: "Corporate",
      }),
      tmccCase: "TMCC002",
      status: "PENDING_APPROVAL",
    },
    {
      customerName: "Charlie",
      gatewayCustomerId: "G003",
      customerConfig: JSON.stringify({
        cisNumbers: ["CIS789"],
        customerType: "SME",
      }),
      tmccCase: "TMCC003",
      status: "REJECTED",
    },
  ];

  // ✅ Test 1: Renders all headers
  it("renders all table headers", () => {
    render(
      <MyTasksTable
        tableHeaders={tableHeaders}
        tableBody={mockTableBody}
        currentRole="admin"
      />
    );

    tableHeaders.forEach((header) => {
      // if getByText doesn't throw, header exists
      screen.getByText(header);
    });
  });

  // ✅ Test 2: Approver role filter
  it("shows only approved and pending approval for approver role", () => {
    render(
      <MyTasksTable
        tableHeaders={tableHeaders}
        tableBody={mockTableBody}
        currentRole="approver"
      />
    );

    screen.getByText("Alice"); // approved
    screen.getByText("Bob"); // pending approval

    const charlie = screen.queryByText("Charlie");
    expect(charlie).toBeNull(); // rejected should not be shown
  });

  // ✅ Test 3: Viewer role message
  it("renders No Record Found for viewer role", () => {
    render(
      <MyTasksTable
        tableHeaders={tableHeaders}
        tableBody={mockTableBody}
        currentRole="viewer"
      />
    );

    screen.getByText(Constants.LABEL.NO_RECORD);
  });

  // ✅ Test 4: Invalid JSON safety
  it("handles invalid customerConfig JSON safely", () => {
    const invalidData = [
      {
        ...mockTableBody[0],
        customerConfig: "{invalidJSON}",
      },
    ];

    render(
      <MyTasksTable
        tableHeaders={tableHeaders}
        tableBody={invalidData}
        currentRole="admin"
      />
    );

    screen.getByText("Alice"); // still renders safely
  });
});
