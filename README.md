// CustomerDetail.test.tsx
import React from "react";
import { render, screen } from "@testing-library/react";
import CustomerDetail from "./CustomerDetail";

describe("CustomerDetail Component", () => {
  const mockCustomer = {};
  const mockSetCustomer = jest.fn();

  it("renders header text", () => {
    render(<CustomerDetail customer={mockCustomer} setCustomer={mockSetCustomer} />);
    expect(screen.getByText(/Customer Info/i)).toBeInTheDocument();
  });

  it("renders all input fields", () => {
    render(<CustomerDetail customer={mockCustomer} setCustomer={mockSetCustomer} />);
    
    expect(screen.getByPlaceholderText("Customer Name")).toBeInTheDocument();
    expect(screen.getByPlaceholderText("CIS Number")).toBeInTheDocument();
    expect(screen.getByPlaceholderText("Billing profile Id")).toBeInTheDocument();
  });

  it("renders select with correct options", () => {
    render(<CustomerDetail customer={mockCustomer} setCustomer={mockSetCustomer} />);
    
    expect(screen.getByText("Commercial")).toBeInTheDocument();
    expect(screen.getByText("Internal")).toBeInTheDocument();
  });

  it("renders radio buttons for Demo Customer", () => {
    render(<CustomerDetail customer={mockCustomer} setCustomer={mockSetCustomer} />);
    
    expect(screen.getByLabelText("Yes")).toBeInTheDocument();
    expect(screen.getByLabelText("No")).toBeInTheDocument();
  });

  it("matches snapshot", () => {
    const { asFragment } = render(<CustomerDetail customer={mockCustomer} setCustomer={mockSetCustomer} />);
    expect(asFragment()).toMatchSnapshot();
  });
});
