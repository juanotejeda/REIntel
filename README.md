# REIntel - Threat Intelligence Automation Workflows

![Version](https://img.shields.io/badge/version-0.1.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-n8n-FF6D5A)
![Security](https://img.shields.io/badge/security-threat%20intel-red)

Colección de workflows de automatización para inteligencia de amenazas y operaciones de seguridad usando n8n. Análisis automatizado de URLs, dominios, IPs y, próximamente, hashes y otros IOCs. 

**Proyecto Open Source de la comunidad Remote Execution (#RE)**

---

## ✨ Características principales

- 🔍 **Análisis automatizado**: Verificación de URLs, dominios e IPs sospechosas.
- 🌐 **Múltiples fuentes**: Integración con VirusTotal y AbuseIPDB para enriquecer IOCs.
- 📊 **Respuesta estructurada**: JSON pensado para SOC / SIEM, con score y veredicto claros.
- 🚀 **Fácil implementación**: Workflows listos para importar en n8n (self-hosted o cloud).
- 🔄 **Escalable**: Diseño preparado para agregar nuevos workflows (hashes, phishing, etc.).
- 🎓 **Educativo**: Ideal para aprender automatización en ciberseguridad y conceptos de SOAR.

---

## 📦 Workflows disponibles

### 1. URL/Domain Analyzer (v0.1)

**Archivo:** `workflows/url-domain-analyzer.json`

**Descripción:** Analiza URLs y dominios sospechosos consultando múltiples fuentes de threat intelligence y devolviendo un JSON estructurado listo para automatización.

**Componentes:**

- Webhook de entrada para recibir URLs/dominios.
- Resolución DNS usando Cloudflare DNS-over-HTTPS.
- Consulta a VirusTotal API para reputación de dominio. 
- Consulta a AbuseIPDB para verificar si la IP está reportada como maliciosa. 
- Cálculo de `threat_score`, nivel de amenaza y recomendación.
- Respuesta JSON con secciones: `meta`, `input`, `verdict`, `sources`.

**Estructura de la respuesta:**

- `meta`: información del análisis (quién, cuándo).
- `input`: datos de entrada normalizados (URL original, dominio, IP).
- `verdict`: score, nivel de riesgo y acción recomendada.
- `sources`: detalle por fuente (VirusTotal, AbuseIPDB).

**APIs necesarias:**

- VirusTotal API (gratuita): https://www.virustotal.com/gui/join-us
- AbuseIPDB API (gratuita): https://www.abuseipdb.com/register 
---

## 🚀 Instalación rápida

### Requisitos previos

- n8n instalado (Docker, npm o n8n Cloud).
- Cuenta gratuita en VirusTotal.
- Cuenta gratuita en AbuseIPDB.
- API keys de ambos servicios.

### Instalación con Docker (recomendado)
```bash
# Crear directorio para n8n
mkdir -p ~/.n8n

# Ejecutar n8n con Docker
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -e N8N_SECURE_COOKIE=false \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

### Instalación con npm

```bash
npm install n8n -g
n8n start
```

Accede a n8n en: http://localhost:5678

---

## 📖 Uso Rápido

### 1. Importar Workflow

1. Clona este repositorio:
```bash
git clone https://github.com/juanotejeda/REIntel.git
cd REIntel
```

2. En n8n, ve a **Workflows → Import from File**
3. Selecciona el archivo `workflows/url-domain-analyzer.json`
4. Haz clic en **Import**

### 2. Configurar Credenciales

1. Abre el workflow importado
2. Configura las credenciales de las APIs:
   - **VirusTotal**: Añade tu API key en el nodo HTTP Request
   - **AbuseIPDB**: Añade tu API key en el nodo HTTP Request
3. Guarda el workflow

### 3. Activar y Probar

1. Activa el workflow (toggle en la esquina superior derecha)
2. Copia la URL del webhook
3. Envía una petición POST:

```bash
curl -X POST https://tu-instancia-n8n.com/webhook/url-analyzer \
  -H "Content-Type: application/json" \
  -d '{"url": "https://ejemplo-sospechoso.com"}'
```

---

## 🧪 Uso desde terminal con jq

Para ver la respuesta de forma legible en consola:

```bash
curl -s http://localhost:5678/webhook/url-analyzer \
  -H "Content-Type: application/json" \
  -d '{"url": "google.com"}' | jq .
```


Instalación de `jq` (Debian/Ubuntu/MX Linux):

```bash
sudo apt install -y jq
```

jq permite pretty-print y filtrado de JSON desde línea de comandos.


4. Revisa el reporte generado

---

## 🔍 Workflows en Detalle

### URL/Domain Analyzer - Flujo de Trabajo

```
Webhook (Entrada)
    ↓
Validación de URL
    ↓
Resolución DNS → Obtiene IP del dominio
    ↓
Consulta VirusTotal → Análisis de reputación
    ↓
Consulta AbuseIPDB → Verificación de IP
    ↓
Agregación de Datos → Calcula score de amenaza
    ↓
Generación de Reporte (JSON)
	↓
Code → reestructura JSON (meta / input / verdict / sources)
	↓
Respond to Webhook → devuelve respuesta JSON	
```

**Ejemplo de Reporte:**

```json
{
"meta": {
"analyzed_by": "REIntel - Remote Execution Community",
"analysis_timestamp": "2025-12-15T18:21:10.266Z"
},
"input": {
"original": "google.com",
"domain": "google.com",
"ip_address": "142.251.134.78"
},
"verdict": {
"threat_score": 1,
"threat_level": "LOW",
"recommendation": "ALLOW"
},
"sources": {
"virustotal": {
"malicious": 1,
"suspicious": 0,
"harmless": 66,
"undetected": 28,
"total_scanners": 95
},
"abuseipdb": {
"abuse_confidence_score": 0,
"total_reports": 0,
"is_whitelisted": false,
"last_reported": null
}
}
}
```
Workflow probado en n8n 2.0.2 (self-hosted con Docker) usando las APIs públicas de VirusTotal y AbuseIPDB.
---

## 🗺️ Roadmap

### Versión 0.2.0 (Próxima)

- [ ] Workflow de análisis de hashes (MD5, SHA1, SHA256)
- [ ] Integración con Shodan para análisis de IPs
- [ ] Workflow de monitoreo de phishing en tiempo real
- [ ] Dashboard web para visualización de análisis

### Versión 0.3.0 (Futuro)

- [ ] Integración con MISP (Malware Information Sharing Platform)
- [ ] Análisis de archivos sospechosos
- [ ] Notificaciones a Slack/Discord/Telegram
- [ ] Base de datos local para histórico de análisis

---

## 🛠️ Estructura del Proyecto

```
REIntel/
├── README.md
├── LICENSE
├── workflows/
│   ├── url-domain-analyzer.json
│   └── (próximamente más workflows)
├── docs/
│   ├── setup-guide.md
│   ├── api-keys.md
│   └── troubleshooting.md
└── examples/
    ├── sample-requests.json
    └── sample-reports.json
```

---

## 📚 Documentación Adicional

- [Guía de Configuración Completa](docs/setup-guide.md) - Próximamente
- [Cómo Obtener API Keys](docs/api-keys.md) - Próximamente
- [Solución de Problemas](docs/troubleshooting.md) - Próximamente

---

## 🤝 Contribuir

¡Las contribuciones son bienvenidas! Si tienes ideas para nuevos workflows o mejoras:

1. Fork el proyecto
2. Crea una rama para tu feature (`git checkout -b feature/nuevo-workflow`)
3. Commit tus cambios (`git commit -m 'Add: nuevo workflow de análisis'`)
4. Push a la rama (`git push origin feature/nuevo-workflow`)
5. Abre un Pull Request

---

## 📄 Licencia

Este proyecto está bajo la licencia MIT. Ver archivo LICENSE para más detalles.

---

## 🔗 Proyectos Relacionados

- [REStrike](https://github.com/juanotejeda/REStrike) - Herramienta de pentesting visual en Go

---

## 📞 Soporte

- Issues: https://github.com/juanotejeda/REIntel/issues
- Discussions: https://github.com/juanotejeda/REIntel/discussions
- Email: juanotejeda@gmail.com

---

**Comunidad Remote Execution (#RE)** | Automatización de Seguridad con n8n
