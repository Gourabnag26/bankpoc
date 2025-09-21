// TimeInput.test.tsx
import React from "react";
import { render, screen, fireEvent } from "@testing-library/react";
import TimeInput from "./TimeInput";

describe("TimeInput Component", () => {
  it("renders with initial time", () => {
    const mockSetTime = jest.fn();
    render(<TimeInput time="10:30" setTime={mockSetTime} />);
    const input = screen.getByDisplayValue("10:30");
    expect(input).toBeTruthy();
  });

  it("formats time on blur", () => {
    const mockSetTime = jest.fn();
    render(<TimeInput time="9:5" setTime={mockSetTime} />);
    const input = screen.getByDisplayValue("9:5");

    fireEvent.blur(input);
    expect(mockSetTime).toHaveBeenCalledWith("09:05");
  });

  it("adds colon after two digits", () => {
    const mockSetTime = jest.fn();
    render(<TimeInput time="" setTime={mockSetTime} />);
    const input = screen.getByRole("textbox");

    fireEvent.change(input, { target: { value: "12" } });
    expect(mockSetTime).toHaveBeenCalledWith("12:");
  });

  it("does not allow invalid hours", () => {
    const mockSetTime = jest.fn();
    render(<TimeInput time="" setTime={mockSetTime} />);
    const input = screen.getByRole("textbox");

    fireEvent.change(input, { target: { value: "25:00" } });
    expect(mockSetTime).not.toHaveBeenCalled();
  });

  it("does not allow invalid minutes", () => {
    const mockSetTime = jest.fn();
    render(<TimeInput time="" setTime={mockSetTime} />);
    const input = screen.getByRole("textbox");

    fireEvent.change(input, { target: { value: "12:75" } });
    expect(mockSetTime).not.toHaveBeenCalled();
  });
});
