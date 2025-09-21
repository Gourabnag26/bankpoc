// AccountSearchTable.test.tsx
import React from "react";
import { render, screen, fireEvent } from "@testing-library/react";
import AccountSearchTable from "./account-table";

describe("AccountSearchTable Component", () => {
  it("renders input fields and search button", () => {
    render(<AccountSearchTable />);
    expect(screen.getByLabelText(/Account Name/i)).toBeTruthy();
    expect(screen.getByLabelText(/Account Number/i)).toBeTruthy();
    expect(screen.getByText(/Search/i)).toBeTruthy();
  });

  it("renders table headers", () => {
    render(<AccountSearchTable />);
    expect(screen.getByText(/Account Name/i)).toBeTruthy();
    expect(screen.getByText(/Account  Number/i)).toBeTruthy();
    expect(screen.getByText(/Routing Number/i)).toBeTruthy();
    expect(screen.getByText(/Action/i)).toBeTruthy();
  });

  it("renders table rows", () => {
    render(<AccountSearchTable />);
    expect(screen.getByText(/Checking Account one/i)).toBeTruthy();
    expect(screen.getByText(/12231/i)).toBeTruthy();
    expect(screen.getByText(/1244/i)).toBeTruthy();
  });

  it("filters data by account name", () => {
    render(<AccountSearchTable />);
    const accountNameInput = screen.getByLabelText(/Account Name/i);
    const searchButton = screen.getByText(/Search/i);

    fireEvent.change(accountNameInput, { target: { value: "two" } });
    fireEvent.click(searchButton);

    expect(screen.getByText(/Checking Account two/i)).toBeTruthy();
    expect(screen.queryByText(/Checking Account one/i)).toBeNull();
  });

  it("filters data by account number", () => {
    render(<AccountSearchTable />);
    const accountNumberInput = screen.getByLabelText(/Account Number/i);
    const searchButton = screen.getByText(/Search/i);

    fireEvent.change(accountNumberInput, { target: { value: "12234" } });
    fireEvent.click(searchButton);

    expect(screen.getByText(/Checking Account four/i)).toBeTruthy();
    expect(screen.queryByText(/Checking Account one/i)).toBeNull();
  });

  it("renders action icons for each row", () => {
    render(<AccountSearchTable />);
    const showIcons = screen.getAllByTestId("ShowIcon");
    const editIcons = screen.getAllByTestId("EditIcon");
    const deleteIcons = screen.getAllByTestId("DeleteIcon");

    expect(showIcons.length).toBeGreaterThan(0);
    expect(editIcons.length).toBeGreaterThan(0);
    expect(deleteIcons.length).toBeGreaterThan(0);
  });
});
