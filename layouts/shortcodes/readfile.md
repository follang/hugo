{{/* Get the filepath */}}
{{/* If the first character is "/", the path is from the site's `baseURL`. */}}
{{ if eq (.Get "file" | printf "%.1s") "/" }}

{{/* Use Hugo `readfile` behavior of path from site's `baseURL`. */}}
{{ $.Scratch.Set "filepath" ( .Get "file" ) }}

{{ else }}

{{/* Make relative: Fetch the current directory and then append it to the specified `file=""` value */}}
{{ $.Scratch.Set "filepath" .Page.Dir }}
{{ $.Scratch.Add "filepath" ( .Get "file" ) }}

{{ end }}


{{/* Check if the specified file exists */}}
{{ if fileExists ($.Scratch.Get "filepath") }}

{{/* If Code, then highlight with the specified language. */}}
{{ if eq (.Get "code") "true" }}

{{ highlight ($.Scratch.Get "filepath" | readFile | safeHTML ) (.Get "lang") "" }}

{{ else }}

{{/* If HTML or Markdown. For Markdown`{{%...%}}`,  don't send content to processor again (use safeHTML). */}}
{{ $.Scratch.Get "filepath" | readFile | safeHTML }}

{{ end }}

{{/* Say something if the file is not found and display the path that was specified in the shortcode (`file=" "`). */}}
{{ else }}

<p style="color: #D74848"><b><i>Something's not right. The <code>{{ .Get "file" }}</code> file was not found.</i></b></p>

{{ end }}
