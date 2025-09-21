// CustomerDetail.test.tsx
import React from "react";
import { render, screen } from "@testing-library/react";
import CustomerDetail from "./CustomerDetail";

describe("CustomerDetail Component", () => {
  const mockCustomer = {};
  const mockSetCustomer = jest.fn();

  it("renders header text", () => {
    render(<CustomerDetail customer={mockCustomer} setCustomer={mockSetCustomer} />);
    const header = screen.getByText(/Customer Info/i);
    expect(header).toBeTruthy();
  });

  it("renders input placeholders", () => {
    render(<CustomerDetail customer={mockCustomer} setCustomer={mockSetCustomer} />);
    
    expect(screen.getByPlaceholderText("Customer Name")).toBeTruthy();
    expect(screen.getByPlaceholderText("CIS Number")).toBeTruthy();
    expect(screen.getByPlaceholderText("Billing profile Id")).toBeTruthy();
  });

  it("renders select options", () => {
    render(<CustomerDetail customer={mockCustomer} setCustomer={mockSetCustomer} />);
    
    expect(screen.getByText("Commercial")).toBeTruthy();
    expect(screen.getByText("Internal")).toBeTruthy();
  });

  it("renders radio buttons", () => {
    render(<CustomerDetail customer={mockCustomer} setCustomer={mockSetCustomer} />);
    
    expect(screen.getByLabelText("Yes")).toBeTruthy();
    expect(screen.getByLabelText("No")).toBeTruthy();
  });

  it("matches snapshot", () => {
    const { asFragment } = render(<CustomerDetail customer={mockCustomer} setCustomer={mockSetCustomer} />);
    expect(asFragment()).toMatchSnapshot();
  });
});
