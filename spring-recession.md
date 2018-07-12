# Spring Transition

The spring transition marks the seasonal shift from high magnitude winter flows to the  dry season baseflow. A continuous decline in flows is expected during this period until the beginning of the summer baseflow is reached. The metrics used to characterize the spring transition are magnitude and timing on the first day of the transition, duration of the spring transition period, and rate of change of flow across the period.

#### Basic Steps

1. Smooth data with a heavy Gaussian filter

2. Identify the rightmost peak flow date \(of smoothed data\), which is used to set a search window 20 days before and 50 days after the rightmost peak flow date.

3. Create a spline of the windowed data, then calculate the first derivative of the spline. This lets us identify the local peaks in which the sign changes positive to negative.

4. The start of spring date will be the last \(rightmost\) local peak which fills the following requirements:

   * 4 continuous days of increase before the start date, with a specified amount of increase each day.
   * The relative flow \(Q-Qmin\) magnitude must be at least 50% of peak relative flow magnitude \(Qmax-Qmin\). Qmin and Qmax are calculated as the smoothed minimum and maximum flows within the search window.

5. Search the unfiltered data 4 days before and 7 days after the identified start date. Change the start of spring date to the highest flow date within this range.

6. Finally, shift the start date 4 days after the date just identified. This is meant to eliminate the flow effects of isolated storms as much as possible.

#### Extra considerations:

When the maximum flow of a water year is extremely low, the FFC algorithm is less effective at detecting spring timing. This is sometimes due to low flows that approach the sensitivity of the flow meter, causing the appearance of step-wise increases in flow \(see figure X\). To control for this issue, a modified spring timing algorithm is used for extremely low flow water years. In this case, the algorithm simply sets the start of spring as the date with the maximum filtered flow value.
