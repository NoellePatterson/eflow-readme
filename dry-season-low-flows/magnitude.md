# Dry Season Low Flows - Magnitude

#### Description

The magnitude of the summer baseflow is calculated as the 10th percentile of the flows ranging from the start date of the summer baseflow to the start date of the wet season. 

```py
if not np.isnan(summer_start_date) and not np.isnan(fall_flush_wet_dates[column_number]):
    su_date = int(summer_start_date)
    wet_date = int(fall_flush_wet_dates[column_number])
if not np.isnan(fall_flush_wet_dates[column_number]):
    flow_data_wet = list(flow_matrix[su_date:,column_number]) + list(flow_matrix[:wet_date, column_number])
summer_10_magnitudes.append(np.nanpercentile(flow_data_wet, 10))
```



