{{- range .Alerts }}
{{- if eq .Status "firing" }}ALERT{{ else }}RESOLVED{{ end }}: {{ if .Labels.alertname }}{{ .Labels.alertname }}{{ else }}alert{{ end }}{{ "\n" }}
env={{ if .Labels.env }}{{ .Labels.env }}{{ else }}n/a{{ end }}, host={{ if .Labels.hostalias }}{{ .Labels.hostalias }}{{ else }}n/a{{ end }}, severity={{ if .Labels.severity }}{{ .Labels.severity }}{{ else }}n/a{{ end }}{{ "\n" }}
StartsAt: {{ .StartsAt }}{{ if .EndsAt }}{{ "\n" }}EndsAt:   {{ .EndsAt }}{{ end }}{{ "\n" }}
{{- if .Annotations }}
Annotations:{{ "\n" }}
{{- range .Annotations.SortedPairs -}}
- {{ .Name }}: {{ .Value }}{{ "\n" }}
{{- end -}}
{{- end -}}
{{- if .DashboardURL }}Dashboard: {{ .DashboardURL }}{{ "\n" }}{{- end -}}
{{- if .PanelURL }}Panel: {{ .PanelURL }}{{ "\n" }}{{- end -}}
{{- if .SilenceURL }}Silence: {{ .SilenceURL }}{{ "\n" }}{{- end -}}
---{{ "\n\n" }}
{{- end }}
