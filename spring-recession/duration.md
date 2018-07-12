# Spring Recession Flows - Duration

#### Definition

The duration of the spring recession is simply calculated as the period of elapsed time from the start of the spring recession until the start date of the following dry season low flows period.

```py
for index, spring_timing in enumerate(spring_timings):
        if spring_timing and summer_timings[index] and summer_timings[index] > spring_timing:
            duration_array.append(summer_timings[index] - spring_timing)
        else:
            duration_array.append(None)
```



