import {
  Status,
  Table,
  TableBody,
  TableCell,
  TableContainer,
  TableHead,
  TableRow,
  Typography,
} from '@ucl/ui-components';
import ActionColumn from '../role-icons/role-icons';
import { Constants } from '../../../shared/utils/constants';

const MyTasksTable = ({ tableHeaders, tableBody, currentRole }: any) => {
  //approver filter
  const filteredData =
    tableBody &&
    tableBody.filter((item: any) => {
      const status = item.status?.toLowerCase().replace(/_/g, ' ');
      if (currentRole === 'approver') {
        return status === 'pending approval' || status === 'approved';
      }
      return true;
    });

  return (
    <>
      <TableContainer>
        <Table>
          <TableHead>
            <TableRow>
              {tableHeaders &&
                tableHeaders.map((headerName: any, key: any) => (
                  <TableCell key={key}>{headerName}</TableCell>
                ))}
            </TableRow>
          </TableHead>

          <TableBody>
            {currentRole !== 'viewer' &&
              filteredData &&
              filteredData.map((data: any) => {
                let customerConfig: any = {};
                try {
                  customerConfig =
                    typeof data.customerConfig === 'string'
                      ? JSON.parse(data.customerConfig)
                      : data.customerConfig;
                } catch (err) {
                  console.warn('Invalid customerConfig JSON:', err);
                  customerConfig = {};
                }

                const statusLabel =
                  data.status?.replaceAll('_', ' ') ?? 'Unknown';
                const statusColor =
                  Constants.authStatusColorCodes[data.status] ?? 'default';

                return (
                  <TableRow key={data.gatewayCustomerId}>
                    <TableCell>{data.customerName || '-'}</TableCell>
                    <TableCell>{data.gatewayCustomerId || '-'}</TableCell>
                    <TableCell>
                      {customerConfig.cisNumbers?.[0] || '-'}
                    </TableCell>
                    <TableCell>{customerConfig.customerType || '-'}</TableCell>
                    <TableCell>{data.tmccCase || '-'}</TableCell>
                    <TableCell>
                      <Status
                        variant="filled"
                        label={statusLabel}
                        color={statusColor}
                      />
                    </TableCell>
                    <TableCell>
                      <ActionColumn
                        role={currentRole}
                        table="My Tasks"
                        status={data.status.toLowerCase().replaceAll("_"," ")}
                        id={data.gatewayCustomerId}
                      />
                    </TableCell>
                  </TableRow>
                );
              })}
          </TableBody>
        </Table>
      </TableContainer>

      {/* Show "No Record" for viewer role */}
      {currentRole === 'viewer' && (
        <Typography
          variant="body1"
          sx={{
            display: 'flex',
            height: '50vh',
            alignItems: 'center',
            justifyContent: 'center',
          }}
        >
          {Constants.LABEL.NO_RECORD}
        </Typography>
      )}
    </>
  );
};

export default MyTasksTable;
