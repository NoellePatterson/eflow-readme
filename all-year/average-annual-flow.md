# Average Annual Flow

#### Definition

The arithmetic mean of daily flow is calculated individually for each water year. This metric is in units of cfs. 

#### Steps

1. Convert raw date and flow data columns into a flow matrix based on water year. Each column starts with the beginning of the water year \(i.e. 10/1\) and ends with the end of the water year \(i.e.9/30\).

2. Calculate the average annual flow for each water year:
  ```py
  average_annual_flows.append(np.nanmean(flow_matrix[:, index]))
  ```
