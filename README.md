// ApiCustomerConfig.test.tsx
import React from "react";
import { render, screen, fireEvent } from "@testing-library/react";
import ApiCustomerConfig from "./ApiCustomerConfig";

describe("ApiCustomerConfig Component - Basic Tests", () => {
  it("renders the main header and CardCheckbox titles", () => {
    render(<ApiCustomerConfig customer={{}} setCustomer={jest.fn()} />);

    // Header
    expect(screen.getByText("Api")).toBeTruthy();

    // CardCheckbox titles
    expect(screen.getByText("General")).toBeTruthy();
    expect(screen.getByText("US RTP")).toBeTruthy();
    expect(screen.getByText("FedNow")).toBeTruthy();
    expect(screen.getByText("Wire")).toBeTruthy();
  });

  it("renders the Select all checkbox", () => {
    render(<ApiCustomerConfig customer={{}} setCustomer={jest.fn()} />);
    const selectAllCheckbox = screen.getByLabelText("Select all") as HTMLInputElement;
    expect(selectAllCheckbox).toBeTruthy();
    expect(selectAllCheckbox.checked).toBe(false);
  });

  it("checks and unchecks Select all checkbox", () => {
    render(<ApiCustomerConfig customer={{}} setCustomer={jest.fn()} />);
    const selectAllCheckbox = screen.getByLabelText("Select all") as HTMLInputElement;

    // Click to select all
    fireEvent.click(selectAllCheckbox);
    expect(selectAllCheckbox.checked).toBe(true);

    // Click again to unselect all
    fireEvent.click(selectAllCheckbox);
    expect(selectAllCheckbox.checked).toBe(false);
  });

  it("renders individual checkboxes inside CardCheckbox components", () => {
    render(<ApiCustomerConfig customer={{}} setCustomer={jest.fn()} />);

    // Check presence of a few known labels
    expect(screen.getByLabelText("Account Balance")).toBeTruthy();
    expect(screen.getByLabelText("Create Credit")).toBeTruthy();
    expect(screen.getByLabelText("Retrieve Status")).toBeTruthy();
    expect(screen.getByLabelText("Webhook Status Update")).toBeTruthy();
  });
});
