# Peak Magnitude Flows - Timing

#### Definition:

Peak flow timing is the peak magnitude flow component that calculates the start date when a flow event crosses over the flow threshold for the \(2nd, 5th, 10th, and 20th percentile\) exceedance flows over the period of record.

#### Steps:

1. Convert raw data from a single column of dates and a single column of flows into a matrix with columns organized by water year. Each column starts with the beginning of the water year \(i.e. 10/1\) and ends with the end of water year \(9/30\).
2. Calculate the 2nd, 5th, 10th, and 20th percentile exceedance flow values over the entire period of record \(i.e., the entire flow matrix\).
  ```py
  for i in exceedance_percent:
        exceedance_value[i] = np.nanpercentile(matrix, 100 - i)
  ```
3. In each water year, create one object each time the flow passes the exceedance value, and append that object into an array.
  ```py
  """Init current flow object"""
        for percent in exceedance_percent:
            exceedance_object[percent] = []
            exceedance_duration[percent] = []
            current_flow_object[percent] = None

        """Loop through each flow value for the year to check if they pass exceedance threshold"""
        for row_number, flow_row in enumerate(matrix[:, column_number]):

            for percent in exceedance_percent:
                if bool(flow_row < exceedance_value[percent] and current_flow_object[percent]) or bool(row_number == len(matrix[:, column_number]) - 1 and current_flow_object[percent]):
                    """End of an object if it falls below threshold, or end of column"""
                    current_flow_object[percent].end_date = row_number + 1
                    current_flow_object[percent].get_max_magnitude()
                    exceedance_duration[percent].append(current_flow_object[percent].duration)
                    current_flow_object[percent] = None
  ```
4. Find the median timing value from the array of timings for that water year.
  ```py
  for row_number, flow_row in enumerate(matrix[:, column_number]):
    for percent in exceedance_percent:
                  if flow_row < exceedance_value[percent] and current_flow_object[percent] or row_number == len(matrix[:, column_number]) - 1 and current_flow_object[percent]:
                      """End of a object if it falls below threshold, or end of column"""
                    elif flow_row >= exceedance_value[percent]:
                        if not current_flow_object[percent]:
                          timing[percent].append(row_number + 1)
  ```
