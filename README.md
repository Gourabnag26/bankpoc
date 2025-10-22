import { render, screen } from "@testing-library/react";
import MyTasksTable from "./MyTasksTable";
import { Constants } from "../../../shared/utils/constants";

// Mock components from external libraries
jest.mock("@ucl/ui-components", () => ({
  Status: ({ label }: any) => <div data-testid="status">{label}</div>,
  Table: ({ children }: any) => <table>{children}</table>,
  TableBody: ({ children }: any) => <tbody>{children}</tbody>,
  TableCell: ({ children }: any) => <td>{children}</td>,
  TableContainer: ({ children }: any) => <div>{children}</div>,
  TableHead: ({ children }: any) => <thead>{children}</thead>,
  TableRow: ({ children }: any) => <tr>{children}</tr>,
  Typography: ({ children }: any) => <div data-testid="typography">{children}</div>,
}));

jest.mock("../role-icons/role-icons", () => ({
  __esModule: true,
  default: ({ role, id }: any) => <div data-testid="action-column">{`${role}-${id}`}</div>,
}));

Constants.LABEL = { NO_RECORD: "No Record Found" };
Constants.authStatusColorCodes = { approved: "green", pending_approval: "yellow" };

describe("MyTasksTable Component", () => {
  const tableHeaders = ["Customer Name", "Customer ID", "Status"];

  const mockData = [
    {
      customerName: "Alice",
      gatewayCustomerId: "CUST001",
      customerConfig: JSON.stringify({ cisNumbers: ["CIS001"], customerType: "TypeA" }),
      status: "approved",
      tmccCase: "CASE123",
    },
    {
      customerName: "Bob",
      gatewayCustomerId: "CUST002",
      customerConfig: JSON.stringify({ cisNumbers: ["CIS002"], customerType: "TypeB" }),
      status: "pending_approval",
      tmccCase: "CASE999",
    },
    {
      customerName: "Charlie",
      gatewayCustomerId: "CUST003",
      customerConfig: JSON.stringify({ cisNumbers: ["CIS003"], customerType: "TypeC" }),
      status: "rejected",
      tmccCase: "CASE456",
    },
  ];

  it("renders all table headers", () => {
    render(<MyTasksTable tableHeaders={tableHeaders} tableBody={mockData} currentRole="admin" />);
    tableHeaders.forEach((header) => {
      expect(screen.getByText(header)).toBeInTheDocument();
    });
  });

  it("shows filtered data for approver role (only pending approval or approved)", () => {
    render(<MyTasksTable tableHeaders={tableHeaders} tableBody={mockData} currentRole="approver" />);
    expect(screen.getByText("Alice")).toBeInTheDocument();
    expect(screen.getByText("Bob")).toBeInTheDocument();
    expect(screen.queryByText("Charlie")).not.toBeInTheDocument(); // rejected should be filtered out
  });

  it("renders ActionColumn for each data row when not viewer", () => {
    render(<MyTasksTable tableHeaders={tableHeaders} tableBody={mockData} currentRole="admin" />);
    const actionColumns = screen.getAllByTestId("action-column");
    expect(actionColumns.length).toBe(mockData.length);
  });

  it("renders 'No Record Found' text for viewer role", () => {
    render(<MyTasksTable tableHeaders={tableHeaders} tableBody={mockData} currentRole="viewer" />);
    expect(screen.getByTestId("typography")).toHaveTextContent("No Record Found");
  });

  it("renders Status component with formatted label", () => {
    render(<MyTasksTable tableHeaders={tableHeaders} tableBody={[mockData[0]]} currentRole="admin" />);
    expect(screen.getByTestId("status")).toHaveTextContent("approved");
  });

  it("handles invalid JSON in customerConfig gracefully", () => {
    const invalidData = [{ ...mockData[0], customerConfig: "{invalidJson}" }];
    render(<MyTasksTable tableHeaders={tableHeaders} tableBody={invalidData} currentRole="admin" />);
    expect(screen.getByText("Alice")).toBeInTheDocument(); // still renders row
  });
});
