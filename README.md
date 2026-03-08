# 🎫 Sistema Inteligente de Asignación Automática de Tickets IT

Una aplicación web que clasifica y asigna automáticamente tickets IT a equipos especializados considerando tipo de solicitud, sistema afectado, prioridad y balanceo de carga.

## 📋 Características

✅ **Clasificación Automática**: Detecta sistema afectado, tipo de incidencia y prioridad  
✅ **Asignación Inteligente**: Selecciona el mejor agente según especialización y carga  
✅ **Balanceo de Carga**: Distribuye tickets considerando capacidad y experiencia  
✅ **Estimación de Tiempo**: Calcula tiempo de resolución según múltiples factores  
✅ **Confianza del Modelo**: Indica nivel de certeza en la asignación  
✅ **Respuesta JSON**: Formato estructurado para integración con otros sistemas  

## 🏗️ Estructura del Proyecto

```
Tarea 1/
├── ticket_assignment_app.py    # Backend (API Flask)
├── requirements.txt             # Dependencias Python
├── README.md                    # Este archivo
├── templates/
│   └── index.html              # Frontend HTML
└── static/
    ├── style.css               # Estilos CSS
    └── script.js               # JavaScript del cliente
```

## 🚀 Instalación y Ejecución

### Requisitos Previos
- Python 3.8+
- pip (gestor de paquetes Python)

### Pasos de Instalación

1. **Instalar dependencias:**
```bash
pip install -r requirements.txt
```

2. **Ejecutar la aplicación:**
```bash
python ticket_assignment_app.py
```

3. **Acceder a la aplicación:**
```
http://localhost:5000
```

## 📊 Equipos y Especialidades

| Equipo | Sistema(s) | Tipos | Agentes |
|--------|-----------|-------|---------|
| Equipo_GPS | Aplicacion C | Consultas de negocio, Soporte de contratos | Ana (exp: 5), Carlos (exp: 3) |
| Equipo_SMB | Aplicacion B | Consultas de negocio, Soporte de contratos, Error técnico, Configuración | Laura (exp: 4), Miguel (exp: 2) |
| Equipo_TECH | Aplicacion A, C | Error técnico, Configuración | Sofia (exp: 5), Daniel (exp: 3) |
| Equipo_TPS | Aplicacion A, B, C | Consultas de negocio, Soporte de contratos | Valeria (exp: 4), Andres (exp: 2) |

## 🔍 Algoritmo de Asignación

### 1. Detección
- **Sistema Afectado**: Identifica Aplicacion A, B o C
- **Tipo de Incidencia**: Clasifica en uno de 4 tipos
- **Prioridad**: Alta, Media o Baja
- **Confianza del Modelo**: 80-98%

### 2. Filtrado
Filtra equipos que:
- Soporten el sistema detectado
- Resuelvan el tipo de incidencia

### 3. Selección de Agente
- **Prioridad Alta**: Mayor experiencia + menor carga (máx. 80% capacidad)
- **Prioridad Media/Baja**: Menor carga relativa

### 4. Estimación de Tiempo
```
Tiempo Base = {Alta: 4h, Media: 8h, Baja: 16h}
Ajustes:
- Prioridad Alta: -20%
- Mayor experiencia: -5% por nivel
- Mayor carga: +50% por carga relativa
```

## 📡 Endpoints API

### POST /api/asignar-ticket
Asigna un tickets basado en su descripción.

**Request:**
```json
{
  "descripcion": "La Aplicacion A no carga, da error crítico",
  "adjuntos": "error_log.txt"
}
```

**Response:**
```json
{
  "sistema_afectado": "Aplicacion A",
  "tipo_incidencia": "Error técnico",
  "nivel_prioridad": "Alta",
  "Asignado al equipo": "Equipo_TECH",
  "Asignado a": "Sofia",
  "tiempo_estimado_horas": 3.2,
  "nivel_confianza_modelo": "92%"
}
```

### GET /api/equipos
Retorna información de todos los equipos.

### GET /api/tipos
Retorna tipos de solicitud disponibles.

### GET /api/sistemas
Retorna sistemas soportados.

## 💡 Ejemplos de Uso

### Ejemplo 1: Error Técnico de Alta Prioridad
```
Ticket: "La Aplicacion C está caída, los usuarios no pueden acceder. URGENTE"

Resultado:
- Sistema: Aplicacion C
- Tipo: Error técnico
- Prioridad: Alta
- Equipo: Equipo_TECH
- Agente: Sofia (mayor experiencia)
- Tiempo: ~3.2 horas
- Confianza: 95%
```

### Ejemplo 2: Consulta de Negocio Baja Prioridad
```
Ticket: "Necesito aclaración sobre el contrato de licencias de Aplicacion B"

Resultado:
- Sistema: Aplicacion B
- Tipo: Soporte de contratos
- Prioridad: Media
- Equipo: Equipo_SMB
- Agente: Laura (menor carga)
- Tiempo: ~8 horas
- Confianza: 88%
```

## 🎯 Palabras Clave Detectadas

### Prioridades
- **Alta**: urgente, crítico, emergencia, down, no funciona
- **Baja**: cuando puedas, sin prisa, cuando tengas tiempo
- **Media**: Por defecto si no se indica

### Sistemas
- Aplicacion A, Aplicacion B, Aplicacion C

### Tipos
- Consultas de negocio
- Soporte de contratos
- Error técnico
- Configuración

## ⚙️ Configuración

Puedes modificar:
- Equipos y sus especialidades en `EQUIPOS`
- Palabras clave de detección en `detectar_*()` functions
- Parámetros de cálculo de tiempo en `calcular_tiempo_estimado()`

## 🐛 Resolución de Problemas

**Puerto 5000 en uso:**
```bash
# Windows
netstat -ano | findstr :5000
taskkill /PID <PID> /F

# Linux/Mac
lsof -i :5000
kill -9 <PID>
```

**Error de módulo Flask:**
```bash
pip install --upgrade flask
```

## 📝 Notas

- La aplicación detecta automáticamente la información del ticket
- Si no se detecta algo, usa valores por defecto razonables
- El nivel de confianza indica qué tan clara fue la información del ticket
- La asignación considera experiencia Y carga actual del agente

## 📞 Soporte

Para reportar problemas o sugerencias, revisa la consola de Flask para los logs detallados.

---

**Version**: 1.0  
**Last Updated**: March 2, 2026  
**Status**: Production Ready ✅
