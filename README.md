
## 📋 Descripción del Proyecto

**FinanceHub** es una plataforma integral de banca digital que permite a los usuarios gestionar sus finanzas personales y empresariales de manera eficiente. El sistema incluye funcionalidades avanzadas como:

- Gestión completa de cuentas bancarias (corrientes, ahorro, empresariales)
- Procesamiento de transacciones en tiempo real (SPEI, transferencias internacionales)
- Administración de tarjetas de crédito y débito
- Plataforma de inversiones (acciones, bonos, CETES, FIBRAs)
- Análisis de riesgo crediticio y detección de fraudes
- Reportes regulatorios y compliance financiero

El proyecto simula un entorno bancario real con datos coherentes que permiten realizar análisis financieros, detección de patrones sospechosos, segmentación de clientes y generación de reportes operativos.

## 🗄️ Estructura del Repositorio

```
financehub-mongodb/
│
├── README.md
├── clientes.json
├── cuentas.json
├── transacciones.json
├── tarjetas.json
└── inversiones.json
└── Consultas.md
```

## 🚀 Configuración de la Base de Datos

### Paso 1: Crear la base de datos en MongoDB

```javascript
// Conectar a MongoDB
mongosh

// Crear y usar la base de datos
use financehub

// Verificar que la base de datos fue creada
db.getName()
```

### Paso 2: Importar las colecciones

Ejecuta los siguientes comandos en tu terminal (no en mongosh):

```bash
# Importar clientes (datos de clientes del banco)
mongoimport --db financehub --collection clientes --file clientes.json --jsonArray

# Importar cuentas (productos bancarios y cuentas)
mongoimport --db financehub --collection cuentas --file cuentas.json --jsonArray

# Importar transacciones (historial de movimientos)
mongoimport --db financehub --collection transacciones --file transacciones.json --jsonArray

# Importar tarjetas (tarjetas de crédito y débito)
mongoimport --db financehub --collection tarjetas --file tarjetas.json --jsonArray

# Importar inversiones (portafolio de inversiones)
mongoimport --db financehub --collection inversiones --file inversiones.json --jsonArray
```

### Paso 3: Verificar la importación

```javascript
// Verificar que las colecciones fueron creadas
show collections

// Contar documentos en cada colección
db.clientes.countDocuments()
db.cuentas.countDocuments()
db.transacciones.countDocuments()
db.tarjetas.countDocuments()
db.inversiones.countDocuments()
```

## Consultas con Expresiones Regulares

### **CLIENTES - Gestión y Segmentación de Clientes**

#### **Consulta 1:** Buscar clientes con nombres que empiecen con "Mar"
```javascript
db.clientes.find({ "nombre": { "$regex": "^Mar", "$options": "i" } })
```
**Utilidad:** Permite implementar funcionalidades de autocompletar en formularios de búsqueda de clientes y campañas de marketing personalizado por nombre.

#### **Consulta 2:** Encontrar clientes con emails corporativos
```javascript
db.clientes.find({ "email": { "$regex": "@(empresa|corp|company|inc)\\.com$", "$options": "i" } })
```
**Utilidad:** Segmentación automática para identificar clientes empresariales y ofrecerles productos de banca corporativa específicos.

#### **Consulta 3:** Buscar clientes con RFC mexicano válido
```javascript
db.clientes.find({ "rfc": { "$regex": "^[A-Z]{4}[0-9]{6}[A-Z0-9]{3}$" } })
```
**Utilidad:** Validación fiscal automática para cumplimiento tributario y verificación de datos ante el SAT mexicano.

#### **Consulta 4:** Clientes con teléfonos celulares mexicanos (+52)
```javascript
db.clientes.find({ "telefono": { "$regex": "^\\+52", "$options": "i" } })
```
**Utilidad:** Segmentación geográfica para campañas SMS locales y verificación de números válidos para autenticación móvil.

