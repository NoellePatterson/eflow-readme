# Wet Season Initiation Flows - Duration

#### Definition:

Wet season initiation duration is usually set as the the number of days from the beginning of the initiation event until the initiation event peak.

#### Steps:

1. The start date calculated for the wet season initiation timing and the Gaussian filtered flow data are used as inputs for the duration algorithm.
2. ```py
   def calc_fall_flush_durations_2(filter_data, date):
   ```
3. Set a requirement for slope and flow magnitude on either side of the fall flush \(left-side rising limb and right-side falling limb\). The derivative percent threshold shown in the code snippet below is the slope requirement. Each side of the peak must have a certain degree of steepness, and once it falls below the steepness threshold then the left or right end of the peak is reached. The derivative threshold is higher for the left side \(requires a steeper rising limb\) and lower for the right side \(requires a less steep falling limb\). This is based on the typical shape of a storm-caused peak in flow. 

The flow percent threshold is a magnitude percentile requirement for the left or right side of the fall flush peak. For the left side, the left end of the fall flush peak must have a lower magnitude than a certain percentile \(in this case the 80th percentile\) of all flow values on the left side of the peak. The same requirement is used for the right side of the peak.

```py
"""Left side sharp"""
    der_percent_threshold_left = 50
    flow_percent_threhold_left = 80

    """Right side mellow"""
    der_percent_threshold_right = 30
    flow_percent_threhold_right = 80
```

1. Find the valleys on either side of the fall flush peak, using the minimum values from the peak detection algorithm. If a valley is within 10 days of the fall flush peak on either side, then automatically set that valley as the end of the fall flush. This could occur on one side of the fall flush peak and not the other side. If there are no identified valleys on either the left or right side of the fall flush peak, then continue to the next step to identify the valleys \(the right and left durations are set as zero by default, then later reassigned when the actual duration is found\).

   ```py
   left_maxarray, left_minarray = peakdet(filter_data[:date], 0.01)
    right_maxarray, right_minarray = peakdet(filter_data[date:], 0.01)

    if not list(left_minarray):
        left = 0
    else:
        left = int(left_minarray[-1][0])

    if not list(right_minarray):
        right = 0
    else:
        right = int(date - 2 + right_minarray[0][0])
   ```

2. If the left side of the fall flush has not already been detected as a valley within 10 days of the fall flush peak, then find the left side through the following requirements:  
   1. The slope of the rising \(left-side\) limb falls below the derivative percent threshold  
   2. The flow magnitude of the far left side falls below the percentile threshold.

3. ```py
   if date - left > 10:
               """create spline, and find derivative"""
               x_axis_left = list(range(len(filter_data[left:date])))
               spl_left = ip.UnivariateSpline(x_axis_left, filter_data[left:date], k=3, s=3)
               spl_first_left = spl_left.derivative(1)

               """check if derivative value falls below certain threshold"""
               spl_first_left_median = np.nanpercentile(spl_first_left(x_axis_left), der_percent_threshold_left)

               """check if actual value falls below threshold, avoiding the rounded peak"""
               median_left = np.nanpercentile(list(set(filter_data[left:date])), flow_percent_threshold_left)

               for index_left, der in enumerate(reversed(spl_first_left(x_axis_left))):
                   # print(der < spl_first_left_median, filter_data[date - index_left] < median_left)
                   if der < spl_first_left_median and filter_data[date - index_left] < median_left:
                       left = date - index_left
                       break
   ```
4. If the right side of the fall flush has not already been detected as a valley within 10 days of the fall flush peak, then find the right side through the following requirements:  
   1. The slope of the falling \(right-side\) limb falls below the derivative percent threshold  
   2. The flow magnitude of the far right side falls below the percentile threshold.

5. ```py
   if right - date > 10:
               x_axis_right = list(range(len(filter_data[date:right])))
               spl_right = ip.UnivariateSpline(x_axis_right, filter_data[date:right], k=3, s=3)
               spl_first_right = spl_right.derivative(1)

               spl_first_right_median = abs(np.nanpercentile(spl_first_right(x_axis_right), der_percent_threshold_right))
               median_right = np.nanpercentile(list(set(filter_data[date:right])), flow_percent_threshold_right)

               for index_right, der in enumerate(spl_first_right(x_axis_right)):
                   # print(date+index_right, der < spl_first_right_median, filter_data[date + index_right] < median_right)
                   if abs(der) < spl_first_right_median and filter_data[date + index_right] < median_right:
                       right = date + index_right
                       break
   ```
6. Once the left and right ends of the fall flush have been detected:

   * Set the duration as the number of days between the left side of the fall flush peak and the fall flush peak:

     ```py
     if left:
     duration = int(date - left)
     ```

   * If there is no left side identified, then set the duration as the number of days between the fall flush peak and the right side of the fall flush peak:
     ```py
     elif not left and right:
     duration = int(right - date)
     ```



