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

    expect(screen.getByLabelText("Email")).toBeTruthy();
    expect(screen.getByLabelText("Phone")).toBeTruthy();
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

    const phoneRadio = screen.getByLabelText("Phone") as HTMLInputElement;
    const emailRadio = screen.getByLabelText("Email") as HTMLInputElement;

    expect(phoneRadio.checked).toBe(true);
    expect(emailRadio.checked).toBe(false);
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

    const phoneRadio = screen.getByLabelText("Phone") as HTMLInputElement;
    expect(phoneRadio.disabled).toBe(true);

    fireEvent.click(phoneRadio);
    expect(handleChange).toHaveBeenCalledTimes(0);
  });
});
