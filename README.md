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
