// ActionButton.test.tsx
import React from "react";
import { render, screen } from "@testing-library/react";
import ActionButton from "./ActionButton";

describe("ActionButton Component", () => {
  it("renders input field", () => {
    render(<ActionButton customer={{}} setCustomer={() => {}} />);
    const input = screen.getByLabelText("TMCC Case");
    expect(input).toBeTruthy();
  });

  it("renders all buttons", () => {
    render(<ActionButton customer={{}} setCustomer={() => {}} />);

    expect(screen.getByText("Save Draft")).toBeTruthy();
    expect(screen.getByText("Save and Exit")).toBeTruthy();
    expect(screen.getByText("Request for Approval")).toBeTruthy();
    expect(screen.getByText("Cancel")).toBeTruthy();
  });

  it("renders correct number of buttons", () => {
    render(<ActionButton customer={{}} setCustomer={() => {}} />);
    const buttons = screen.getAllByRole("button");
    expect(buttons.length).toBe(4);
  });
});
