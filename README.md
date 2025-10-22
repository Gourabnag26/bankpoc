
import { render, screen } from "@testing-library/react";
import { MemoryRouter } from "react-router-dom";
import MyTasksTable from './my-tasks-table';

describe('CustomTable Component', () => {

    const tableHeaders: any = [
        "Customer Name",
        "Gateway Customer ID",
        "CIS Number",
        "Customer Type",
        "TMCC Case",
        "Status",
        "Actions"
    ];
    const tableBody: any = [{
        "customerName":"Column Bank",
        "gatewayCustomerId":"6a4ea94-b019-4a59-94b4-54291f72ccfc1",
        "cisNumber": "1234567890",
        "customerType":"Commercial",
        "tmccCase": "34535",
        "status" : "Pending Approval"
    }];
    const authRoles = {
        "viewer": "view",
        "creator": "edit",
        "editor": "edit",
        "approver": "approve"
    };

    it('renders MyTasksTable Component successfully for approver', () => {
        const { baseElement }: any = render(
            <MemoryRouter>
                <MyTasksTable tableHeaders={tableHeaders} tableBody={tableBody}  authRoles={authRoles['approver']} currentRole='approve' />;
            </MemoryRouter>);
        expect(baseElement).toBeTruthy();
    
    });

    it('renders MyTasksTable all expected table headers for approver', () => {
        render(
            <MemoryRouter>
                <MyTasksTable tableHeaders={tableHeaders} tableBody={[]}  authRoles={authRoles['approver']} currentRole='approve' />;
            </MemoryRouter>);

        tableHeaders.forEach((header: any) => {
            expect(screen.getByText(header)).toBeTruthy();
        });
    });

    it('renders MyTasksTable all expected table body for approver', () => {
        render(
            <MemoryRouter>
                <MyTasksTable tableHeaders={[]} tableBody={tableBody}  authRoles={authRoles['approver']} currentRole='approve' />;
            </MemoryRouter>);

        tableBody.forEach((row: any) => {
            Object.values(row).forEach((cellValue: any) => {
                expect(screen.getByText(cellValue)).toBeTruthy();
            });
        });
    });

    it('renders MyTasksTable all expected table header and body for approver', () => {
        render(
            <MemoryRouter>
                <MyTasksTable tableHeaders={tableHeaders} tableBody={tableBody}  authRoles={authRoles['approver']} currentRole='approve' />;
            </MemoryRouter>);

        tableHeaders.forEach((header: any) => {
            expect(screen.getByText(header)).toBeTruthy();
        });

        tableBody.forEach((row: any) => {
            Object.values(row).forEach((cellValue: any) => {
                expect(screen.getByText(cellValue)).toBeTruthy();
            });
        });
    });


    it('renders MyTasksTable Component successfully for creator', () => {
        const { baseElement }: any = render(
            <MemoryRouter>
                <MyTasksTable tableHeaders={tableHeaders} tableBody={tableBody}  authRoles={authRoles['creator']} currentRole='edit' />;
            </MemoryRouter>);
        expect(baseElement).toBeTruthy();
    
    });

    it('renders MyTasksTable all expected table headers for creator', () => {
        render(
            <MemoryRouter>
                <MyTasksTable tableHeaders={tableHeaders} tableBody={[]}  authRoles={authRoles['creator']} currentRole='edit' />;
            </MemoryRouter>);

        tableHeaders.forEach((header: any) => {
            expect(screen.getByText(header)).toBeTruthy();
        });
    });

    it('renders MyTasksTable all expected table body for creator', () => {
        render(
            <MemoryRouter>
                <MyTasksTable tableHeaders={[]} tableBody={tableBody}  authRoles={authRoles['creator']} currentRole='edit' />;
            </MemoryRouter>);

        tableBody.forEach((row: any) => {
            Object.values(row).forEach((cellValue: any) => {
                expect(screen.getByText(cellValue)).toBeTruthy();
            });
        });
    });

    it('renders MyTasksTable all expected table header and body for creator', () => {
        render(
            <MemoryRouter>
                <MyTasksTable tableHeaders={tableHeaders} tableBody={tableBody}  authRoles={authRoles['creator']} currentRole='edit' />;
            </MemoryRouter>);

        tableHeaders.forEach((header: any) => {
            expect(screen.getByText(header)).toBeTruthy();
        });

        tableBody.forEach((row: any) => {
            Object.values(row).forEach((cellValue: any) => {
                expect(screen.getByText(cellValue)).toBeTruthy();
            });
        });
    });

    it('renders MyTasksTable Component successfully for editor', () => {
        const { baseElement }: any = render(
            <MemoryRouter>
                <MyTasksTable tableHeaders={tableHeaders} tableBody={tableBody}  authRoles={authRoles['editor']} currentRole='edit' />;
            </MemoryRouter>);
        expect(baseElement).toBeTruthy();
    });

    it('renders MyTasksTable all expected table headers for editor', () => {
        render(
            <MemoryRouter>
                <MyTasksTable tableHeaders={tableHeaders} tableBody={[]}  authRoles={authRoles['editor']} currentRole='edit' />;
            </MemoryRouter>);

        tableHeaders.forEach((header: any) => {
            expect(screen.getByText(header)).toBeTruthy();
        });
    });

    it('renders MyTasksTable all expected table body for editor', () => {
        render(
            <MemoryRouter>
                <MyTasksTable tableHeaders={[]} tableBody={tableBody}  authRoles={authRoles['editor']} currentRole='edit' />;
            </MemoryRouter>);

        tableBody.forEach((row: any) => {
            Object.values(row).forEach((cellValue: any) => {
                expect(screen.getByText(cellValue)).toBeTruthy();
            });
        });
    });

    it('renders MyTasksTable all expected table header and body for editor', () => {
        render(
            <MemoryRouter>
                <MyTasksTable tableHeaders={tableHeaders} tableBody={tableBody}  authRoles={authRoles['editor']} currentRole='edit' />;
            </MemoryRouter>);

        tableHeaders.forEach((header: any) => {
            expect(screen.getByText(header)).toBeTruthy();
        });

        tableBody.forEach((row: any) => {
            Object.values(row).forEach((cellValue: any) => {
                expect(screen.getByText(cellValue)).toBeTruthy();
            });
        });
    });

    it('renders MyTasksTable Component successfully for viewer', () => {
        const { baseElement }: any = render(
            <MemoryRouter>
                <MyTasksTable tableHeaders={tableHeaders} tableBody={tableBody}  authRoles={authRoles['viewer']} currentRole='view' />;
            </MemoryRouter>);
        expect(baseElement).toBeTruthy();
    });

    it('renders MyTasksTable all expected table headers for viewer', () => {
        render(
            <MemoryRouter>
                <MyTasksTable tableHeaders={tableHeaders} tableBody={[]}  authRoles={authRoles['viewer']} currentRole='view' />;
            </MemoryRouter>);

        tableHeaders.forEach((header: any) => {
            expect(screen.getByText(header)).toBeTruthy();
        });
    });

});
