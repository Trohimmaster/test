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