#### **Consulta 5:** Buscar clientes premium en ciudades que terminen en "dad"
```javascript
db.clientes.find({ 
  "direccion.ciudad": { "$regex": "dad$", "$options": "i" },
  "segmento": "premium"
})
```
**Utilidad:** Targeting específico para promociones exclusivas en grandes urbes como "Ciudad de México", "Guadalajara", etc.

### **CUENTAS - Análisis de Productos Bancarios**

#### **Consulta 6:** Cuentas con números que empiecen con "4001" (tarjetas Visa)
```javascript
db.cuentas.find({ "numero_cuenta": { "$regex": "^4001" } })
```
**Utilidad:** Identificación rápida de productos específicos para análisis de portafolio y reportes por emisor de tarjetas.

#### **Consulta 7:** Buscar cuentas con códigos SWIFT específicos
```javascript
db.cuentas.find({ "codigo_swift": { "$regex": "^(BBVA|BNAM|BNMX)", "$options": "i" } })
```
**Utilidad:** Generación de reportes de corresponsalía bancaria y análisis de transferencias internacionales por banco.

#### **Consulta 8:** Cuentas de ahorro con saldos mayores a $100,000
```javascript
db.cuentas.find({ 
  "tipo_cuenta": "ahorro",
  "saldo": { "$gt": 100000 }
})
```
**Utilidad:** Identificación de clientes de alto valor para ofertas de inversión y productos premium de Private Banking.

#### **Consulta 9:** Cuentas con status que contenga "activ"
```javascript
db.cuentas.find({ "status": { "$regex": "activ", "$options": "i" } })
```
**Utilidad:** Filtrado operativo para reportes de cuentas vigentes y cálculo de cartera activa del banco.

#### **Consulta 10:** Buscar cuentas abiertas en 2024
```javascript
db.cuentas.find({ "fecha_apertura": { "$regex": "^2024" } })
```
**Utilidad:** Análisis de crecimiento anual y medición de efectividad en campañas de adquisición de clientes nuevos.

### **TRANSACCIONES - Análisis de Movimientos y Fraudes**

#### **Consulta 11:** Transacciones con referencias que contengan "PAGO" o "PAY"
```javascript
db.transacciones.find({ "referencia": { "$regex": "(PAGO|PAY)", "$options": "i" } })
```
**Utilidad:** Categorización automática de transacciones para análisis de flujo de pagos y generación de reportes contables.

#### **Consulta 12:** Buscar transferencias SPEI (con clave de rastreo específica)
```javascript
db.transacciones.find({ "clave_rastreo": { "$regex": "^[0-9]{18}$" } })
```
**Utilidad:** Auditoría de transferencias interbancarias y seguimiento de operaciones SPEI para reportes regulatorios ante BANXICO.

#### **Consulta 13:** Transacciones con montos exactos (sin centavos)
```javascript
db.transacciones.find({ "monto": { "$regex": "\\.00$" } })
```
**Utilidad:** Detección de patrones sospechosos en transacciones con montos redondos que podrían indicar actividades fraudulentas.

#### **Consulta 14:** Buscar transacciones nocturnas (entre 10 PM y 6 AM)
```javascript
db.transacciones.find({ "timestamp": { "$regex": "(2[2-3]:|0[0-5]:)", "$options": "i" } })
```
**Utilidad:** Sistema de alertas para detección de fraudes basado en horarios inusuales de transacciones.

#### **Consulta 15:** Transacciones con descripción que mencione "OXXO" o "7ELEVEN"
```javascript
db.transacciones.find({ "descripcion": { "$regex": "(OXXO|7ELEVEN|ELEVEN)", "$options": "i" } })
```
**Utilidad:** Análisis de comportamiento de gastos en tiendas de conveniencia para ofertas de cashback y programas de lealtad.

### **TARJETAS - Gestión y Seguridad**

#### **Consulta 16:** Tarjetas Visa (que empiecen con 4)
```javascript
db.tarjetas.find({ "numero_tarjeta": { "$regex": "^4[0-9]{3}\\s[*]{4}\\s[*]{4}\\s[0-9]{4}$" } })
```
**Utilidad:** Segmentación por emisor para reportes específicos de Visa y análisis de productos por marca.

