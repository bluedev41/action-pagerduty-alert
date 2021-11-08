# PagerDuty Alert GitHub Action
Sends a PagerDuty alert, e.g. on action failure.

## Prerequisites

1. Create a service integration in PagerDuty:
    1. Go to PagerDuty > "Services" > Pick your service > "Integrations" > "Add a new integration"
    2. Choose a name (e.g. "Your GitHub CI/CD") and "Use our API directly" with "Events API v2"
    3. Copy the integration key
2. Set up a secret in your GitHub repo to store the integration key, e.g. "PAGERDUTY_INTEGRATION_KEY"

## Inputs

`pagerduty-integration-key`

**Required:** the integration key for your PagerDuty service

`alert-summary`

**Required:** A brief text summary of the event, used to generate the summaries/titles of any associated alerts. The maximum permitted length of this property is 1024 characters.

`alert-severity`

**Required:** The perceived severity of the status the event is describing with respect to the affected system. This can be critical, error, warning or info.

`alert-event-action`

**Required:** The type of event. Can be trigger, acknowledge or resolve.

`pagerduty-dedup-key`

**Optional:** a `dedup_key` for your alert. This will enable PagerDuty to coalesce multiple alerts into one.
More documentation is available [here](https://developer.pagerduty.com/docs/events-api-v2/trigger-events/).

## Example usage

In your `steps`:

```yaml
- name: Send PagerDuty alert on failure
  if: ${{ failure() }}
  uses: smartcontractkit/action-pagerduty-alert@0.1.0
  with:
    alert-summary: 'Example alert failed because of xyz'
    alert-severity: 'warning'
    event-action: 'trigger'
    pagerduty-integration-key: '${{ secrets.PAGERDUTY_INTEGRATION_KEY }}'
    pagerduty-dedup-key: github_workflow_failed
```