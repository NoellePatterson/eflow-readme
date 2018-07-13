# Peak Magnitude Flows - Magnitude

#### Definition:

This metric calculates the magnitude of flows that exceed the 2nd, 5th, 10th, and 20th percentile exceedance flows over the period of record.

#### Steps:

1. Convert raw data of from a single column of dates and a single column of flows into a matrix with columns organized by water year. Each column starts with the beginning of the water year \(i.e. 10/1\) and ends with the end of water year \(9/30\).
2. Calculate the 2nd, 5th, 10th, and 20th percentile exceedance flow values over the entire period of record \(i.e., the entire flow matrix\).
3. For each event in which the exceedance threshold is crossed, calculate the maximum flow of that event.

#### Code Snippet:

Given exceedance values based on each percentile, calculate maximum flow values of each flow object that crosses the exceedance threshold.

```py
if flow_row < exceedance_value[percent] and current_flow_object[percent] or row_number == len(matrix[:, column_number]) - 1 and current_flow_object[percent]:
                   """End of a object if it falls below threshold, or end of column"""
                   magnitude[percent].append(max(current_flow_object[percent].flow) / average_annual_flow)
```
