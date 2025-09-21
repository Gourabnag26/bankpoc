// TransactionLimit.test.tsx
import React from "react";
import { render, screen } from "@testing-library/react";
import TransactionLimit from "./TransactionLimit";

describe("TransactionLimit Component", () => {
  const mockCustomer = {};
  const mockSetCustomer = jest.fn();

  it("renders header text", () => {
    render(<TransactionLimit customer={mockCustomer} setCustomer={mockSetCustomer} />);
    expect(screen.getByText("Limits")).toBeTruthy();
  });

  it("renders input placeholders", () => {
    render(<TransactionLimit customer={mockCustomer} setCustomer={mockSetCustomer} />);
    
    expect(screen.getByPlaceholderText("Transaction Limit")).toBeTruthy();
    expect(screen.getByPlaceholderText("Daily Limit")).toBeTruthy();
  });

  it("renders processing window section", () => {
    render(<TransactionLimit customer={mockCustomer} setCustomer={mockSetCustomer} />);
    
    expect(screen.getByText("Processing Window")).toBeTruthy();
  });

  it("renders timezone options", () => {
    render(<TransactionLimit customer={mockCustomer} setCustomer={mockSetCustomer} />);
    
    expect(screen.getByText("GMT")).toBeTruthy();
    expect(screen.getByText("UTC")).toBeTruthy();
    expect(screen.getByText("IST")).toBeTruthy();
    expect(screen.getByText("CST")).toBeTruthy();
  });
});
