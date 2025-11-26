# 📊 Resumen Ejecutivo de Rúbricas - Proyecto Mutantes

## 🎯 Vista General

**Puntuación Total:** 100 puntos
**Aprobación Mínima:** 70 puntos
**Versión:** 1.0.0
**Fecha:** 2025-11-07

---

## 📈 Distribución de Puntos por Categoría

```
┌─────────────────────────────────────────┬────────┬──────────┐
│ Categoría                               │ Puntos │ Peso %   │
├─────────────────────────────────────────┼────────┼──────────┤
│ 🧬 Algoritmo de Detección de Mutantes  │   35   │   35%    │
│ 🏗️  Arquitectura y Calidad de Código    │   25   │   25%    │
│ 🧪 Testing y Cobertura                  │   20   │   20%    │
│ 🌐 API REST y Documentación             │   12   │   12%    │
│ 💾 Persistencia y Base de Datos         │    8   │    8%    │
└─────────────────────────────────────────┴────────┴──────────┘
```

---

## 🧬 Categoría 1: Algoritmo de Detección (35 puntos)

### Distribución de Puntos

| Criterio | Puntos | Descripción |
|----------|--------|-------------|
| **1.1 Correctitud Funcional** | 10 | Tests y detección correcta |
| **1.2 Complejidad Temporal** | 12 | Rendimiento y optimizaciones |
| **1.3 Complejidad Espacial** | 5 | Uso eficiente de memoria |
| **1.4 Optimizaciones** | 8 | Técnicas avanzadas implementadas |

### 1.1 Correctitud Funcional (10 puntos)

**Métricas:**
- ✅ **Tests Unitarios (6 pts):**
  - Mínimo: 15 tests en `MutantDetectorTest`
  - Óptimo: 17+ tests
  - Comando: `./gradlew test --tests MutantDetectorTest`

- ✅ **Cobertura (4 pts):**
  - Mínimo: 85% en `MutantDetector`
  - Óptimo: 95%+
  - Archivo: `build/reports/jacoco/test/html/`

**Validaciones requeridas:**
1. Detecta ADN mutante (2+ secuencias) ✓
2. Detecta ADN humano (0-1 secuencias) ✓
3. Valida matriz NxN ✓
4. Valida caracteres A,T,C,G únicamente ✓
5. Maneja casos edge (null, empty) ✓

---

### 1.2 Complejidad Temporal - RENDIMIENTO (12 puntos) ⚡

**Benchmarks de Performance:**

```
┌──────────────┬────────────┬──────────────┬────────┬────────┐
│ Tamaño       │ Óptimo     │ Aceptable    │ Peso   │ Puntos │
├──────────────┼────────────┼──────────────┼────────┼────────┤
│ 6x6          │ ≤ 1 ms     │ ≤ 5 ms       │  20%   │  2.4   │
│ 100x100      │ ≤ 20 ms    │ ≤ 100 ms     │  30%   │  3.6   │
│ 1000x1000    │ ≤ 500 ms   │ ≤ 5000 ms    │  30%   │  3.6   │
│ Early Term.  │ Código     │ Verificación │  20%   │  2.4   │
└──────────────┴────────────┴──────────────┴────────┴────────┘
```

**Fórmula:** `Puntos = base × (tiempo_óptimo / tiempo_medido)`

**Patrón Early Termination:**
```java
if (sequenceCount > 1) return true;
```

---

### 1.3 Complejidad Espacial (5 puntos)

**Evaluación:**

| Aspecto | Puntos | Validación |
|---------|--------|------------|
| **Sin estructuras auxiliares** | 3.0 | ❌ No usar `ArrayList`, `HashMap` en `isMutant()` |
| **Conversión eficiente** | 2.0 | ✅ Usar `toCharArray()` |

**Penalizaciones:**
- `-3 pts`: Uso de `new ArrayList/HashMap` dentro de `isMutant()`
- `-2 pts`: Declaración de `List<>`, `Set<>`, `Map<>` innecesarias

**Bonus:**
- `+2 pts`: Conversión a `char[][]` para acceso O(1)

---

### 1.4 Optimizaciones Implementadas (8 puntos)

**Checklist de Optimizaciones:**

