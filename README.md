# REIntel - Threat Intelligence Automation Workflows

![Version](https://img.shields.io/badge/version-0.1.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-n8n-FF6D5A)
![Security](https://img.shields.io/badge/security-threat%20intel-red)

Colección de workflows de automatización para inteligencia de amenazas y operaciones de seguridad usando n8n. Análisis automatizado de URLs, dominios, IPs y más.

**Proyecto Open Source de la comunidad Remote Execution (#RE)**

---

## ✨ Características Principales

- 🔍 **Análisis Automatizado**: Verificación de URLs, dominios e IPs sospechosas
- 🌐 **Múltiples Fuentes**: Integración con VirusTotal, AbuseIPDB y más
- 📊 **Reportes Consolidados**: Genera informes con score de amenaza
- 🚀 **Fácil Implementación**: Workflows listos para importar en n8n
- 🔄 **Escalable**: Añade nuevas fuentes de threat intel fácilmente
- 🎓 **Educativo**: Ideal para aprender automatización en ciberseguridad
- 💾 **Gratuito**: Usa APIs gratuitas sin costos ocultos

---

## 📦 Workflows Disponibles

### 1. URL/Domain Analyzer (v0.1)
**Archivo:** `workflows/url-domain-analyzer.json`

**Descripción:** Analiza URLs y dominios sospechosos consultando múltiples fuentes de threat intelligence.

**Componentes:**
- Webhook de entrada para recibir URLs/dominios
- Resolución DNS usando Cloudflare DNS-over-HTTPS
- Consulta a VirusTotal API (análisis de reputación)
- Consulta a AbuseIPDB (verificación de IP maliciosa)
- Generación de reporte consolidado con score de amenaza

**APIs Necesarias:**
- VirusTotal API (gratuita): https://www.virustotal.com/gui/join-us
- AbuseIPDB API (gratuita): https://www.abuseipdb.com/register

---

## 🚀 Instalación Rápida

### Requisitos Previos

- n8n instalado (Docker, npm o cloud)
- Cuentas gratuitas en VirusTotal y AbuseIPDB
- API keys de los servicios mencionados

### Instalación con Docker (Recomendado)

```bash
# Crear directorio para n8n
mkdir -p ~/.n8n

# Ejecutar n8n con Docker
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
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
```

**Ejemplo de Reporte:**

```json
{
  "analyzed_url": "https://malicious-site.com",
  "domain": "malicious-site.com",
  "ip_address": "192.0.2.1",
  "threat_score": 85,
  "threat_level": "HIGH",
  "virustotal": {
    "detections": 45,
    "total_scanners": 70,
    "malicious_votes": 12
  },
  "abuseipdb": {
    "abuse_confidence": 98,
    "reports": 234,
    "last_reported": "2025-12-14"
  },
  "recommendation": "BLOCK",
  "timestamp": "2025-12-15T13:29:00Z"
}
```

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
