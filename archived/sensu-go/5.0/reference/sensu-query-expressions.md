---
title: "Sensu query expressions"
linkTitle: "Sensu Query Expressions"
description: "Based on JavaScript expressions, Sensu query expressions provide additional functionalities for Sensu usage (like nested parameters and custom functions) so Sensu resources can be evaluated directly. Read the reference doc to learn about Sensu query expressions."
weight: 10
version: "5.0"
product: "Sensu Go"
platformContent: false 
menu:
  sensu-go-5.0:
    parent: reference
---

- [Specification](#sensu-query-expressions-specification)
- [Examples](#sensu-query-expressions-examples)

## How do Sensu query expressions work?

Sensu query expressions (**SQE**) are based on [JavaScript][3] expressions, and
provide additional functionalities for Sensu usage (like nested parameters and
custom functions) so Sensu resources can be directly evaluated. SQE should
always return **true** or **false**.

## Syntax quick reference

<table>
<thead>
<tr>
<th>operator</th>
<th>description</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>===</code> / <code>!==</code></td>
<td>Identity operator / Nonidentity operator</td>
</tr>
<tr>
<td><code>==</code> / <code>!=</code></td>
<td>Equality operator / Inequality operator</td>
</tr>
<tr>
<td><code>&&</code> / <code>||</code></td>
<td>Logical AND / Logical OR</td>
</tr>
<tr>
<td><code><</code> / <code>></code></td>
<td>Less than / Greater than</td>
</tr>
<tr>
<td><code><=</code> / <code>>=</code></td>
<td>Less than or equal to / Greater than or equal to</td>
</tr>
</tbody>
</table>

## Sensu query expressions specification

Sensu query expressions are valid ECMAScript 5 (JavaScript) expressions that return
**true** or **false**. Other values are not allowed. If other values are
returned, an error is logged and the filter evaluates to false.

### Custom functions

* `hour`: returns the hour, in UTC and in the 24-hour time notation, of a UNIX
  Epoch time.

{{< highlight go >}}
// event.timestamp equals to 1520275913, which is Monday, March 5, 2018 6:51:53 PM UTC
// The following expression returns true
hour(event.timestamp) >= 17
{{< /highlight >}}

* `weekday`: returns a number representing the day of the week, where Sunday
  equals `0`, of a UNIX Epoch time.

{{< highlight go >}}
// event.timestamp equals to 1520275913, which is Monday, March 5, 2018 6:51:53 PM UTC
// The following expression returns false
weekday(event.timestamp) == 0
{{< /highlight >}}

## Sensu query expressions examples

### Evaluating an event attribute

The following example returns true if the event's entity contains a custom
attribute named `namespace` that is equal to `production`.

{{< highlight javascript >}}
event.entity.namespace == 'production'
{{< /highlight >}}

### Evaluating an array

To evaluate an attribute that contains an array of elements, use the `.indexOf` method.
The following example returns true if an entity includes the subscription `system`.

{{< highlight text >}}
entity.subscriptions.indexOf('system') >= 0
{{< /highlight >}}

### Evaluating the day of the week

The following example returns true if the event occurred on a weekday.

{{< highlight javascript >}}
weekday(event.timestamp) >= 1 && weekday(event.timestamp) <= 5
{{< /highlight >}}

### Evaluating office hours

The following example returns true if the event occurred between 9 AM and 5 PM
UTC.

{{< highlight javascript >}}
hour(event.timestamp) >= 9 && hour(event.timestamp) <= 17
{{< /highlight >}}

[2]: ../../../latest/reference/filters/#what-are-filter-attribute-eval-tokens
[3]: https://github.com/robertkrimen/otto
