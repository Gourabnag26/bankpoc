export interface Task {
  taskId: number;
  gatewayCustomerId: string;
  status: "DRAFT" | "PENDING_APPROVAL" | "APPROVED" | string; // string fallback for future statuses
  customerName: string;
  customerConfig: CustomerConfig | string; // Some APIs return as JSON string
  createdBy: string;
  updatedBy: string;
  approvedBy?: string | null;
}
