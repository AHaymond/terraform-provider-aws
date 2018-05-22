---
layout: "aws"
page_title: "AWS: aws_guardduty_invite_accepter"
sidebar_current: "docs-aws-resource-guardduty-invite-accepter"
description: |-
  Provides a resource to accept a pending GuardDuty invite on creation, ensure the detector has the correct master account on read, and disassociate with the master account upon removal.
---

# aws_guardduty_invite_accepter

Provides a resource to accept a pending GuardDuty invite on creation, ensure the detector has the correct master account on read, and disassociate with the master account upon removal.

## Example Usage

```hcl
resource "aws_guardduty_detector" "master" {}

resource "aws_guardduty_detector" "member" {
  provider = "aws.dev"
}

resource "aws_guardduty_member" "member" {
  account_id         = "${aws_guardduty_detector.member.account_id}"
  detector_id        = "${aws_guardduty_detector.master.id}"
  email              = "required@example.com"
  invite             = true
}

resource "aws_guardduty_invite_accepter" "member" {
  depends_on = ["aws_guardduty_member.member"]
  provider   = "aws.dev"

  detector_id        = "${aws_guardduty_detector.member.id}"
  master_id          = "${aws_guardduty_detector.master.account_id}"
}
```

## Argument Reference

The following arguments are supported:

* `detector_id` - (Required) The detector ID of the member GuardDuty account.
* `master_id` - (Required) AWS account ID for master account.

## Attributes Reference

The following additional attributes are exported:

* `id` - The member GuardDuty detector ID and master AWS account ID

## Timeouts

`aws_guardduty_invite_accepter` provides the following [Timeouts](/docs/configuration/resources.html#timeouts)
configuration options:

- `create` - (Default `60s`) How long to wait for an invite to accept.

## Import

`aws_guardduty_invite_accepter` can be imported using the the member GuardDuty detector ID and master AWS account ID, e.g.

```
$ terraform import aws_guardduty_invite_accepter.member 00b00fd5aecc0ab60a708659477e9617:123456789012
```
