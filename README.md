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

  // ✅ Test 1: Renders all table headers
  it("renders all table headers", () => {
    render(
      <MyTasksTable
        tableHeaders={tableHeaders}
        tableBody={mockTableBody}
        currentRole="admin"
      />
    );

    tableHeaders.forEach((header) => {
      expect(screen.getByText(header)).toBeInTheDocument();
    });
  });

  // ✅ Test 2: Filters correctly for approver role
  it("shows only approved and pending approval for approver role", () => {
    render(
      <MyTasksTable
        tableHeaders={tableHeaders}
        tableBody={mockTableBody}
        currentRole="approver"
      />
    );

    expect(screen.getByText("Alice")).toBeInTheDocument(); // approved
    expect(screen.getByText("Bob")).toBeInTheDocument(); // pending approval
    expect(screen.queryByText("Charlie")).not.toBeInTheDocument(); // rejected filtered out
  });

  // ✅ Test 3: Shows 'No Record Found' for viewer
  it("renders No Record Found for viewer", () => {
    render(
      <MyTasksTable
        tableHeaders={tableHeaders}
        tableBody={mockTableBody}
        currentRole="viewer"
      />
    );

    expect(screen.getByText(Constants.LABEL.NO_RECORD)).toBeInTheDocument();
  });

  // ✅ Test 4: Handles invalid JSON gracefully
  it("handles invalid customerConfig JSON without crashing", () => {
    const invalidData = [
      {
        ...mockTableBody[0],
        customerConfig: "{invalidJson}",
      },
    ];

    render(
      <MyTasksTable
        tableHeaders={tableHeaders}
        tableBody={invalidData}
        currentRole="admin"
      />
    );

    expect(screen.getByText("Alice")).toBeInTheDocument(); // still renders
  });
});
