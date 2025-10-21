import { Status, Table, TableBody, TableCell, TableContainer, TableHead, TableRow, Typography } from '@ucl/ui-components';
import ActionColumn from '../role-icons/role-icons';
import { Constants } from '../../../shared/utils/constants';

const MyTasksTable = ({ tableHeaders, tableBody, currentRole }: any) => {

  const filteredData = tableBody && tableBody.filter((item: any) => currentRole === 'approver' ? item.status === 'Pending Approval' || item.status === 'Approved' : true);
  const
  return (
    <>
      <TableContainer>
        <Table>
          <TableHead>
            <TableRow>
              {tableHeaders && tableHeaders.map((headerName: any, key: any) => (
                <TableCell key={key}>{headerName}</TableCell>
              ))}
            </TableRow>
          </TableHead>
          {
            <TableBody>
              {currentRole !== 'viewer' && filteredData && filteredData?.map((data: any) => (
                <TableRow key={data.gatewayCustomerId}>
                  {data.customerName && (
                    <TableCell>{data.customerName}</TableCell>
                  )}
                  {data.gatewayCustomerId && (
                    <TableCell>{data.gatewayCustomerId}</TableCell>
                  )}
                  {data.customerConfig.cisNumbers[0] && <TableCell>{data.customerConfig.cisNumbers[0]}</TableCell>}
                  {data.customerType && (
                    <TableCell>{data.customerType}</TableCell>
                  )}
                  {data.tmccCase && <TableCell>{data.tmccCase}</TableCell>}
                  {data.status && (
                    <TableCell>
                      <Status
                        variant={'filled'}
                        label={data.status.replaceAll("_"," ")}
                        color={Constants.authStatusColorCodes[data.status]}
                      />
                    </TableCell>
                  )}
                  <TableCell>
                    <ActionColumn
                      role={currentRole}
                      table="My Tasks"
                      status={data.status}
                      id={data.gatewayCustomerId}
                    />
                  </TableCell>
                </TableRow>
              ))}
            </TableBody>
          }
        </Table>
      </TableContainer>
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
