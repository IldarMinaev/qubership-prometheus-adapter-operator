# CustomScaleMetricRule API Docs

This Document documents the types introduced by the Prometheus Adapter Operator to be consumed by users.

> Note this document is generated from code comments.
> When contributing a change to this document please do so by changing the code comments.

## Table of Contents

* [CustomMetricRuleConfig](#custommetricruleconfig)
* [CustomScaleMetricRule](#customscalemetricrule)
* [CustomScaleMetricRuleList](#customscalemetricrulelist)
* [CustomScaleMetricRuleSpec](#customscalemetricrulespec)
* [GroupResource](#groupresource)
* [NameMapping](#namemapping)
* [RegexFilter](#regexfilter)
* [ResourceMapping](#resourcemapping)

## CustomMetricRuleConfig

CustomMetricRuleConfig defines the metric exposing rule from Prometheus.
This structure is similar to the DiscoveryRule from github.com/directxman12/k8s-prometheus-adapter/pkg/config but
we can not use the original structure because it is not compliant with kube-builder's CRD generator.

| Field | Description | Scheme | Required |
| ----- | ----------- | ------ | -------- |
| seriesQuery | SeriesQuery specifies which metrics this rule should consider via a Prometheus query series selector query. | string | true |
| seriesFilters | SeriesFilters specifies additional regular expressions to be applied on the series names returned from the query. This is useful for constraints that can't be represented in the SeriesQuery (e.g. series matching `container_.+` not matching `container_.+_total`. A filter will be automatically appended to match the form specified in Name. | []RegexFilter | false |
| resources | Resources specifies how associated Kubernetes resources should be discovered for the given metrics. | [ResourceMapping](#resourcemapping) | true |
| name | Name specifies how the metric name should be transformed between custom metric API resources, and Prometheus metric names. | [NameMapping](#namemapping) | true |
| metricsQuery | MetricsQuery specifies modifications to the metrics query, such as converting cumulative metrics to rate metrics. It is a template where `.LabelMatchers` is a the comma-separated base label matchers and `.Series` is the series name, and `.GroupBy` is the comma-separated expected group-by label names. The delimeters are `<<` and `>>`. | string | false |

[Back to TOC](#table-of-contents)

## CustomScaleMetricRule

CustomScaleMetricRule is the Schema for the customscalemetricrules API

| Field | Description | Scheme | Required |
| ----- | ----------- | ------ | -------- |
| metadata |  | [metav1.ObjectMeta](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.17/#objectmeta-v1-meta) | false |
| spec |  | [CustomScaleMetricRuleSpec](#customscalemetricrulespec) | false |
| status |  | CustomScaleMetricRuleStatus | false |

[Back to TOC](#table-of-contents)

## CustomScaleMetricRuleList

CustomScaleMetricRuleList contains a list of CustomScaleMetricRule

| Field | Description | Scheme | Required |
| ----- | ----------- | ------ | -------- |
| metadata |  | [metav1.ListMeta](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.17/#listmeta-v1-meta) | false |
| items |  | []CustomScaleMetricRule | true |

[Back to TOC](#table-of-contents)

## CustomScaleMetricRuleSpec

CustomScaleMetricRuleSpec defines the desired state of CustomScaleMetricRule

| Field | Description | Scheme | Required |
| ----- | ----------- | ------ | -------- |
| rules |  | []CustomMetricRuleConfig | false |

[Back to TOC](#table-of-contents)

## GroupResource

GroupResource represents a Kubernetes group-resource.

| Field | Description | Scheme | Required |
| ----- | ----------- | ------ | -------- |
| group |  | string | false |
| resource |  | string | true |

[Back to TOC](#table-of-contents)

## NameMapping

NameMapping specifies how to convert Prometheus metrics to/from custom metrics API resources.

| Field | Description | Scheme | Required |
| ----- | ----------- | ------ | -------- |
| matches | Matches is a regular expression that is used to match Prometheus series names.  It may be left blank, in which case it is equivalent to `.*`. | string | true |
| as | As is the name used in the API.  Captures from Matches are available for use here.  If not specified, it defaults to $0 if no capture groups are present in Matches, or $1 if only one is present, and will error if multiple are. | string | false |

[Back to TOC](#table-of-contents)

## RegexFilter

RegexFilter is a filter that matches positively or negatively against a regex. Only one field may be set at a time.

| Field | Description | Scheme | Required |
| ----- | ----------- | ------ | -------- |
| is |  | string | false |
| isNot |  | string | false |

[Back to TOC](#table-of-contents)

## ResourceMapping

ResourceMapping specifies how to map Kubernetes resources to Prometheus labels

| Field | Description | Scheme | Required |
| ----- | ----------- | ------ | -------- |
| template | Template specifies a golang string template for converting a Kubernetes group-resource to a Prometheus label.  The template object contains the `.Group` and `.Resource` fields.  The `.Group` field will have dots replaced with underscores, and the `.Resource` field will be singularized.  The delimiters are `<<` and `>>`. | string | false |
| overrides | Overrides specifies exceptions to the above template, mapping label names to group-resources | map[string]GroupResource | false |

[Back to TOC](#table-of-contents)
