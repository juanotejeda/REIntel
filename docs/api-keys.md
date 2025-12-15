# Guía para Obtener API Keys

Esta guía te ayudará a obtener las API keys necesarias para usar los workflows de REIntel.

---

## VirusTotal API Key

**Servicio:** Análisis de malware y reputación de URLs/dominios
**Plan gratuito:** 4 peticiones por minuto, 500 por día

### Pasos para obtener la API key:

1. Ve a [VirusTotal Join Us](https://www.virustotal.com/gui/join-us)
2. Crea una cuenta (puedes usar Google, GitHub o email)
3. Verifica tu email si es necesario
4. Una vez logueado, ve a tu perfil (esquina superior derecha)
5. Click en **API Key** en el menú lateral
6. Copia tu API key (formato: `64 caracteres alfanuméricos`)

### Configuración en n8n:

1. En el workflow, encuentra el nodo **VirusTotal Check**
2. Click en el nodo para editarlo
3. En la sección **Headers**, encuentra el parámetro `x-apikey`
4. Reemplaza `YOUR_VIRUSTOTAL_API_KEY` con tu API key real
5. Guarda el workflow

**Ejemplo:**
```
Header Name: x-apikey
Header Value: 1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef
```

---

## AbuseIPDB API Key

**Servicio:** Base de datos de IPs maliciosas reportadas
**Plan gratuito:** 1,000 peticiones por día

### Pasos para obtener la API key:

1. Ve a [AbuseIPDB Register](https://www.abuseipdb.com/register)
2. Crea una cuenta con tu email
3. Verifica tu email
4. Una vez logueado, ve a [API](https://www.abuseipdb.com/api) en el menú superior
5. Click en **Create Key**
6. Dale un nombre a tu key (ej: "REIntel Workflow")
7. Copia tu API key (formato: `80 caracteres alfanuméricos`)

### Configuración en n8n:

1. En el workflow, encuentra el nodo **AbuseIPDB Check**
2. Click en el nodo para editarlo
3. En la sección **Headers**, encuentra el parámetro `Key`
4. Reemplaza `YOUR_ABUSEIPDB_API_KEY` con tu API key real
5. Guarda el workflow

**Ejemplo:**
```
Header Name: Key
Header Value: 1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef
```

---

## Cloudflare DNS (No requiere API key)

El servicio DNS-over-HTTPS de Cloudflare es completamente gratuito y no requiere autenticación.

**Endpoint usado:** `https://cloudflare-dns.com/dns-query`

No necesitas configurar nada adicional para este servicio.

---

## Mejores Prácticas de Seguridad

### 1. No expongas tus API keys

- ❌ **NUNCA** subas workflows con API keys reales a GitHub
- ❌ **NUNCA** compartas tus API keys en foros o chat público
- ✅ Usa variables de entorno en n8n para almacenar keys
- ✅ Usa las credenciales de n8n (más seguro)

### 2. Usar Credenciales de n8n (Recomendado)

En lugar de poner las API keys directamente en los headers:

1. Ve a **Settings → Credentials** en n8n
2. Click en **Add Credential**
3. Busca **Header Auth** o **API Key Auth**
4. Crea una credencial con tu API key
5. En el nodo HTTP Request, selecciona tu credencial guardada

### 3. Monitorea el uso de tus APIs

- **VirusTotal:** Revisa tu [Dashboard](https://www.virustotal.com/gui/user/apikey) regularmente
- **AbuseIPDB:** Revisa tu [API Usage](https://www.abuseipdb.com/account/api) regularmente

### 4. Límites de Rate

Si excedes los límites, recibirás errores:

- **VirusTotal:** HTTP 204 (sin contenido) o 429 (demasiadas peticiones)
- **AbuseIPDB:** HTTP 429 con mensaje de error

**Solución:** Agrega nodos de delay/espera entre peticiones masivas

---

## Verificar que tus API keys funcionan

### Test rápido con curl:

**VirusTotal:**
```bash
curl --request GET \
  --url 'https://www.virustotal.com/api/v3/domains/google.com' \
  --header 'x-apikey: TU_API_KEY_AQUI'
```

**AbuseIPDB:**
```bash
curl --request GET \
  --url 'https://api.abuseipdb.com/api/v2/check?ipAddress=8.8.8.8' \
  --header 'Key: TU_API_KEY_AQUI' \
  --header 'Accept: application/json'
```

Si recibes respuestas JSON con datos, ¡tus keys funcionan correctamente!

---

## Troubleshooting

### Error: "Invalid API key"

- Verifica que copiaste la key completa (sin espacios al inicio/final)
- Asegúrate de usar el header correcto (`x-apikey` para VT, `Key` para AbuseIPDB)
- Regenera la API key si es necesario

### Error: "Rate limit exceeded"

- Espera unos minutos antes de hacer más peticiones
- Considera actualizar a un plan pago si necesitas más requests
- Implementa caching para no consultar URLs ya analizadas

### Error: "Unauthorized" o "403 Forbidden"

- Verifica que tu cuenta esté activa y verificada
- Revisa que no hayas violado los términos de servicio
- Contacta al soporte del servicio si el problema persiste

---

## Soporte

Si tienes problemas obteniendo o configurando las API keys:

- Abre un [Issue en GitHub](https://github.com/juanotejeda/REIntel/issues)
- Únete a las [Discussions](https://github.com/juanotejeda/REIntel/discussions)
- Contacta: juanotejeda@gmail.com

**¡Nunca compartas tus API keys al pedir ayuda!**
