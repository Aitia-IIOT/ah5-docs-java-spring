# DeviceQoSEvaluator - Augmented Measurements

## Group

| Group name (x) | OID prefix |
| -------------- | ---------- |
| CPU | 1.y |
| Memory | 2.y |
| Network Interface | 3.y |

## Sub-group

| CPU sub-group (y) | OID |
| -------------- | ---------- |
| Total Load % | 1.4.z |

| Memory sub-group (y) | OID |
| -------------- | ---------- |
| Used % | 2.1.z |

| Network sub-group (y) | OID |
| -------------- | ---------- |
| Egress Load % | 3.1.z |
| Ingress Load % | 3.2.z |

## Statistics

| Statistics object (z) | OID |
| -------------- | ---------- |
| Minimum| x.y.1 |
| Mean | x.y.2 |
| Median | x.y.3 |
| Maximum | x.y.4 |
| Current | x.y.5 |