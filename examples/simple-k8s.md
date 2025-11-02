Name: `simple-k8s`

For the Components section
```
[
  {"type": "deployment", "name": "nginx", "replicas": 3},
  {"type": "service", "name": "nginx-svc"},
  {"type": "ingress", "name": "web-ingress"}
]
```

For the Field section
```
[
  {"from": "web-ingress", "to": "nginx-svc"},
  {"from": "nginx-svc", "to": "nginx"}
]
```

For the Clusters section
```
[]
```