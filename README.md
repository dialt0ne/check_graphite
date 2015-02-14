check_graphite
==============

Icinga/Nagios service check script using graphite data

Script based on the work done by [disqus](https://github.com/disqus/nagios-plugins) and [wikimedia](https://github.com/wikimedia/operations-puppet/blob/production/modules/nagios_common/files/check_commands/check_graphite)

## Usage

### general:

```
$ ./check_graphite -h
usage: check_graphite [-h] [-U URL] [-T TIMEOUT]
                      {check_anomaly,check_series_threshold,check_threshold}
                      ...

Controller for the graphite fetching plugin. You can build a few different
type of checks, both traditional nagios checks and anomaly detection ones.
Examples: Check if a metric exceeds a certain value 10 times in the last 20
minutes: ./check_graphite --url http://some-graphite-host check_threshold
my.beloved.metric --from -20minutes --threshold 100 --over -C 10 -W 5 Check if
a metric has exceeded its holt-winters confidence bands 5% of the times over
the last 500 checks ./check_graphyte.py --url http://some-graphite-host
check_anomaly my.beloved.metric --check_window 500 -C 5 -W 1

optional arguments:
  -h, --help            show this help message and exit
  -U URL, --url URL     Url of the graphite server
  -T TIMEOUT, --timeout TIMEOUT
                        Timeout on the graphite call (defaults to 10)

check_type:
  {check_anomaly,check_series_threshold,check_threshold}
                        use with --help for additional help
    check_threshold     Checks if the metric exceeds the desired threshold
    check_series_threshold
                        Checks a series of metrics to see if they individually
                        are within acceptable thresholds
    check_anomaly       Checks if the metric is out of the forecasted bounds
                        for a number of times in the last iterations
```

### check_threshold:

```
$ ./check_graphite check_threshold -h
usage: check_graphite check_threshold [-h] [-C CRIT] [-W WARN] [--from _FROM]
                                      [--over] [--under] [--perc PERCENTAGE]
                                      METRIC

positional arguments:
  METRIC                the metric to fetch from graphite

optional arguments:
  -h, --help            show this help message and exit
  -C CRIT, --critical CRIT
                        Threshold for critical alert (integer)
  -W WARN, --warning WARN
                        Threshold for warning (integer)
  --from _FROM          When to fetch the metric from (date or "-1d")
  --over                If alarms should happen when we exceed the threshold
  --under               If alarms should happen when we are below the
                        threshold
  --perc PERCENTAGE     Number of datapoints above or below threshold that
                        will raise the alarm
```

### check_series_threshold:

```
$ ./check_graphite check_series_threshold -h
usage: check_graphite check_series_threshold [-h] [-C CRIT] [-W WARN]
                                             [--from _FROM] [--over] [--under]
                                             [--perc PERCENTAGE]
                                             METRIC

positional arguments:
  METRIC                the metric to fetch from graphite

optional arguments:
  -h, --help            show this help message and exit
  -C CRIT, --critical CRIT
                        Threshold for critical alert (integer)
  -W WARN, --warning WARN
                        Threshold for warning (integer)
  --from _FROM          When to fetch the metric from (date or "-1d")
  --over                If alarms should happen when we exceed the threshold
  --under               If alarms should happen when we are below the
                        threshold
  --perc PERCENTAGE     Number of datapoints above or below threshold that
                        will raise the alarm
```

### check_anomaly:

```
$ ./check_graphite check_anomaly -h
usage: check_graphite check_anomaly [-h] [-C CRIT] [-W WARN]
                                    [--check_window CHECK_WINDOW]
                                    [--delta DELTA] [--over] [--under]
                                    METRIC

positional arguments:
  METRIC                the metric to fetch from graphite

optional arguments:
  -h, --help            show this help message and exit
  -C CRIT, --critical CRIT
                        Threshold for critical alert (integer)
  -W WARN, --warning WARN
                        Threshold for warning (integer)
  --check_window CHECK_WINDOW
                        How many datapoints to consider in the anomaly
                        detection sampling (we will still require 1w of data)
  --delta DELTA         Holt-Winters delta
  --over                If alarms should happen when we are above normal
                        values
  --under               If alarms should happen when we are below normal
                        values
```
