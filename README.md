# test
{{- if eq .Status "firing" -}}[ALERT]{{- else -}}[RESOLVED]{{- end -}}{{ if .CommonLabels.severity }}[{{ toUpper .CommonLabels.severity }}]{{ end }} {{ if .CommonLabels.alertname }}{{ .CommonLabels.alertname }}{{ else if .CommonAnnotations.summary }}{{ .CommonAnnotations.summary }}{{ else }}Alert{{ end }}{{ if .CommonLabels.env }} | env={{ .CommonLabels.env }}{{ end }}{{ if .CommonLabels.hostalias }} | host={{ .CommonLabels.hostalias }}{{ end }} ({{ len .Alerts.Firing }}f/{{ len .Alerts.Resolved }}r)



{{- $tz := "Europe/Kyiv" -}}
Overall state: {{ if eq .Status "firing" }}ALERT{{ else }}RESOLVED{{ end }}
Firing: {{ len .Alerts.Firing }} | Resolved: {{ len .Alerts.Resolved }}
Source: {{ .ExternalURL }}

{{- range .Alerts }}
--------------------------------------------------------------------------------
{{ if eq .Status "firing" }}🔥 Firing{{ else }}✅ Resolved{{ end }}
Name: {{ if .Labels.alertname }}{{ .Labels.alertname }}{{ else if .Annotations.summary }}{{ .Annotations.summary }}{{ else }}(unnamed alert){{ end }}

When:
  StartsAt: {{ .StartsAt | tz $tz | date "2006-01-02 15:04:05 MST" }}
  {{- if .EndsAt }}
  EndsAt:   {{ .EndsAt   | tz $tz | date "2006-01-02 15:04:05 MST" }}
  {{- end }}

Aliases:
  severity = {{ if .Labels.severity }}{{ .Labels.severity }}{{ else }}n/a{{ end }}
  env      = {{ if .Labels.env }}{{ .Labels.env }}{{ else }}n/a{{ end }}
  hostalias= {{ if .Labels.hostalias }}{{ .Labels.hostalias }}{{ else }}n/a{{ end }}

Annotations:
{{- if .Annotations }}
{{- range $k, $v := .Annotations.SortedPairs }}
  - {{ $k }}: {{ $v }}
{{- end }}
{{- else }}
  (none)
{{- end }}

Value(s): {{ .ValueString }}

Links:
  {{- if .DashboardURL }}Dashboard: {{ .DashboardURL }}{{ end }}
  {{- if .PanelURL }}
  {{- if .DashboardURL }} | {{ end }}Panel: {{ .PanelURL }}{{ end }}
  {{- if or .DashboardURL .PanelURL }} | {{ end }}Silence: {{ .SilenceURL }}
--------------------------------------------------------------------------------
{{- end }}