#### **Consulta 17:** Tarjetas que vencen en 2024
```javascript
db.tarjetas.find({ "fecha_vencimiento": { "$regex": "/24$" } })
```
**Utilidad:** Automatización de campañas de renovación de tarjetas y prevención de expiración de productos activos.

#### **Consulta 18:** Buscar tarjetas con límites altos (más de $50,000)
```javascript
db.tarjetas.find({ "limite_credito": { "$gt": 50000 } })
```
**Utilidad:** Identificación de clientes premium para ofertas de productos exclusivos y análisis de exposición crediticia.

#### **Consulta 19:** Tarjetas con CVV que empiece con "1" o "2"
```javascript
db.tarjetas.find({ "cvv": { "$regex": "^[12]" } })
```
**Utilidad:** Análisis estadístico de distribución de códigos de seguridad para auditorías internas (solo uso administrativo).

#### **Consulta 20:** Buscar tarjetas bloqueadas temporalmente
```javascript
db.tarjetas.find({ "status": { "$regex": "bloqueada|suspended", "$options": "i" } })
```
**Utilidad:** Gestión de incidencias de seguridad y reportes de tarjetas con restricciones para seguimiento operativo.

### **INVERSIONES - Análisis de Portafolios**

#### **Consulta 21:** Inversiones en acciones mexicanas (que terminen en .MX)
```javascript
db.inversiones.find({ "ticker": { "$regex": "\\.MX$", "$options": "i" } })
```
**Utilidad:** Análisis de exposición al mercado local mexicano y diversificación geográfica de portafolios de clientes.

#### **Consulta 22:** Buscar inversiones con rendimiento positivo
```javascript
db.inversiones.find({ "rendimiento": { "$regex": "^\\+", "$options": "i" } })
```
**Utilidad:** Identificación de inversiones exitosas para reportes de performance y recomendaciones a otros clientes.

#### **Consulta 23:** Inversiones en fondos que contengan "FIBRA" o "REIT"
```javascript
db.inversiones.find({ "nombre_instrumento": { "$regex": "(FIBRA|REIT)", "$options": "i" } })
```
**Utilidad:** Análisis sectorial de inversiones inmobiliarias y diversificación por clase de activo en portafolios.

#### **Consulta 24:** Buscar inversiones compradas en horario de apertura (9:00-9:59 AM)
```javascript
db.inversiones.find({ "fecha_compra": { "$regex": "09:[0-5][0-9]", "$options": "i" } })
```
**Utilidad:** Análisis de comportamiento de trading para identificar patrones de inversión de clientes activos en el mercado.

#### **Consulta 25:** Inversiones con estrategia conservadora o moderada
```javascript
db.inversiones.find({ "estrategia": { "$regex": "(conservadora|moderada)", "$options": "i" } })
```
**Utilidad:** Segmentación de clientes por perfil de riesgo para recomendaciones personalizadas y compliance regulatorio.

##  Casos de Uso del Sistema

### **Detección de Fraudes**
- Monitoreo de transacciones nocturnas
- Identificación de patrones sospechosos en montos
- Alertas automáticas por comportamiento anómalo

### **Cumplimiento Regulatorio**
- Validación de RFC y datos fiscales
- Reportes SPEI para BANXICO
- Análisis de transferencias internacionales

### **Marketing y CRM**
- Segmentación de clientes por perfil
- Campañas personalizadas por productos
- Análisis de comportamiento de gastos

### **Gestión de Riesgos**
- Evaluación de portafolios de inversión
- Análisis crediticio por segmento
- Monitoreo de exposición por mercado

### **Análisis Operativo**
- Reportes de crecimiento de cuentas
- Performance de productos bancarios
- Análisis de rentabilidad por cliente

---

**Desarrollado por:** jersson esteban fuentes parra 
