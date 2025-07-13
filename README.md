# StellarLATAM
Stellar LATAM Hackathon 2025 Costa Rica


#  Donare+ — Donaciones con Confianza Viva  
*Trazabilidad transparente, ejecución verificable, impacto con rostro humano.*

---

##  Resumen del problema

Millones en donaciones se pierden cada año por corrupción, uso indebido o falta de visibilidad. Donantes (especialmente grandes) **no saben en qué se usó el dinero**, y comunidades receptoras **no tienen voz ni pruebas de que el apoyo realmente llegó**.

---

##  Nuestra solución

**Donare+** es una plataforma descentralizada de trazabilidad para donaciones grandes, construida sobre **Stellar + Soroban**, que transforma las donaciones en:

- **Fondos trazables**, liberados por etapas solo con evidencia válida.  
- **Historias reales y verificadas** que muestran el impacto humano.  
- **Votaciones comunitarias** que priorizan en qué usar el dinero.  
- **Auditoría abierta**, distribuida y transparente.

---

##  Características clave

---

###  1. Votación Comunitaria Trazable  
**Objetivo:** permitir que las comunidades beneficiarias voten directamente en qué se usa una donación, asegurando legitimidad, inclusión y registro on-chain.

####  Registro de votantes

| Escenario | Método | Validación |
|----------|--------|------------|
| ONG tiene lista de beneficiarios | Carga de base de datos | Se genera 1 token de participación por ID único |
| No hay registro digital previo | Registro presencial | Se recopila cédula + foto; se crea wallet o ID local |
| Área geográfica limitada | Geolocalización activa | App verifica GPS para permitir votar |
| Comunidad organizada | Validación social (proof-of-humanity) | 3 miembros ya validados confirman al nuevo votante |

 Resultado: cada votante recibe un **token VOTE-{proyecto_id}** asociado a su wallet.

---

####  Emisión del voto

1. El votante accede a una interfaz sencilla (web o móvil).  
2. Se le presentan entre **2 y 5 opciones** definidas por la ONG o comunidad (ej. "agua potable", "techos", "formación").  
3. El voto se emite firmando una transacción Stellar y **quemando el token de participación**.  
4. El contrato **Soroban** vinculado al proyecto **suma votos en tiempo real on-chain**.

 **Seguridad y control:**
- 1 voto por persona.  
- Votos anónimos pero auditables.  
- Tiempo límite de votación definido (por ejemplo, 48h).  
- Trazabilidad completa (número de votos, distribución, resultados).

---

##  2. Impacto con Rostro — Evidencia Humanizada

**Objetivo:** mostrar el resultado de cada entrega de ayuda mediante testimonios reales y verificados, conectando emocionalmente al donante con el impacto generado.

---

####  Flujo técnico

1. **Entrega registrada**  
   - La ONG marca una entrega como completada (con monto, fecha, descripción).  
   - Se genera un ID de entrega y queda vinculado a una transacción Stellar.

2. **Canal de evidencia abierto al beneficiario**  
   - El beneficiario recibe un enlace seguro o accede desde su wallet.  
   - Puede subir:  
     - Foto (ej. recibiendo el recurso)  
     - Video corto  
     - Audio o texto con testimonio

3. **Validación automática de evidencia**  
   - **GPS**: coordenadas del archivo o activación de ubicación.  
   - **Timestamp**: metadata del archivo verificada vs. período esperado.  
   - **Hash**: se genera hash SHA-256 del archivo y se guarda en Stellar.

   Si alguna validación falla → evidencia se marca como sospechosa.

---

####  Visualización para el donante

En su panel privado, el donante ve:

 San Salvador — 12/julio/2025
 Rosa M. recibió 3 dosis de insulina para su hijo Miguel.
 Ver foto — 🎤 Escuchar testimonio
 “Gracias a esta ayuda, mi hijo pudo recibir su tratamiento

Esto genera una experiencia emocional y verificable, mejor que cualquier PDF o correo de agradecimiento.

---

##  3. Liberación Condicionada de Fondos

**Objetivo:** garantizar que **los fondos solo se liberen si se entrega evidencia válida y verificable** de ejecución.

---

####  Estructura del contrato Soroban

- Al iniciar el proyecto, el donante define:
  - Monto total (ej. $10,000)
  - Número de tramos (ej. 3)
  - Condiciones de liberación por tramo:
    - Tipo de evidencia esperada
    - Fecha estimada
    - Lugar de ejecución

```soroban
struct Tramo {
  id: u32,
  monto: u32,
  liberado: bool,
  condiciones: CondicionesValidacion
}
```

#### Flujo de liberación

ONG completa una etapa y sube evidencia.

Backend verifica automáticamente:


| Validación | Herramienta       | Resultado esperado        |
|------------|-------------------|---------------------------|
|  Timestamp | Exif/metadata      | Fecha dentro del rango     |
|  Geolocalización | GPS activo o metadata | Dentro de zona válida       |
|  Hash      | SHA-256           | Archivo no alterado        |

Si todo es válido:

- Se ejecuta `liberar_tramo(n)` en el contrato.
- Se transfiere el monto del tramo a la wallet de la ONG.
- Se emite un evento on-chain:
Si todo es válido:

Se ejecuta liberar_tramo(n) en el contrato.

Se transfiere el monto del tramo a la wallet de la ONG.

Se emite un evento on-chain:

**Tramo 1 liberado — $3,000 — evidencia validada hash: 0xabc123**

---

## 4. Auditoría Comunitaria Distribuida  
Objetivo: permitir que ciudadanos, ONGs o periodistas verifiquen libremente la ejecución de entregas sin centralizar el poder.

---

#### Postulación de auditores  
Cualquier persona con identidad verificada puede postularse.

Se aprueba si cumple alguno de estos criterios:
 
- Historial de actividad en la plataforma  
- Votos positivos de la comunidad (DAO-like)  

Se le asigna un rol de auditor con una wallet identificada públicamente.

---

#### Permisos y funciones  
Pueden revisar:

- Evidencia subida por la ONG  
- Resultados de votación  
- Historias publicadas por beneficiarios  

Pueden emitir:

-  Validación aprobada  
-  Observación sospechosa  
-  Rechazo explícito (si pruebas son falsas)  

---

#### Gobernanza y reputación  
Cada acción del auditor queda registrada en Stellar:

- Auditor wallet_xyz validó evidencia #42 — aprobado  
- Auditor wallet_xyz recibió 1 voto negativo por omisión  

Reputación:

- Alta → puede auditar proyectos grandes  
- Baja → limitado a proyectos pequeños o suspensión temporal  

 Los contratos pueden requerir mínimo N validaciones independientes positivas para liberar fondos.

---

## Dashboard Final  
Muestra a cualquier visitante:

- Total donado / usado  
- Proyectos financiados  
- Historias por comunidad  
- Votos comunitarios  
- Auditorías realizadas  
- Trazabilidad de cada entrega
  
