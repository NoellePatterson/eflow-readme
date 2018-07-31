# Coefficient of Variation

#### Definition

Coefficient of variation \(COV\) is defined as the standard deviation divided by the mean of the daily flow for each water year and is calculated individually for each water year.

#### Steps

1. Convert raw date and flow data columns into a flow matrix based on water year. Each column starts with the beginning of the water year \(i.e. 10/1\) and ends with the end of the water year \(i.e.9/30\).

2. Calculate the coefficient of variation for each water year:
  ```py
  coefficient_variations.append(standard_deviations[-1] / average_annual_flows[-1])
  ```
