# Meter Status Layer

This layer adds commercial charm meter status support.

## Usage

Add this layer to your charm layer's `layer.yaml`:

```
includes:
 - layer:basic
 - layer:/path/to/meter-status
```

## Meter Status

The meter status feedback to your charm indicates whether it is operating
within a user's established budget. The status code will be one of the
following indicators:

- `green` indicates all is well; metrics are being received and the workload has
  sufficient budget allocated.
- `amber` indicates some risk that the workload may exceed allocated budget soon.
- `red` indicates that the workload is operating without a sufficient budget.

### Reacting to meter status

The reactive state `meter.status.changed` is set when the meter status changes.
Use `charms.metering.status()` to read the status. This function returns a
tuple of `(code, message)`.

If a meter status is set, `code` is one of `green`, `amber`, or `red`. If it's
not set, `code` will be `None`.

Meter status usually comes with an informative `message` string. If there is no
message, it will be `None`.

```
import charms.metering

...

@when('meter.status.changed')
def meter_status_changed():
    code, info = metering.status()
    ...
```

States are also set on the different status codes, so you can handle `green`,
`amber` or `red` with their own handler functions:

```
@when('meter.status.red')
def degrade_service_and_alert():
   ...

@when('meter.status.amber')
def ensure_service_and_warn():
   ...

@when('meter.status.green')
def ensure_service_and_clear():
   ...

```
