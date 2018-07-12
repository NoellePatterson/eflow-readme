# Winter High Flows - Frequency

#### 

#### Definition:

High flow frequency is the number of times that flow crosses over the exceedance percentile threshold during a water year. For each water year, record an object for each event in which the flow crosses over the exceedance flow, and calculate the total number of exceedance objects. This results in an array of frequency values for each water year. The final timing metric for each water year is the median of the array of frequency values for that water year. The final frequency metric for the gage is the 10th, 50th, and 90th percentile of the values recorded for each water year. These results are outputted for each exceedance flow threshold \(2nd, 5th, 10th, 20th, and 50th percentile\).

#### Steps:

1. Convert raw data of from a single column of dates and a single column of flows into a matrix with columns organized by water year. Each column starts with the beginning of the water year \(i.e. 10/1\) and ends with the end of water year \(9/30\).
2. Calculate the 2nd, 5th, 10th, 20th, and 50th percentile exceedance flow values over the entire period of record \(i.e., the entire flow matrix\).
3. In each water year, create one object each time the flow passes the exceedance value, and append that object onto an array.
4. Calculate the frequency by counting the length of the object array.

#### Code Snippet:

Once the flow value passes an exceedance percentile threshold, a `FlowExceedance` object is created with that flow's date.The object also tracks the **duration**, **highest magnitude** and **end day**. Using `len()` method to count how many times the flow data passes the exceedance value.

```py
for i in exceedance_percent:
    freq[i].append(len(exceedance_object[i]))
```



