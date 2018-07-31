# Dry Season Low Flows - Duration

#### Description

The dry season low flow duration is calculated as start of the dry season baseflow until the next year's start of the wet season. This metric is measured in number of days.

#### Steps

1. The timing of the start of the dry season low flow period and the start of the wet season must first be calculated, and then passed into the calculation for the dry season low flows duration.
  ```py
  def calc_summer_baseflow_durations_magnitude(flow_matrix, summer_start_dates, fall_flush_dates, fall_flush_wet_dates):
  ```
2. Identify the range of flow data corresponding to the start of the dry season low flow period until the start of the wet season.
  ```py
  flow_data_wet = list(flow_matrix[su_date:,column_number]) + list(flow_matrix[:wet_date, column_number])
  ```
3. Calculate the dry season low flow duration as the number of days in the range from the start of the dry season low flow period until the start of the wet season, identified in step 3.
  ```py
  summer_wet_durations.append(len(flow_data_wet))
  ```
