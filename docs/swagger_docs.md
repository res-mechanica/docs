
<link type="text/css" rel="stylesheet" href="https://cdn.jsdelivr.net/npm/swagger-ui-dist@3/swagger-ui.css">
<link rel="shortcut icon" href="https://fastapi.tiangolo.com/img/favicon.png">

<div id="swagger-ui">
</div>
<script src="https://cdn.jsdelivr.net/npm/swagger-ui-dist@3/swagger-ui-bundle.js"></script>
<!-- `SwaggerUIBundle` is now available on the page -->
<script>
const ui = SwaggerUIBundle({
    url: 'https://api.goodmoves.ai/v3/openapi.json',
oauth2RedirectUrl: window.location.origin + '/docs/oauth2-redirect',
    dom_id: '#swagger-ui',
    presets: [
    SwaggerUIBundle.presets.apis,
    SwaggerUIBundle.SwaggerUIStandalonePreset
    ],
    layout: "BaseLayout",
    deepLinking: true,
    showExtensions: true,
    showCommonExtensions: true
})
</script>