```
┌─────────────────────────┬────────┬───────────────────────────┐
│ Optimización            │ Puntos │ Patrón de Código          │
├─────────────────────────┼────────┼───────────────────────────┤
│ Early Termination       │  2.4   │ sequenceCount > 1         │
│ Single Pass             │  2.0   │ Solo 2 loops (row, col)   │
│ Boundary Checking       │  1.6   │ col <= n - SEQUENCE_LEN   │
│ Direct Comparison       │  1.2   │ matrix[row][col+1] == base│
│ Validation Set O(1)     │  0.8   │ Set.of('A','T','C','G')   │
└─────────────────────────┴────────┴───────────────────────────┘
```

**Ejemplo de código óptimo:**
```java
// ✅ Early Termination (2.4 pts)
if (sequenceCount > 1) return true;

// ✅ Single Pass (2.0 pts)
for (int row = 0; row < n; row++) {
    for (int col = 0; col < n; col++) {
        // Verificar todas las direcciones aquí
    }
}

// ✅ Boundary Checking (1.6 pts)
if (col <= n - SEQUENCE_LENGTH) {
    checkHorizontal(...);
}

// ✅ Direct Comparison (1.2 pts)
return matrix[row][col+1] == base &&
       matrix[row][col+2] == base &&
       matrix[row][col+3] == base;

// ✅ Validation Set (0.8 pts)
Set<Character> VALID_BASES = Set.of('A','T','C','G');
```

---

## 🏗️ Categoría 2: Arquitectura y Calidad (25 puntos)

### Distribución

| Criterio | Puntos | Descripción |
|----------|--------|-------------|
| **2.1 Arquitectura de 6 Capas** | 8 | Estructura del proyecto |
| **2.2 Patrones de Diseño** | 5 | DI, DTO, Repository |
| **2.3 Uso de Lombok** | 3 | Reducción de boilerplate |
| **2.4 Manejo de Excepciones** | 4 | Global handler y custom |
| **2.5 Validaciones** | 5 | Bean validation y custom |

### 2.1 Arquitectura de 6 Capas (8 puntos)

**Estructura requerida:**
```
src/main/java/org/example/
├── controller/     (1.5 pts) - REST endpoints
├── dto/            (1.5 pts) - Request/Response objects
├── service/        (1.5 pts) - Business logic
├── repository/     (1.0 pts) - Data access
├── entity/         (1.5 pts) - JPA entities
└── config/         (1.0 pts) - Configuration
```

### 2.2 Patrones de Diseño (5 puntos)

| Patrón | Puntos | Implementación |
|--------|--------|----------------|
| **Dependency Injection** | 2.0 | `@RequiredArgsConstructor` + `final` fields |
| **DTO Pattern** | 1.5 | 2+ DTOs (Request/Response) |
| **Repository Pattern** | 1.5 | `extends JpaRepository` |

### 2.3 Uso de Lombok (3 puntos)

**Anotaciones requeridas (mínimo 3/6):**
- `@Data` - DTOs
- `@NoArgsConstructor` - DTOs/Entities
- `@AllArgsConstructor` - DTOs
- `@RequiredArgsConstructor` - Services/Controllers
- `@Getter` / `@Setter` - Entities

**Puntuación:**
- 5-6 anotaciones: 3.0 pts
- 3-4 anotaciones: 2.0 pts
- 1-2 anotaciones: 1.0 pts

### 2.4 Manejo de Excepciones (4 puntos)

| Elemento | Puntos | Anotación/Patrón |
|----------|--------|------------------|
| **GlobalExceptionHandler** | 2.0 | `@RestControllerAdvice` |
| **Custom Exceptions** | 1.0 | `extends RuntimeException` |
| **Exception Handlers** | 1.0 | `@ExceptionHandler` |

### 2.5 Validaciones (5 puntos)

| Validación | Puntos | Implementación |
|------------|--------|----------------|
| **Bean Validation en DTOs** | 2.0 | `@NotNull`, `@NotEmpty`, `@Valid` |
| **Custom Validator Annotation** | 1.5 | `@interface ValidDnaSequence` |
| **Validator Implementation** | 1.5 | `implements ConstraintValidator` |

---

## 🧪 Categoría 3: Testing y Cobertura (20 puntos)

### Distribución

| Criterio | Puntos | Descripción |
|----------|--------|-------------|
| **3.1 Cobertura de Código** | 8 | JaCoCo reports |
| **3.2 Suite de Tests** | 7 | Cantidad y tipos |
| **3.3 Casos de Test** | 5 | Casos específicos del algoritmo |

