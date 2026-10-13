# Dominio: Biometric

> **Propietario:** Contexto delimitado Biometric
> **Motor de BD:** MongoDB 7 para embeddings + una tabla SQL de flujo (`biometric_update_case` alojada en `configuration`)
>
> **Justificación del motor:**
> - Los embeddings faciales y de huella son vectores de longitud variable — el almacenamiento documental es natural.
> - MongoDB admite índices de búsqueda vectorial para comparación por similitud.
> - La flexibilidad del esquema admite distintas versiones del modelo de embeddings.

---

## Principios de diseño

- Biometric **NO** conoce Person — `person_id` representa una identidad externa.
- Los embeddings persisten en **NoSQL** (MongoDB); el flujo de re-inscripción persiste en SQL.
- Las referencias de ID se resuelven a nivel de aplicación.
- `biometric_update_case` es propiedad lógica de Biometric (DBML v7) y referencia estas colecciones por ID lógico (`current_embedding_ref`, entre paradigmas, sin FK). Aún está alojada físicamente en el esquema `configuration` de forma transitoria por compatibilidad con Liquibase/servicios — moverla requiere una migración de compatibilidad.

---

## 1. Colección: facial_embeddings

**Propósito:** Almacenamiento de embeddings faciales (vectores numéricos que representan un rostro).

```javascript
// MongoDB Collection: facial_embeddings
{
  _id: ObjectId,

  // Business fields
  person_id: UUID,             // External identity (→ Identity.person)
  encoding: [Float],           // Facial embedding vector
  model_version: String,       // AI model version used
  template_version: Int,       // Version counter for this person's facial templates
  enrolled_at: Date,           // When this embedding was captured

  // Status fields
  is_active: Boolean,

  // Audit (in all documents)
  created_at: Date,
  updated_at: Date,
  deleted_at: Date,
  created_by: UUID,
  updated_by: UUID,
  deleted_by: UUID
}
```

**Diccionario de datos:**

| Campo | Tipo | Descripción | Ejemplo |
|-------|------|-------------|---------|
| _id | ObjectId | Identificador de documento MongoDB | `507f1f77bcf86cd799439011` |
| person_id | UUID | Identidad externa (→ Identity.person) | `550e8400-e29b-41d4-a716-446655440001` |
| encoding | Array[Float] | Vector de embedding facial (512/1024 dimensiones) | `[0.12, -0.45, ...]` |
| model_version | String | Versión del modelo de IA utilizado | `face_net_v2.1` |
| template_version | Int | Contador de versión de las plantillas faciales de esta persona | `1` |
| enrolled_at | Date | Cuándo se capturó este embedding | `2024-01-15T10:30:00Z` |
| is_active | Boolean | Indica si es el embedding activo actual | `true` |
| created_at | Date | Cuándo se creó el documento | `2024-01-15T10:30:00Z` |
| updated_at | Date | Cuándo se actualizó el documento por última vez | `2024-01-15T10:30:00Z` |
| deleted_at | Date | NULL = activo; con valor = eliminado lógicamente | `null` |
| created_by | UUID | Usuario que creó este documento | `550e8400-...` |
| updated_by | UUID | Usuario que actualizó este documento por última vez | `550e8400-...` |
| deleted_by | UUID | Usuario que eliminó lógicamente este documento | `null` |

**Índices:**

| Nombre | Campos | Tipo | Justificación |
|------|--------|------|---------------|
| idx_embedding_person_active | (person_id, is_active) | Compuesto | Buscar embeddings activos por persona |
| idx_embedding_deleted | deleted_at | Disperso (sparse) | Filtrar documentos eliminados lógicamente |

---

## 2. Colección: fingerprint_embeddings

**Propósito:** Almacenamiento de plantillas biométricas de huella dactilar.

```javascript
// MongoDB Collection: fingerprint_embeddings
{
  _id: ObjectId,

  // Business fields
  person_id: UUID,             // External identity (→ Identity.person)
  finger_number: Int,          // 1..10 (finger identifier)
  encoding: [Float],           // Fingerprint template vector
  model_version: String,       // AI model version used
  template_version: Int,       // Version counter for this person's finger templates
  enrolled_at: Date,           // When this template was captured

  // Status fields
  is_active: Boolean,

  // Audit (in all documents)
  created_at: Date,
  updated_at: Date,
  deleted_at: Date,
  created_by: UUID,
  updated_by: UUID,
  deleted_by: UUID
}
```

**Diccionario de datos:**

| Campo | Tipo | Descripción | Ejemplo |
|-------|------|-------------|---------|
| _id | ObjectId | Identificador de documento MongoDB | `507f1f77bcf86cd799439012` |
| person_id | UUID | Identidad externa (→ Identity.person) | `550e8400-...` |
| finger_number | Int | Identificador de dedo (1..10) | `1` |
| encoding | Array[Float] | Vector de plantilla de huella | `[0.23, -0.67, ...]` |
| model_version | String | Versión del modelo de IA utilizado | `fingerprint_v1.3` |
| template_version | Int | Contador de versión para esta persona+dedo | `1` |
| enrolled_at | Date | Cuándo se capturó esta plantilla | `2024-01-15T10:30:00Z` |
| is_active | Boolean | Indica si es la plantilla activa actual | `true` |
| created_at | Date | Cuándo se creó el documento | `2024-01-15T10:30:00Z` |
| updated_at | Date | Cuándo se actualizó el documento por última vez | `2024-01-15T10:30:00Z` |
| deleted_at | Date | NULL = activo; con valor = eliminado lógicamente | `null` |
| created_by | UUID | Usuario que creó este documento | `550e8400-...` |
| updated_by | UUID | Usuario que actualizó este documento por última vez | `550e8400-...` |
| deleted_by | UUID | Usuario que eliminó lógicamente este documento | `null` |

**Índices:**

| Nombre | Campos | Tipo | Justificación |
|------|--------|------|---------------|
| idx_fingerprint_person_finger_active | (person_id, finger_number, is_active) | Compuesto | Buscar plantilla activa por persona y dedo |
| idx_fingerprint_deleted | deleted_at | Disperso (sparse) | Filtrar documentos eliminados lógicamente |

---

## 3. Relaciones entre contextos

| Origen | Destino | Tipo de referencia | Mecanismo |
|------|----|----------------|-----------|
| Facial_Embedding.person_id | Identity.Person.person_id | ID externo (sin FK) | Referencia entre contextos por UUID |
| Fingerprint_Embedding.person_id | Identity.Person.person_id | ID externo (sin FK) | Referencia entre contextos por UUID |
| Configuration.Biometric_Update_Case.current_embedding_ref | Facial/Fingerprint_Embedding._id | ID externo (sin FK) | Referencia entre paradigmas (SQL → NoSQL) |

> **Nota:** Los embeddings persisten en MongoDB sin FK físicas. La tabla SQL de flujo `biometric_update_case` es propiedad lógica de Biometric y está alojada físicamente en `configuration`; todas las referencias se resuelven a nivel de aplicación.

---

## 4. Correlaciones

- Entidades de dominio que se mapean a estas colecciones → `02-domain/entities-and-rules.md`
- Patrón Saga y Outbox para consistencia distribuida → `05-architecture/pattern-guide.md`
- Datos de cada servicio en detalle → `09-microservices/services/biometric-service/data-model.md`
- Cómo se accede a los datos vía API → `07-api/contracts/openapi/`
