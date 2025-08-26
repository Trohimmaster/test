[{{ if eq .Status "firing" }}ALERT{{ else }}RESOLVED{{ end }}] {{ .Labels.alertname }} ({{ .Labels.severity }})


Status:  {{ .Status }}
Alert:   {{ .Labels.alertname }}
Env:     {{ .Labels.env }}
Host:    {{ .Labels.hostalias }}
Severity: {{ .Labels.severity }}

When:
  StartsAt: {{ .StartsAt }}
  {{- if .EndsAt }}
  EndsAt:   {{ .EndsAt }}
  {{- end }}

Annotations:
{{ range $key, $value := .Annotations }}
  • {{ $key }}: {{ $value }}
{{ end }}

Dashboard link:
{{ .Annotations.dashboard_link }}