### 3.1 Cobertura de Código (8 puntos)

```
┌────────────────────┬─────────┬────────────┬────────┐
│ Métrica            │ Óptimo  │ Aceptable  │ Puntos │
├────────────────────┼─────────┼────────────┼────────┤
│ Cobertura Total    │  ≥90%   │   ≥70%     │  3.2   │
│ Cobertura Service  │  ≥96%   │   ≥85%     │  3.2   │
│ Cobertura Control. │  ≥95%   │   ≥80%     │  1.6   │
└────────────────────┴─────────┴────────────┴────────┘
```

**Comando:** `./gradlew test jacocoTestReport`
**Reporte:** `build/reports/jacoco/test/html/index.html`

### 3.2 Suite de Tests (7 puntos)

```
┌────────────────────┬─────────┬─────────┬────────┐
│ Tipo               │ Mínimo  │ Óptimo  │ Puntos │
├────────────────────┼─────────┼─────────┼────────┤
│ Tests Totales      │   25    │   35+   │  2.1   │
│ Tests Unitarios    │   20    │   28+   │  2.45  │
│ Tests Integración  │    8    │   12+   │  2.45  │
└────────────────────┴─────────┴─────────┴────────┘
```

**Archivos requeridos:**
- `MutantDetectorTest.java` (17 tests)
- `MutantServiceTest.java` (5 tests)
- `StatsServiceTest.java` (6 tests)
- `MutantControllerTest.java` (8 tests)

### 3.3 Casos de Test del Algoritmo (5 puntos)

**Checklist (1 punto cada uno):**

| Caso | Descripción | Pattern |
|------|-------------|---------|
| ✅ Mutante - Horizontal | ADN con secuencias horizontales | `test.*[Hh]orizontal.*[Mm]utant` |
| ✅ Mutante - Diagonal | ADN con secuencias diagonales | `test.*[Dd]iagonal.*[Mm]utant` |
| ✅ Humano - Sin secuencias | ADN sin ninguna secuencia | `test.*[Nn]o.*[Ss]equence` |
| ✅ Humano - 1 secuencia | ADN con solo 1 secuencia | `test.*[Oo]ne.*[Ss]equence` |
| ✅ Validación - Inválido | Null, empty, caracteres inválidos | `test.*(Invalid|[Nn]ull|[Ee]mpty)` |

---

## 🌐 Categoría 4: API REST y Documentación (12 puntos)

### Distribución

| Criterio | Puntos | Descripción |
|----------|--------|-------------|
| **4.1 Endpoints Funcionales** | 5 | POST /mutant, GET /stats |
| **4.2 Swagger/OpenAPI** | 4 | Documentación completa |
| **4.3 Contratos de API** | 3 | DTOs y ResponseEntity |

### 4.1 Endpoints Funcionales (5 puntos)

**POST /mutant (4 puntos):**

| Test Case | Status Esperado | Puntos |
|-----------|----------------|--------|
| Mutante válido | 200 OK | 1.5 |
| Humano válido | 403 Forbidden | 1.5 |
| Input inválido | 400 Bad Request | 1.0 |

**GET /stats (1 punto):**
- Responde 200 OK: 0.5 pts
- JSON correcto (`count_mutant_dna`, `count_human_dna`, `ratio`): 0.5 pts

### 4.2 Swagger/OpenAPI (4 puntos)

| Validación | Puntos | Verificación |
|------------|--------|--------------|
| **Swagger UI accesible** | 1.0 | `GET /swagger-ui.html` → 200 |
| **OpenAPI JSON** | 1.0 | `GET /api-docs` → JSON válido |
| **Anotaciones en Controller** | 1.0 | `@Tag`, `@Operation`, `@ApiResponse` (3+) |
| **Schema en DTOs** | 1.0 | `@Schema` en 2+ DTOs |

**URL de prueba:** `http://localhost:8080/swagger-ui.html`

### 4.3 Contratos de API (3 puntos)

| Elemento | Puntos | Implementación |
|----------|--------|----------------|
| **DTOs con Validación** | 1.5 | `@NotNull`, `@NotEmpty` en DTOs |
| **ResponseEntity** | 1.5 | Mínimo 2 usos en controller |

---

## 💾 Categoría 5: Persistencia y BD (8 puntos)

