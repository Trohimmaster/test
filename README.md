{{ range .Alerts }}
<b>Status:</b> {{ .Status }}<br>
<b>Alert:</b> {{ .Labels.alertname }}<br>
<b>Labels:</b><br>
<ul>
  {{ range .Labels.SortedPairs }}
    <li>{{ .Name }} = {{ .Value }}</li>
  {{ end }}
</ul>
<b>Annotations:</b><br>
<ul>
  {{ range .Annotations.SortedPairs }}
    <li>{{ .Name }} = {{ .Value }}</li>
  {{ end }}
</ul>
<hr>
{{ end }}




{{ range .Alerts }}
<b>Status:</b> {{ .Status }}<br>
<b>Alert:</b> {{ .Labels.alertname }}<br>
<b>Env:</b> {{ .Labels.env }}<br>
<b>Host:</b> {{ .Labels.hostalias }}<br>
<b>Severity:</b> {{ .Labels.severity }}<br>

<b>Annotations:</b><br>
- {{ .Annotations.summary }}<br>
- {{ .Annotations.description }}<br>
- <a href="{{ .Annotations.dashboard }}">Open Dashboard</a><br>
<hr>
{{ end }}


<b>Dashboard:</b> <a href="{{ index .Annotations "dashboard" }}">{{ index .Annotations "dashboard" }}</a><br>












{{ range .Alerts }}
  {{ if eq .Status "firing" }}
    <div style="background-color:#FFA500; color:white; padding:8px; font-weight:bold; border-radius:5px;">
      🚨 Alert Firing
    </div>
  {{ else if eq .Status "resolved" }}
    <div style="background-color:#32CD32; color:white; padding:8px; font-weight:bold; border-radius:5px;">
      ✅ Alert Resolved
    </div>
  {{ end }}

  <br>

  <table style="border-collapse:collapse;">
    <tr><td><b>Alert:</b></td><td>{{ .Labels.alertname }}</td></tr>
    <tr><td><b>Env:</b></td><td>{{ .Labels.env }}</td></tr>
    <tr><td><b>Host:</b></td><td>{{ .Labels.hostalias }}</td></tr>
    <tr><td><b>Severity:</b></td><td>{{ .Labels.severity }}</td></tr>
  </table>

  <br>

  <b>Details:</b>
  <ul>
    {{ if .Annotations.summary }}<li>{{ .Annotations.summary }}</li>{{ end }}
    {{ if .Annotations.description }}<li>{{ .Annotations.description }}</li>{{ end }}
  </ul>

  {{ if .Annotations.dashboard }}
    <a href="{{ .Annotations.dashboard }}" style="display:inline-block; padding:6px 12px; background:#1f78c1; color:white; text-decoration:none; border-radius:4px;">
      🔎 Open Dashboard
    </a>
  {{ end }}

  <hr>
{{ end }}








{{ range .Alerts }}
Status: {{ .Status }} | Alert: {{ .Labels.alertname }}
{{ end }}






{{ range .Alerts }}
<b>Status:</b> {{ .Status }}<br>
<b>Alert:</b> {{ .Labels.alertname }}<br>
<hr>
{{ end }}










{{ range .Alerts }}
{{ if eq .Status "firing" }}
<div style="background:#FFA500;color:#fff;padding:8px;font-weight:bold;border-radius:5px;">
  🚨 Alert Firing
</div>
{{ else if eq .Status "resolved" }}
<div style="background:#2E8B57;color:#fff;padding:8px;font-weight:bold;border-radius:5px;">
  ✅ Alert Resolved
</div>
{{ else }}
<div style="background:#808080;color:#fff;padding:8px;font-weight:bold;border-radius:5px;">
  ℹ️ Alert {{ .Status }}
</div>
{{ end }}

<b>Alert:</b> {{ .Labels.alertname }}<br>
<b>Env:</b> {{ .Labels.env }}<br>
<b>Host:</b> {{ .Labels.hostalias }}<br>
<b>Severity:</b> {{ .Labels.severity }}<br>

{{ if .Annotations.summary }}<b>Summary:</b> {{ .Annotations.summary }}<br>{{ end }}
{{ if .Annotations.description }}<b>Description:</b> {{ .Annotations.description }}<br>{{ end }}
{{ if .Annotations.dashboard }}
<a href="{{ .Annotations.dashboard }}">🔎 Open Dashboard</a><br>
{{ end }}

<hr>
{{ end }}








{{ range .Alerts }}
{{ if eq .Status "firing" }}
<b>🚨 Alert Firing</b>
{{ else if eq .Status "resolved" }}
<b>✅ Alert Resolved</b>
{{ else }}
<b>ℹ️ Alert {{ .Status }}</b>
{{ end }}

<b>Alert:</b> {{ .Labels.alertname }}<br>
<b>Env:</b> {{ .Labels.env }}<br>
<b>Host:</b> {{ .Labels.hostalias }}<br>
<b>Severity:</b> {{ .Labels.severity }}<br>

{{ if .Annotations.summary }}<b>Summary:</b> {{ .Annotations.summary }}<br>{{ end }}
{{ if .Annotations.description }}<b>Description:</b> {{ .Annotations.description }}<br>{{ end }}
{{ if .Annotations.dashboard }}<a href="{{ .Annotations.dashboard }}">🔎 Open Dashboard</a><br>{{ end }}

<hr>
{{ end }}

















