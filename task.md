# Linux Program Specification

Develop a program for the Linux operating system that implements the following functionality:

- Worker Processes: Should accept UDP messages in JSON format on a port with a specific number (see structure below).
- Master Process:
  - Aggregates information collected from all workers.
  - Computes and writes metrics based on the received data to a file at intervals of 10 seconds and 60 seconds.


## Input Message Format:

```json
{
    "A1": <integer>,
    "A2": <integer>,
    "A3": <integer>
}
```


## File Writing Format:

```json
{
    "timestamp": "<time stamp in seconds>",
    "count_type": "<'10s' or '60s' - indicating whether the record is for 10 or 60 seconds, respectively>",
    "A1_sum": "<sum of A1 values for the count_type interval>",
    "A2_max": "<maximum value of A2 for the count_type interval>",
    "A3_min": "<minimum value of A3 for the count_type interval>"
}
```

Assuming messages were received in the first 10 seconds:
```json
{"A1": 1, "A2": 10, "A3": 100}
{"A1": 2, "A2": 12, "A3": 102}
{"A1": 3, "A2": 30, "A3": 130}
```

And the next 10 seconds:
```json
{"A1": 4, "A2": 20, "A3": 200}
{"A1": 5, "A2": 13, "A3": 103}
{"A1": 6, "A2": 40, "A3": 140}
```

The resulting file would look like:
```json
{"timestamp": 123456710, "count_type": "10s", "A1_sum": 6, "A2_max": 30, "A3_min": 100}
{"timestamp": 123456720, "count_type": "10s", "A1_sum": 15, "A2_max": 40, "A3_min": 103}
{"timestamp": 123456730, "count_type": "10s", "A1_sum": 0, "A2_max": 0, "A3_min": 0}
{"timestamp": 123456740, "count_type": "10s", "A1_sum": 0, "A2_max": 0, "A3_min": 0}
{"timestamp": 123456750, "count_type": "10s", "A1_sum": 0, "A2_max": 0, "A3_min": 0}
{"timestamp": 123456760, "count_type": "10s", "A1_sum": 0, "A2_max": 0, "A3_min": 0}
{"timestamp": 123456760, "count_type": "60s", "A1_sum": 21, "A2_max": 40, "A3_min": 100}
```

The program should be executed under Python and efficiently utilize processor time (on each core), performing input/output operations and metric calculations.
