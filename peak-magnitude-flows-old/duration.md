# Winter High Flow - Duration

#### Definition:

High flow duration is the median number of days in which flow stays above the exceedance percentile threshold during a water year. 

#### Steps:

1. Convert raw data of from a single column of dates and a single column of flows into a matrix with columns organized by water year. Each column starts with the beginning of the water year \(i.e. 10/1\) and ends with the end of water year \(9/30\).
2. Calculate the 2nd, 5th, 10th, and 20th percentile exceedance flow values over the entire period of record \(i.e., the entire flow matrix\).
3. In each water year, create one object each time the flow passes the exceedance value, and calculate the number of days the flow stays above the threshold for that object.
4. Find the median duration value from the array of durations for that water year. 

#### Code Snippet:

Once the flow value passes an exceedance percentile threshold, a `FlowExceedance` object is created with that flow's date. The object also tracks the **duration**, **highest magnitude** and **end day**. For each day the flow value is over the exceedance value, the object adds the flow value to itself using the `add_flow` method. Duration is then calculated by finding the length of `flow` attribute.

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



