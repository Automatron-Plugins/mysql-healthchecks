# MySQL Health Checks

Collection of Nagios compliant health checks for Automatron

Includes:
  * available.py - Checks if MySQL is up and available by establishing connection
  * status_metrics.py - Validates and compares values of defined metrics from "show status" SQL query

## Installation

Simply copy the `mysql/` directory into `plugins/checks/`.

```shell
$ cp -r mysql plugins/checks/
```

## `available.py`

The `available` plugin is used to query MySQL's internal status system and determine whether the MySQL service is available or not.

### Runbook Example

The below is an example of using the `mysql/available` health check in a runbook.

```yaml
checks:
  mysql_up:
    execute_from: target
    type: plugin
    plugin: mysql/available.py
    args: --host=localhost --user=USERNAME --password=YOURPASSWORD
```

This plugin can be executed from either `target` or `remote` depending on the MySQL service's configuration.

#### Required arguments

The `mysql/available` plugin requires 3 arguments.

```yaml
args: -s <mysql host> -u <mysql user> -p <mysql password>
```

If the `show status` query is unsuccessful or does not find the key it is looking for the check will return a `CRITICAL` status. There is no `WARNING` or `UNKNOWN` status for this check.

## `status_metrics.py`

The `status_metrics` plugin is used to query MySQL's internal status system and alert when the defined `metric` exceeds the `warning` and `critical` thresholds.

### Runbook Example

The below is an example of using the `status_metrics` health check in a runbook.

```yaml
checks:
  status_metrics:
    execute_from: target
    type: plugin
    plugin: mysql/status_metrics.py
    args: --warn=20 --critical=10 --metric=slow_queries --host=localhost --user=USERNAME --password=YOURPASSWORD --type=greater
```

This plugin can be executed from either `target` or `remote` depending on the MySQL service's configuration.

#### Required arguments

The `mysql/status_metrics` plugin requires 7 arguments.

```yaml
args: -w <warning value> -c <critical value> -t {greater, lesser} -m <metric> -s <mysql host> -u <mysql user> -p <mysql password>
```

The `type` flag is used to define whether or not the alert is triggered when the `metric` value is "greater" or "lesser" than the values defined as `warn` and `critical`.
