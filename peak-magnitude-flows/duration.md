# Peak Magnitude Flows - Duration

#### Definition

Peak flow duration is the peak magnitude flow component that calculates the median number of days that a flow event stays over the flow thresholds for the 2nd, 5th, 10th, and 20th percentile exceedance flows over the period of record. This metric is measured in number of days. 

#### Steps

1. Convert raw data of from a single column of dates and a single column of flows into a matrix with columns organized by water year. Each column starts with the beginning of the water year \(i.e. 10/1\) and ends with the end of water year \(9/30\).
2. Calculate the 2nd, 5th, 10th, and 20th percentile exceedance flow values over the entire period of record \(i.e., the entire flow matrix\).
  ```py
  for i in exceedance_percent:
        exceedance_value[i] = np.nanpercentile(matrix, 100 - i)
  ```
3. In each water year, create one object each time the flow passes the exceedance percentile threshold value.
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
4. For each day flow value is over the exceedance value, the object adds the flow value to itself using the `add_flow` method. Duration is then calculated by finding the length of `flow` attribute.
  ```py
  class FlowExceedance:

      def __init__(self, start_date, end_date, duration, exceedance):
          self.start_date = start_date
          self.end_date = end_date
          self.duration = duration
          self.flow = []
          self.exceedance = exceedance

      def add_flow(self, flow_data):
          self.flow.append(flow_data)
  ```