### Distribución

| Criterio | Puntos | Descripción |
|----------|--------|-------------|
| **5.1 Deduplicación (Hash)** | 4 | Estrategia de hash SHA-256 |
| **5.2 Repository JPA** | 2 | Interface con query methods |
| **5.3 Entidad JPA** | 2 | Anotaciones y campos |

### 5.1 Estrategia de Deduplicación (4 puntos)

| Elemento | Puntos | Implementación |
|----------|--------|----------------|
| **Cálculo de Hash** | 1.5 | `MessageDigest.getInstance("SHA-256")` |
| **Campo dnaHash** | 1.0 | Campo `String dnaHash` en Entity |
| **Unique Constraint** | 1.0 | `@Column(unique = true)` |
| **Búsqueda por Hash** | 0.5 | `findByDnaHash(String)` en Repository |

**¿Por qué hash?**
- Evita duplicados automáticamente
- Búsqueda O(1) eficiente
- Ahorra espacio en BD

### 5.2 Repository JPA (2 puntos)

```java
@Repository
public interface DnaRecordRepository extends JpaRepository<DnaRecord, Long> {
    Optional<DnaRecord> findByDnaHash(String dnaHash);  // 0.5 pts
    long countByIsMutant(boolean isMutant);             // 0.5 pts
}
```

**Puntos:**
- `extends JpaRepository`: 1.0 pts
- Query methods (2+): 1.0 pts

### 5.3 Entidad JPA (2 puntos)

**Anotaciones requeridas (3+):**
- `@Entity`
- `@Table`
- `@Id`
- `@GeneratedValue`
- `@Column`

**Campos requeridos (4):**
- `id` - Long
- `dnaHash` - String (unique)
- `isMutant` - boolean
- `createdAt` - LocalDateTime

**Puntos:**
- 3+ anotaciones JPA: 1.0 pts
- 4 campos requeridos: 1.0 pts

---

## 📊 Escalas de Calificación

### Por Porcentaje

```
┌────────────┬─────────────┬───────────────────────────────┐
│ Rango      │ Calificación│ Descripción                   │
├────────────┼─────────────┼───────────────────────────────┤
│ 90-100 pts │ EXCELENTE   │ Todas las optimizaciones      │
│ 80-89 pts  │ MUY BUENO   │ Mayoría de optimizaciones     │
│ 70-79 pts  │ BUENO       │ Cumple requisitos (APROBADO)  │
│ 60-69 pts  │ SUFICIENTE  │ Requiere mejoras significativas│
│ 0-59 pts   │ INSUFICIENTE│ No cumple estándares mínimos  │
└────────────┴─────────────┴───────────────────────────────┘
```

### Características por Nivel

#### 🌟 EXCELENTE (90-100)
- ✅ TODAS las optimizaciones implementadas
- ✅ Tiempos óptimos (1ms, 20ms, 500ms)
- ✅ Cobertura >90%
- ✅ 35+ tests completos
- ✅ Swagger 100% documentado
- ✅ 6 capas perfectamente organizadas
- ✅ Hash + deduplicación correcta
- ✅ Lombok en todos los componentes

#### 🔷 MUY BUENO (80-89)
- ✅ Mayoría de optimizaciones (4/5)
- ✅ Tiempos buenos (3ms, 50ms, 2000ms)
- ✅ Cobertura >85%
- ✅ 30+ tests
- ✅ Swagger con anotaciones básicas
- ⚠️ Algunos detalles menores a mejorar

#### 🟢 BUENO (70-79) - APROBADO ✓
- ✅ Optimizaciones básicas (Early Termination)
- ✅ Tiempos aceptables (5ms, 100ms, 5000ms)
- ✅ Cobertura >80%
- ✅ 25+ tests
- ✅ 6 capas implementadas
- ⚠️ Swagger incompleto
- ⚠️ Algunas optimizaciones faltantes

#### ⚠️ SUFICIENTE (60-69)
- ⚠️ Algoritmo funcional pero lento
- ⚠️ Pocas optimizaciones
- ⚠️ Cobertura 70-80%
- ⚠️ 20-25 tests
- ❌ Arquitectura incompleta
- ❌ Swagger mínimo/ausente

#### ❌ INSUFICIENTE (<60) - NO APROBADO
- ❌ Algoritmo muy lento o con errores
- ❌ Sin optimizaciones
- ❌ Cobertura <70%
- ❌ <20 tests
- ❌ Arquitectura desorganizada
- ❌ Sin documentación

