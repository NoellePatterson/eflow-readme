# Peak Magnitude Flows - Wet Season Timing

#### Definition

The timing of the wet season is defined as the date that sufficient baseflow has accrued based on a magnitude threshold of 30% of the wet season’s smoothed peak flow. This metric is measured in Julian days, where January 1st = 1 and December 31st = 365.

#### Steps

1. Convert raw date and flow data columns into a flow matrix based on water year. Each column starts with the beginning of the water year \(i.e. 10/1\) and ends with the end of the water year \(i.e.9/30\).

2. Check if the data contains more than the allowed None and zero data. Then, interpolate between missing values to get a full dataset:

   ```py
   if np.isnan(flow_matrix[:, column_number]).sum() > max_nan_allowed_per_year or np.count_nonzero(flow_matrix[:, column_number]==0) > max_zero_allowed_per_year:
         continue
   ```

3. Filter the raw flow data with a broad resolution \(high sigma\) and a finer resolution \(low sigma\) filter to use in peak identifications:
   ```py
   wet_season_filter_data = gaussian_filter1d(flow_data, wet_season_sigma)
   broad_filter_data = gaussian_filter1d(flow_data, broad_sigma)
   ```
4. Identify the max and min flow values \(in the range of the beginning of the water year to the point of max flow\) using the high sigma filtered data, and identify the array of peaks using the low sigma filtered data.
   ```py
    max_wet_peak_mag = max(broad_filter_data[20:])
    max_wet_peak_index = find_index(broad_filter_data, max_wet_peak_mag)
    min_wet_peak_mag = min(broad_filter_data[:max_wet_peak_index])
    maxarray_wet, minarray = peakdet(wet_season_filter_data, peak_sensitivity_wet)
   ```
5. Identify the earliest peak of sufficient relative magnitude to use as the search index for the start of the wet season.
   ```py
   if (maxarray_wet[index][1]-min_wet_peak_mag)/(max_wet_peak_mag-min_wet_peak_mag) > peak_detect_perc:
   search_index = int(maxarray_wet[index][0])
   break
   ```
6. Search from right to left, starting at the search index, for the first flow value that falls below the relative magnitude threshold of 30%. This flow date is set as the start date of the wet season.
   ```py
   for index, value in enumerate(reversed(wet_season_filter_data[:search_index])):
        if index == len(wet_season_filter_data[:search_index] - 1):
            return None
        elif (value - min_wet_peak_mag) / (max_wet_peak_mag - min_wet_peak_mag) < wet_threshold_perc:
            """If value percentage falls below wet_threshold_perc"""
            return_date = search_index - index
            return return_date
   ```
7. Add 10 days to the identified wet season start date. This has been found to better adjust the final result to the inaccuracy caused by the data filtering.
   ```py
   if return_date:
      wet_dates[-1] = return_date + 10
   ```
