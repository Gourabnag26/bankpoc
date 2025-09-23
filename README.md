import React from "react";
import { render, screen } from "@testing-library/react";
import ActionButton from "./action-button";
import { ICustomerData } from "../../customer-profile";
import { MemoryRouter } from "react-router-dom";

describe("ActionButton Component", () => {
     const mockCustomer: ICustomerData = {
        customerName: '',
        cisNumber: 1,
        customerType: 'Commericial',
        billingProfileId: '',
        demoCustomer: 'Yes',
        businessContactName: '',
        businessContactEmail: '',
        businessContactPhone: '',
        businessContactPreferredMethod: 'Email',
        technologyContactName: '',
        technologyContactEmail: '',
        technologyContactPhone: '',
        technologyContactPreferredMethod: 'Email',
        transactionLimit: 0,
        dailyLimit: 0,
        processingWindowStartTime: '00:00',
        processingWindowEndTime: '24:00',
        processingWindowZone: 'GMT',
      };


  it("renders all buttons", () => {
    <MemoryRouter></MemoryRouter>
    render(<ActionButton customer={mockCustomer} setCustomer={() => ({})} disabled={false} />);

    expect(screen.getByText("Save Draft")).toBeTruthy();
    expect(screen.getByText("Save and Exit")).toBeTruthy();
    expect(screen.getByText("Request for Approval")).toBeTruthy();
    expect(screen.getByText("Delete Draft")).toBeTruthy();
  });

  it("renders correct number of buttons", () => {
    render(<ActionButton customer={mockCustomer} setCustomer={() => ({})} disabled={false}  />);
    const buttons = screen.getAllByRole("button");
    expect(buttons.length).toBe(4);
  });
});
