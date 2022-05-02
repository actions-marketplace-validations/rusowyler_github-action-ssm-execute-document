# github-action-ssm-execute-document

This action will connect to AWS using OpenId Connect identity provider and run a SSM document.
Optionally you may specify a SMS ARN to receive status change notifications.

This action will not wait for the script to finish. If you need notifications of the status change
or results use the 'sns-notification-arn' parameter to enable notifications.

There are different variables to setup the action:

## Inputs

### `target-key` (argument) [optional]

The tags key to filter the target instances. Defaults to: Name.

### `target-values` (argument)

The tags values to filter the target instances. Coma separated.
For example: servers-a,stg-web

### `ssm-document-name` (argument)

The SSM document you want to execute. Check the role assumed has permissions to execute it.

### `aws-account-id` (argument)

The AWS account number.

### `aws-region` (argument)

The AWS region where the SSM document and the target servers reside.

### `sns-notification-arn` (argument)

The ARN to send the state notifications of the executed command

## Example usage

```yaml
name: "Run 'date' on Web Server"
on:
  workflow_dispatch
jobs:
  execute:
    runs-on: ubuntu-latest    
    permissions: # for GitHub's OIDC Token endpoint.
      id-token: write 
      contents: read
    steps:
      - name: Execute SSM command
        uses: lendistry/github-action-ssm-execute-document
        with:
          target-key: "type"
          target-values: "web-server" # comma separated values
          ssm-document-name: "getdate" # ony allow documents
          aws-account-id: "123456789012"
          aws-region: "us-east-2"
          sns-notification-arn: "" # optional SNS notification ARN
```