// TechnicalContact.test.tsx
import React from "react";
import { render, screen } from "@testing-library/react";
import TechnicalContact from "./TechnicalContact";

describe("TechnicalContact Component", () => {
  const mockCustomer = {};
  const mockSetCustomer = jest.fn();

  it("renders header text", () => {
    render(<TechnicalContact customer={mockCustomer} setCustomer={mockSetCustomer} />);
    const header = screen.getByText(/Technical Contact/i);
    expect(header).toBeTruthy();
  });

  it("renders input placeholders", () => {
    render(<TechnicalContact customer={mockCustomer} setCustomer={mockSetCustomer} />);
    
    expect(screen.getByPlaceholderText("Name")).toBeTruthy();
    expect(screen.getByPlaceholderText("Phone Number")).toBeTruthy();
    expect(screen.getByPlaceholderText("Email")).toBeTruthy();
  });

  it("renders radio buttons for Preferred Method", () => {
    render(<TechnicalContact customer={mockCustomer} setCustomer={mockSetCustomer} />);
    
    expect(screen.getByLabelText("Email")).toBeTruthy();
    expect(screen.getByLabelText("Phone")).toBeTruthy();
  });

  it("renders Preferred Method label", () => {
    render(<TechnicalContact customer={mockCustomer} setCustomer={mockSetCustomer} />);
    const label = screen.getByText(/Preferred Method/i);
    expect(label).toBeTruthy();
  });
});
