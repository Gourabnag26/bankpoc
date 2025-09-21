// AccountPopup.test.tsx
import React from "react";
import { render, screen, fireEvent } from "@testing-library/react";
import AccountPopup from "./AccountPopup";

describe("AccountPopup Component with mock data", () => {
  it("renders all sections with mock data", () => {
    render(<AccountPopup />);

    // Header sections
    expect(screen.getByText("Customer Info")).toBeTruthy();
    expect(screen.getByText("Limits")).toBeTruthy();
    expect(screen.getByText("Api")).toBeTruthy();

    // Customer Info inputs
    expect(screen.getByPlaceholderText("Customer Name")).toBeTruthy();
    expect(screen.getByPlaceholderText("Account Number")).toBeTruthy();
    expect(screen.getByPlaceholderText("Router Number")).toBeTruthy();

    // Limits inputs
    expect(screen.getByPlaceholderText("Transaction Limit")).toBeTruthy();
    expect(screen.getByPlaceholderText("Daily Limit")).toBeTruthy();
  });

  it("handles Select all checkbox correctly with mock data", () => {
    render(<AccountPopup />);

    // Select all checkbox
    const selectAllCheckbox = screen.getByLabelText("Select all") as HTMLInputElement;
    expect(selectAllCheckbox.checked).toBe(false);

    // Click select all
    fireEvent.click(selectAllCheckbox);
    expect(selectAllCheckbox.checked).toBe(true);

    // All CardCheckbox options should be checked
    const allLabels = [
      "Account Balance",
      "Create Credit", 
      "Retrieve Status",
      "Create Credit",
      "Retrieve Status",
      "Create Credit",
      "Retrieve Status",
      "Webhook Status Update",
    ];

    allLabels.forEach((label) => {
      const checkbox = screen.getByLabelText(label) as HTMLInputElement;
      expect(checkbox.checked).toBe(true);
    });

    // Click again to unselect all
    fireEvent.click(selectAllCheckbox);
    expect(selectAllCheckbox.checked).toBe(false);

    allLabels.forEach((label) => {
      const checkbox = screen.getByLabelText(label) as HTMLInputElement;
      expect(checkbox.checked).toBe(false);
    });
  });

  it("updates individual CardCheckbox selections", () => {
    render(<AccountPopup />);

    const generalCheckbox = screen.getByLabelText("Account Balance") as HTMLInputElement;
    expect(generalCheckbox.checked).toBe(false);

    // Click individual checkbox
    fireEvent.click(generalCheckbox);
    expect(generalCheckbox.checked).toBe(true);

    // Select all should remain false
    const selectAllCheckbox = screen.getByLabelText("Select all") as HTMLInputElement;
    expect(selectAllCheckbox.checked).toBe(false);
  });
});
