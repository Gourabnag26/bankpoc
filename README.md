// BusinessContact.test.tsx
import React from "react";
import { render, screen } from "@testing-library/react";
import BusinessContact from "./BusinessContact";

describe("BusinessContact Component", () => {
  const mockCustomer = {};
  const mockSetCustomer = jest.fn();

  it("renders header text", () => {
    render(<BusinessContact customer={mockCustomer} setCustomer={mockSetCustomer} />);
    const header = screen.getByText(/Business Contact/i);
    expect(header).toBeTruthy();
  });

  it("renders input placeholders", () => {
    render(<BusinessContact customer={mockCustomer} setCustomer={mockSetCustomer} />);
    
    expect(screen.getByPlaceholderText("Name")).toBeTruthy();
    expect(screen.getByPlaceholderText("Phone Number")).toBeTruthy();
    expect(screen.getByPlaceholderText("Email")).toBeTruthy();
  });

  it("renders radio buttons for Preferred Method", () => {
    render(<BusinessContact customer={mockCustomer} setCustomer={mockSetCustomer} />);
    
    expect(screen.getByLabelText("Email")).toBeTruthy();
    expect(screen.getByLabelText("Phone")).toBeTruthy();
  });

  it("renders Preferred Method label", () => {
    render(<BusinessContact customer={mockCustomer} setCustomer={mockSetCustomer} />);
    const label = screen.getByText(/Preferred Method/i);
    expect(label).toBeTruthy();
  });
});
