# Peak Magnitude Flows - Timing

#### Definition:

High flow timing is the start date when flow crosses over the exceedance percentile threshold during a water year. For each water year, record the timing of each event in which the flow crosses over the exceedance flow threshold. This results in an array of timing values for each water year. The final timing metric for each water year is the median of the array of timing values for that water year. The final timing metric for the gage is the 10th, 50th, and 90th percentile of the values recorded for each water year. These results are outputted for each exceedance flow threshold \(2nd, 5th, 10th, and 20th percentile\).

#### Steps:

1. Convert raw data of from a single column of dates and a single column of flows into a matrix with columns organized by water year. Each column starts with the beginning of the water year \(i.e. 10/1\) and ends with the end of water year \(9/30\).
2. Calculate the 2nd, 5th, 10th, and 20th percentile exceedance flow values over the entire period of record \(i.e., the entire flow matrix\).
3. In each water year, create one object each time the flow passes the exceedance value, and append that object into an array.
4. Find the median timing value from the array of timings for that water year.

#### Code Snippet:

Once the flow value passes an exceedance percentile threshold, a `FlowExceedance` object is created with that flow's date. The object also tracks the **duration**, **highest magnitude** and **end day**.

```py
elif flow_row >= exceedance_value[percent]:
    if not current_flow_object[percent]:
        exceedance_object[percent].append(FlowExceedance(calender_date, None, 1, percent))
for percent in exceedance_percent:    
    timing[percent].append(median_of_time(exceedance_object[percent]))
```



