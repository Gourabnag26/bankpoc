// CardCheckbox.test.tsx
import React from "react";
import { render, screen, fireEvent } from "@testing-library/react";
import CardCheckbox from "./CardCheckbox";

describe("CardCheckbox Component", () => {
  const mockOnChange = jest.fn();
  const checkboxes = [
    { id: "1", label: "Option 1", value: "opt1" },
    { id: "2", label: "Option 2", value: "opt2" },
  ];

  beforeEach(() => {
    jest.clearAllMocks();
  });

  it("renders the title", () => {
    render(
      <CardCheckbox
        title="Select Options"
        checkboxes={checkboxes}
        selectedValues={[]}
        onChange={mockOnChange}
      />
    );

    expect(screen.getByText("Select Options")).toBeTruthy();
  });

  it("renders all checkboxes with labels", () => {
    render(
      <CardCheckbox
        title="Select Options"
        checkboxes={checkboxes}
        selectedValues={[]}
        onChange={mockOnChange}
      />
    );

    expect(screen.getByLabelText("Option 1")).toBeTruthy();
    expect(screen.getByLabelText("Option 2")).toBeTruthy();
  });

  it("marks selected checkboxes as checked", () => {
    render(
      <CardCheckbox
        title="Select Options"
        checkboxes={checkboxes}
        selectedValues={["1"]}
        onChange={mockOnChange}
      />
    );

    const option1 = screen.getByLabelText("Option 1") as HTMLInputElement;
    const option2 = screen.getByLabelText("Option 2") as HTMLInputElement;

    expect(option1.checked).toBe(true);
    expect(option2.checked).toBe(false);
  });

  it("calls onChange with updated values when checkbox is clicked", () => {
    render(
      <CardCheckbox
        title="Select Options"
        checkboxes={checkboxes}
        selectedValues={[]}
        onChange={mockOnChange}
      />
    );

    const option1 = screen.getByLabelText("Option 1");
    fireEvent.click(option1);

    expect(mockOnChange).toHaveBeenCalledWith(["1"]);
  });

  it("removes checkbox id when unchecked", () => {
    render(
      <CardCheckbox
        title="Select Options"
        checkboxes={checkboxes}
        selectedValues={["1"]}
        onChange={mockOnChange}
      />
    );

    const option1 = screen.getByLabelText("Option 1");
    fireEvent.click(option1);

    expect(mockOnChange).toHaveBeenCalledWith([]);
  });
});
