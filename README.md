import React from "react";
import { render, screen, fireEvent } from "@testing-library/react";
import RadioGroup from "./RadioGroup";

describe("RadioGroup Component", () => {
  const options = [
    { label: "Email", value: "email" },
    { label: "Phone", value: "phone" },
  ];

  it("renders all radio options", () => {
    render(
      <RadioGroup
        name="contactMethod"
        options={options}
        selectedValue=""
        onChange={jest.fn()}
      />
    );

    expect(screen.getByLabelText("Email")).toBeInTheDocument();
    expect(screen.getByLabelText("Phone")).toBeInTheDocument();
  });

  it("marks the selected value correctly", () => {
    render(
      <RadioGroup
        name="contactMethod"
        options={options}
        selectedValue="phone"
        onChange={jest.fn()}
      />
    );

    expect(screen.getByLabelText("Phone")).toBeChecked();
    expect(screen.getByLabelText("Email")).not.toBeChecked();
  });

  it("calls onChange when a different option is selected", () => {
    const handleChange = jest.fn();
    render(
      <RadioGroup
        name="contactMethod"
        options={options}
        selectedValue="email"
        onChange={handleChange}
      />
    );

    fireEvent.click(screen.getByLabelText("Phone"));

    expect(handleChange).toHaveBeenCalledTimes(1);
    expect(handleChange).toHaveBeenCalledWith("phone");
  });

  it("does not call onChange when disabled", () => {
    const handleChange = jest.fn();
    render(
      <RadioGroup
        name="contactMethod"
        options={options}
        selectedValue="email"
        onChange={handleChange}
        disabled
      />
    );

    const phoneRadio = screen.getByLabelText("Phone");
    expect(phoneRadio).toBeDisabled();

    fireEvent.click(phoneRadio);
    expect(handleChange).not.toHaveBeenCalled();
  });
});