---

## 🎯 Checklist Rápido de Entrega

### Algoritmo (35 pts) - PRIORIDAD MÁXIMA
- [ ] 17+ tests en `MutantDetectorTest` pasan
- [ ] `if (sequenceCount > 1) return true;` implementado
- [ ] Conversión a `char[][]` para acceso rápido
- [ ] Boundary checking antes de buscar
- [ ] Sin estructuras auxiliares innecesarias
- [ ] Cobertura >85%

### Arquitectura (25 pts)
- [ ] 6 carpetas creadas (controller, dto, service, repository, entity, config)
- [ ] `@RequiredArgsConstructor` en services/controllers
- [ ] 2+ DTOs con `@Data`
- [ ] Repository `extends JpaRepository`
- [ ] `GlobalExceptionHandler` con `@RestControllerAdvice`

### Testing (20 pts)
- [ ] 35+ tests totales
- [ ] Cobertura >70% (service >90%)
- [ ] Tests de: mutante, humano, validaciones
- [ ] `./gradlew test` pasa sin errores

### API REST (12 pts)
- [ ] POST /mutant → 200 (mutante), 403 (humano), 400 (inválido)
- [ ] GET /stats → JSON correcto
- [ ] Swagger UI en `/swagger-ui.html` funciona
- [ ] `@Tag`, `@Operation`, `@Schema` implementados

### Persistencia (8 pts)
- [ ] Campo `dnaHash` con `unique = true`
- [ ] Método `calculateDnaHash()` con SHA-256
- [ ] `findByDnaHash()` y `countByIsMutant()` en repository
- [ ] 4 campos: id, dnaHash, isMutant, createdAt

---

## 🚀 Comandos de Verificación

```bash
# Compilar y verificar build
./gradlew clean build

# Ejecutar todos los tests
./gradlew test

# Generar reporte de cobertura
./gradlew test jacocoTestReport
# Ver: build/reports/jacoco/test/html/index.html

# Tests específicos
./gradlew test --tests MutantDetectorTest
./gradlew test --tests MutantControllerTest

# Iniciar aplicación
./gradlew bootRun
# Swagger: http://localhost:8080/swagger-ui.html
# H2 Console: http://localhost:8080/h2-console

# Tests + Cobertura (recomendado para Windows)
./gradlew test jacocoTestReport --no-daemon
```

---

## 📁 Archivos Relacionados

| Archivo | Propósito | Audiencia |
|---------|-----------|-----------|
| `RUBRICA_EVALUACION.json` | Rúbrica técnica en JSON para automatización | Sistema n8n |
| `GUIA_EVALUACION_ESTUDIANTE.md` | Guía detallada con ejemplos | Estudiantes |
| `evaluador_automatico.py` | Script Python para evaluación automática | Sistema n8n / CLI |
| `RESUMEN_RUBRICAS.md` | Este documento - Vista ejecutiva | Instructores/Estudiantes |

---

## 💡 Consejos Clave para Máxima Puntuación

### Top 5 Prioridades:

1. **Algoritmo Optimizado (35%)**
   - Implementa TODAS las optimizaciones
   - Early termination es CRÍTICO
   - Mide tiempos reales

2. **Tests Completos (20%)**
   - Cubre todos los casos edge
   - Usa `@Test` para cada escenario
   - Verifica cobertura >85%

3. **Arquitectura Limpia (25%)**
   - Mantén las 6 capas separadas
   - Usa Lombok consistentemente
   - Dependency Injection correcto

4. **Documentación Swagger (12%)**
   - Anotaciones en controller y DTOs
   - Verifica en browser que funcione
   - Facilita evaluación automática

5. **Hash para BD (8%)**
   - SHA-256 para calcular hash
   - Campo único en Entity
   - Búsqueda O(1) eficiente

---

## 📞 Soporte

Para dudas sobre la evaluación:
- Consultar: `GUIA_EVALUACION_ESTUDIANTE.md` (ejemplos detallados)
- Código técnico: `RUBRICA_EVALUACION.json`
- Script evaluador: `evaluador_automatico.py`

---

**Versión:** 1.0.0
**Última actualización:** 2025-11-07
**Proyecto:** Mutantes - MercadoLibre Backend Exam