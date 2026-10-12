# CloudWatch Monitoring

## Objective

Configure infrastructure monitoring for the CloudOps lab EC2 instance and verify that system-level metrics are available in Amazon CloudWatch.

## EC2 Basic Monitoring

EC2 monitoring provides infrastructure metrics such as:

- CPU utilization
- Network traffic
- Network packets

## CloudWatch Agent

The CloudWatch Agent was installed on the Ubuntu EC2 instance and configured to publish custom metrics under the namespace:

`CloudOps-Lab`

The configured metrics are:

| Metric | Description | Collection interval |
|---|---|---|
| `mem_used_percent` | Percentage of memory used | 60 seconds |
| `disk_used_percent` | Percentage of disk space used on `/` | 60 seconds |

The agent status was verified as `running`, with configuration status `configured`.

Both memory and disk metrics were viewed in the CloudWatch metrics interface.

## CPU Alarm

| Setting | Value |
|---|---|
| Alarm name | `CloudOps-Lab-High-CPU` |
| Metric | `AWS/EC2 CPUUtilization` |
| Statistic | Average |
| Period | 5 minutes |
| Threshold | Greater than 70% |
| Evaluation | 1 datapoint within 5 minutes |
| Notification action | Not configured |
| Observed state during validation | `OK` |

## Validation Procedure

1. Confirmed the CloudWatch Agent was running on the EC2 instance.
2. Opened the `CloudOps-Lab` namespace in CloudWatch.
3. Viewed the `mem_used_percent` metric.
4. Viewed the `disk_used_percent` metric.
5. Adjusted the graph period to one minute to make repeated data points easier to observe.
6. Checked the CPU alarm and observed its state as `OK`.

## Operational Notes

- The CPU alarm state `OK` means the configured alarm condition was not breached at the time of observation.
- An alarm in `OK` state does not by itself prove that notifications work; no notification action was configured.
- Custom CloudWatch metrics may incur charges. Review usage and AWS billing before extending the monitoring configuration.
