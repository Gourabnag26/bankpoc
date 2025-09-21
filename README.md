// AccountPopup.test.tsx
import React from "react";
import { render, screen, fireEvent } from "@testing-library/react";
import AccountPopup from "./AccountPopup";

describe("AccountPopup Component", () => {
  it("renders header sections", () => {
    render(<AccountPopup />);
    expect(screen.getByText("Customer Info")).toBeTruthy();
    expect(screen.getByText("Limits")).toBeTruthy();
    expect(screen.getByText("Api")).toBeTruthy();
  });

  it("renders Customer Info inputs", () => {
    render(<AccountPopup />);
    expect(screen.getByPlaceholderText("Customer Name")).toBeTruthy();
    expect(screen.getByPlaceholderText("Account Number")).toBeTruthy();
    expect(screen.getByPlaceholderText("Router Number")).toBeTruthy();
  });

  it("renders Limits inputs", () => {
    render(<AccountPopup />);
    expect(screen.getByPlaceholderText("Transaction Limit")).toBeTruthy();
    expect(screen.getByPlaceholderText("Daily Limit")).toBeTruthy();
  });

  it("handles Select all checkbox correctly", () => {
    render(<AccountPopup />);

    const selectAllCheckbox = screen.getByLabelText("Select all") as HTMLInputElement;
    expect(selectAllCheckbox.checked).toBe(false);

    // Click select all
    fireEvent.click(selectAllCheckbox);

    // After clicking, checkbox should be checked
    expect(selectAllCheckbox.checked).toBe(true);

    // Now verify that "General" CardCheckbox is selected
    const generalOption = screen.getByLabelText("Account Balance") as HTMLInputElement;
    expect(generalOption.checked).toBe(true);

    // US RTP options
    const rtpOption1 = screen.getByLabelText("Create Credit") as HTMLInputElement;
    const rtpOption2 = screen.getByLabelText("Retrieve Status") as HTMLInputElement;
    expect(rtpOption1.checked).toBe(true);
    expect(rtpOption2.checked).toBe(true);

    // FedNow options
    const fednowOption1 = screen.getAllByLabelText("Create Credit")[1] as HTMLInputElement;
    const fednowOption2 = screen.getAllByLabelText("Retrieve Status")[1] as HTMLInputElement;
    expect(fednowOption1.checked).toBe(true);
    expect(fednowOption2.checked).toBe(true);

    // Wire options
    const wireOption1 = screen.getAllByLabelText("Create Credit")[2] as HTMLInputElement;
    const wireOption2 = screen.getAllByLabelText("Retrieve Status")[2] as HTMLInputElement;
    const wireOption3 = screen.getByLabelText("Webhook Status Update") as HTMLInputElement;
    expect(wireOption1.checked).toBe(true);
    expect(wireOption2.checked).toBe(true);
    expect(wireOption3.checked).toBe(true);

    // Click again to unselect all
    fireEvent.click(selectAllCheckbox);
    expect(selectAllCheckbox.checked).toBe(false);
    expect(generalOption.checked).toBe(false);
    expect(rtpOption1.checked).toBe(false);
    expect(rtpOption2.checked).toBe(false);
    expect(fednowOption1.checked).toBe(false);
    expect(fednowOption2.checked).toBe(false);
    expect(wireOption1.checked).toBe(false);
    expect(wireOption2.checked).toBe(false);
    expect(wireOption3.checked).toBe(false);
  });
});
