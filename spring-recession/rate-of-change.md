# Spring Recession Flows - Rate of Change

#### Definition

The spring recession rate of change calculates the median daily rate of change in flow from the start date of the spring recession until the start of the dry season low flow season. Only days with negative change (i.e. a decreasing rate) are used in the calculation.

#### Steps

1. Calculate the spring recession timing and dry season low flow timing. Rate of change is calculated between these two timing indices.

2. For each water year, check if both the spring recession and dry season timing dates exist, and make sure the spring date occurs before the dry season date:

  ```py
  if not math.isnan(spring_timing) and not math.isnan(summer_timing) and summer_timing > spring_timing:
  ```

3. For each daily time interval, calculate a simple daily rate of change, where change is equal to \(Day2 - Day1\)/Day1. Rate of change is only recorded for those dates in which the daily change is negative.
   ```py
   flow_data = raw_flow[int(spring_timing) : int(summer_timing)]
   elif flow_data[flow_index + 1] < flow_data[flow_index]:
      rate_of_change_neg.append((flow_data[flow_index] - flow_data[flow_index + 1]) / flow_data[flow_index])
   ```
