# 25 Consultas MongoDB con Expresiones Regulares 


### **Consulta 1:** Buscar clientes con nombres que empiecen con "Mar"
```javascript
db.clientes.find({ "nombre": { "$regex": "^Mar", "$options": "i" } })
```

### **Consulta 2:** Encontrar clientes con emails corporativos
```javascript
db.clientes.find({ "email": { "$regex": "@(empresa|corp|company|inc)\\.com$", "$options": "i" } })
```

### **Consulta 3:** Buscar clientes con RFC mexicano válido
```javascript
db.clientes.find({ "rfc": { "$regex": "^[A-Z]{4}[0-9]{6}[A-Z0-9]{3}$" } })
```

### **Consulta 4:** Clientes con teléfonos celulares mexicanos (+52)
```javascript
db.clientes.find({ "telefono": { "$regex": "^\\+52", "$options": "i" } })
```

### **Consulta 5:** Buscar clientes premium en ciudades que terminen en "dad"
```javascript
db.clientes.find({ 
  "direccion.ciudad": { "$regex": "dad$", "$options": "i" },
  "segmento": "premium"
})
```

---



### **Consulta 6:** Cuentas con números que empiecen con "4001" (tarjetas Visa)
```javascript
db.cuentas.find({ "numero_cuenta": { "$regex": "^4001" } })
```

### **Consulta 7:** Buscar cuentas con códigos SWIFT específicos
```javascript
db.cuentas.find({ "codigo_swift": { "$regex": "^(BBVA|BNAM|BNMX)", "$options": "i" } })
```

### **Consulta 8:** Cuentas de ahorro con saldos mayores a $100,000
```javascript
db.cuentas.find({ 
  "tipo_cuenta": "ahorro",
  "saldo": { "$regex": "^[1-9][0-9]{5,}" }
})
```

### **Consulta 9:** Cuentas con status que contenga "activ"
```javascript
db.cuentas.find({ "status": { "$regex": "activ", "$options": "i" } })
```

### **Consulta 10:** Buscar cuentas abiertas en 2024
```javascript
db.cuentas.find({ "fecha_apertura": { "$regex": "^2024" } })
```

---

### **Consulta 11:** Transacciones con referencias que contengan "PAGO" o "PAY"
```javascript
db.transacciones.find({ "referencia": { "$regex": "(PAGO|PAY)", "$options": "i" } })
```

### **Consulta 12:** Buscar transferencias SPEI (con clave de rastreo específica)
```javascript
db.transacciones.find({ "clave_rastreo": { "$regex": "^[0-9]{18}$" } })
```

### **Consulta 13:** Transacciones con montos exactos (sin centavos)
```javascript
db.transacciones.find({ "monto": { "$regex": "\\.00$" } })
```

### **Consulta 14:** Buscar transacciones nocturnas (entre 10 PM y 6 AM)
```javascript
db.transacciones.find({ "timestamp": { "$regex": "(2[2-3]:|0[0-5]:)", "$options": "i" } })
```

### **Consulta 15:** Transacciones con descripción que mencione "OXXO" o "7ELEVEN"
```javascript
db.transacciones.find({ "descripcion": { "$regex": "(OXXO|7ELEVEN|ELEVEN)", "$options": "i" } })
```

---



### **Consulta 16:** Tarjetas Visa (que empiecen con 4)
```javascript
db.tarjetas.find({ "numero_tarjeta": { "$regex": "^4[0-9]{3}\\s[*]{4}\\s[*]{4}\\s[0-9]{4}$" } })
```

### **Consulta 17:** Tarjetas que vencen en 2024
```javascript
db.tarjetas.find({ "fecha_vencimiento": { "$regex": "/24$" } })
```

### **Consulta 18:** Buscar tarjetas con límites altos (más de $50,000)
```javascript
db.tarjetas.find({ "limite_credito": { "$regex": "^[5-9][0-9]{4,}|^[1-9][0-9]{5,}" } })
```

### **Consulta 19:** Tarjetas con CVV que empiece con "1" o "2"
```javascript
db.tarjetas.find({ "cvv": { "$regex": "^[12]" } })
```

### **Consulta 20:** Buscar tarjetas bloqueadas temporalmente
```javascript
db.tarjetas.find({ "status": { "$regex": "bloqueada|suspended", "$options": "i" } })
```

---


### **Consulta 21:** Inversiones en acciones mexicanas (que terminen en .MX)
```javascript
db.inversiones.find({ "ticker": { "$regex": "\\.MX$", "$options": "i" } })
```

### **Consulta 22:** Buscar inversiones con rendimiento positivo
```javascript
db.inversiones.find({ "rendimiento": { "$regex": "^\\+", "$options": "i" } })
```

### **Consulta 23:** Inversiones en fondos que contengan "FIBRA" o "REIT"
```javascript
db.inversiones.find({ "nombre_instrumento": { "$regex": "(FIBRA|REIT)", "$options": "i" } })
```

### **Consulta 24:** Buscar inversiones compradas en horario de apertura (9:00-9:59 AM)
```javascript
db.inversiones.find({ "fecha_compra": { "$regex": "09:[0-5][0-9]", "$options": "i" } })
```

### **Consulta 25:** Inversiones con estrategia conservadora o moderada
```javascript
db.inversiones.find({ "estrategia": { "$regex": "(conservadora|moderada)", "$options": "i" } })
```

---


### Para ejecutar todas las consultas:
1. **Conectar a MongoDB:**
   ```bash
   mongosh
   use financehub
   ```

2. **Ejecutar consultas individualmente:**
   ```javascript
   // Ejemplo: Consulta 1
   db.clientes.find({ "nombre": { "$regex": "^Mar", "$options": "i" } })
   ```

3. **Ver resultados formateados:**
   ```javascript
   db.clientes.find({ "nombre": { "$regex": "^Mar", "$options": "i" } }).pretty()
   ```

### Modificadores importantes:
- `"$options": "i"` = Case insensitive (no distingue mayúsculas/minúsculas)
- `^` = Inicio de cadena
- `$` = Final de cadena
- `\\+` = Escape para el símbolo +
- `[0-9]` = Cualquier dígito
- `[A-Z]` = Cualquier letra mayúscula
- `{6}` = Exactamente 6 caracteres
- `{4,}` = 4 o más caracteres
- `|` = OR lógico