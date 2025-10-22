import { render, screen } from "@testing-library/react";
import { MemoryRouter } from "react-router-dom";
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
      customerName: "User1",
      gatewayCustomerId: "ID001",
      customerConfig: JSON.stringify({
        cisNumbers: ["CIS001"],
        customerType: "Retail",
      }),
      tmccCase: "CASE001",
      status: "APPROVED",
    },
    {
      customerName: "User2",
      gatewayCustomerId: "ID002",
      customerConfig: JSON.stringify({
        cisNumbers: ["CIS002"],
        customerType: "Corporate",
      }),
      tmccCase: "CASE002",
      status: "PENDING_APPROVAL",
    },
    {
      customerName: "User3",
      gatewayCustomerId: "ID003",
      customerConfig: JSON.stringify({
        cisNumbers: ["CIS003"],
        customerType: "SME",
      }),
      tmccCase: "CASE003",
      status: "REJECTED",
    },
  ];

  const renderWithRouter = (ui: React.ReactNode) => {
    return render(<MemoryRouter>{ui}</MemoryRouter>);
  };

  // ✅ test1
  it("test1: renders all table headers", () => {
    renderWithRouter(
      <MyTasksTable
        tableHeaders={tableHeaders}
        tableBody={mockTableBody}
        currentRole="admin"
      />
    );

    tableHeaders.forEach((header) => {
      screen.getByText(header);
    });
  });

  // ✅ test2
  it("test2: filters approved and pending approval for approver role", () => {
    renderWithRouter(
      <MyTasksTable
        tableHeaders={tableHeaders}
        tableBody={mockTableBody}
        currentRole="approver"
      />
    );

    screen.getByText("User1");
    screen.getByText("User2");

    const user3 = screen.queryByText("User3");
    expect(user3).toBeNull();
  });

  // ✅ test3
  it("test3: renders 'No Record Found' for viewer role", () => {
    renderWithRouter(
      <MyTasksTable
        tableHeaders={tableHeaders}
        tableBody={mockTableBody}
        currentRole="viewer"
      />
    );

    screen.getByText(Constants.LABEL.NO_RECORD);
  });

  // ✅ test4
  it("test4: handles invalid JSON in customerConfig", () => {
    const invalidData = [
      {
        ...mockTableBody[0],
        customerConfig: "{invalidJSON}",
      },
    ];

    renderWithRouter(
      <MyTasksTable
        tableHeaders={tableHeaders}
        tableBody={invalidData}
        currentRole="admin"
      />
    );

    screen.getByText("User1"); // renders safely
  });
});
