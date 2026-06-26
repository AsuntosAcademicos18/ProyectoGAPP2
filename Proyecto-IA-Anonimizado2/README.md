# 🏛️ Material Complementario (Anexo Técnico)

# Innovación pública mediante IA generativa: arquitecturas desacopladas para la gestión documental territorial

![Node.js](https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white)
![Groq](https://img.shields.io/badge/Groq_LPU-F26522?style=for-the-badge&logo=groq&logoColor=white)
![Llama 3.3](https://img.shields.io/badge/Llama_3.3_8B-0467DF?style=for-the-badge&logo=meta&logoColor=white)
![Power Automate](https://img.shields.io/badge/Power_Automate-0078D4?style=for-the-badge&logo=powerautomate&logoColor=white)

## Nota metodológica

El presente documento constituye el **material complementario (anexo técnico)** asociado al artículo *Innovación pública mediante IA generativa: arquitecturas desacopladas para la gestión documental territorial*. Su propósito es proporcionar evidencia técnica adicional que respalde el diseño, implementación y validación del artefacto desarrollado en la investigación.

De acuerdo con los principios de transparencia y trazabilidad propios de la investigación basada en diseño (*Design Science Research*), este material complementa la información contenida en el manuscrito mediante diagramas arquitectónicos, evidencias visuales de la solución implementada, descripciones de los componentes tecnológicos, fragmentos representativos de configuración y resultados de validación funcional. Su finalidad no es sustituir la explicación metodológica o conceptual del artículo principal, sino facilitar la comprensión de aquellos elementos técnicos cuya descripción detallada excede el alcance habitual de una publicación científica.

Con el fin de proteger la seguridad de la infraestructura tecnológica utilizada durante la implementación y garantizar el cumplimiento de las disposiciones aplicables en materia de protección de datos, gestión documental y ciberseguridad institucional, el material presentado ha sido sanitizado. En consecuencia, se han eliminado o anonimizado credenciales, direcciones de infraestructura, identificadores organizacionales, datos personales y componentes sensibles del código fuente. Los ejemplos incluidos conservan la lógica funcional del artefacto y permiten comprender su operación sin comprometer activos de información de la entidad.

El material complementario se encuentra organizado en cinco apartados:

1. **Arquitectura general del artefacto y flujo de datos.**
2. **Evidencias de interacción y captura documental.**
3. **Evidencia técnica del microservicio de integración y procesamiento semántico.**
4. **Visualización y explotación analítica de resultados.**
5. **Evidencias de validación funcional del artefacto.**

Cada una de estas secciones se encuentra vinculada explícitamente con los apartados correspondientes del manuscrito, permitiendo establecer una relación directa entre las decisiones de diseño, los mecanismos de implementación y los resultados obtenidos durante la evaluación del sistema.

---

# 1. Arquitectura general del artefacto

La solución propuesta se implementó mediante una arquitectura desacoplada orientada al procesamiento automatizado de documentos administrativos. El diseño distribuye las responsabilidades funcionales entre diferentes capas especializadas, permitiendo separar la captura documental, la integración, el procesamiento semántico y la explotación analítica de los resultados.

La arquitectura se encuentra compuesta por cinco niveles funcionales:

## 1.1. Capa de captura y orquestación institucional

Esta capa se apoya en el ecosistema Microsoft 365 y está integrada por Power Apps, Power Automate y SharePoint. Su función consiste en capturar los documentos suministrados por los usuarios autorizados, iniciar el flujo documental y gestionar el almacenamiento institucional de la información procesada.

## 1.2. Capa de interoperabilidad

Corresponde al mecanismo mediante el cual la información estructurada es transferida desde Microsoft 365 hacia servicios especializados mediante solicitudes HTTP. Esta capa actúa como interfaz de intercambio entre el entorno institucional y los componentes externos de procesamiento.

## 1.3. Capa de integración y control lógico

Implementada mediante un microservicio desarrollado en Node.js, esta capa verifica la integridad de las solicitudes recibidas, valida la estructura de los datos y prepara las instrucciones que serán enviadas al modelo fundacional utilizado para el análisis documental.

## 1.4. Capa de procesamiento semántico

Esta capa concentra las funciones de inferencia asociadas al procesamiento del lenguaje natural. Su propósito consiste en transformar el contenido documental en información estructurada, generando resúmenes ejecutivos y metadatos reutilizables dentro del flujo institucional.

## 1.5. Capa de persistencia y explotación analítica

Finalmente, los resultados obtenidos son reincorporados a los repositorios institucionales para su almacenamiento, consulta y utilización posterior en procesos de recuperación documental y análisis de información.

La Figura A1 presenta la representación general de la arquitectura implementada.

## Figura A1. Arquitectura general del artefacto

```mermaid
graph TD

A[Power Apps]
--> B[Power Automate]

B --> C[API REST]

C --> D[Validación estructural]
C --> E[Parametrización del modelo]

D --> F[Servicio de inferencia]
E --> F

F --> G[Generación de resumen y metadatos]

G --> C

C --> B

B --> H[SharePoint]

H --> I[Visualización y analítica]
```

La arquitectura fue concebida para minimizar las dependencias entre componentes y facilitar la incorporación de servicios especializados sin modificar los entornos institucionales preexistentes. Esta propiedad permite mantener la gobernanza documental dentro del ecosistema corporativo, mientras que las capacidades de procesamiento semántico operan en capas independientes y desacopladas.

# 2. Evidencias de interacción y captura documental

La capa de interacción constituye el punto de entrada del artefacto y concentra las funciones de captura documental, validación preliminar y visualización de resultados. Su diseño se orientó a mantener toda la experiencia de uso dentro del entorno institucional de Microsoft 365, evitando que los usuarios debieran interactuar directamente con componentes externos o servicios especializados de procesamiento semántico.

Las evidencias visuales presentadas en esta sección documentan los principales componentes de interacción incorporados al sistema y complementan la descripción desarrollada en las subsecciones 4.2 y 4.3 del manuscrito.

---

## 2.1. Captura documental y validación preliminar

La interfaz de captura fue desarrollada en Power Apps y actúa como la primera capa de control del flujo documental. Su propósito consiste en recibir los documentos proporcionados por los usuarios autorizados y verificar el cumplimiento de las condiciones mínimas necesarias para su procesamiento posterior.

Las validaciones implementadas incluyen:

- Restricción del procesamiento a un único documento por transacción.
- Verificación del formato PDF como formato documental admitido.
- Control de acceso mediante los mecanismos de autenticación del entorno institucional.
- Validación preliminar de la información requerida para iniciar el flujo automatizado.

Estas restricciones permiten reducir inconsistencias en los datos de entrada y mejorar la estabilidad del procesamiento posterior.

### Figura A2. Interfaz de captura y validación documental

![Pantalla de captura y validación](docs/img/01-captura-validacion.png)

---

## 2.2. Visualización de resultados documentales

Una vez completado el procesamiento documental, la información generada por la arquitectura retorna al entorno institucional y es presentada al usuario mediante la propia interfaz de Power Apps.

La solución permite visualizar de manera inmediata:

- El resumen ejecutivo generado durante el procesamiento.
- Las palabras clave identificadas por el sistema.
- La información asociada al documento procesado.
- El estado actualizado del registro documental.

Esta estrategia permite mantener la experiencia de interacción dentro de un único entorno de trabajo y evita que los usuarios deban consultar sistemas externos para acceder a los resultados.

### Figura A3. Visualización de resultados estructurados

![Visualización de resultados](docs/img/02-resultados-ia.png)

---

## 2.3. Integración entre Power Automate y servicios HTTP

La interoperabilidad entre la infraestructura institucional y los componentes especializados de procesamiento se implementó mediante flujos automatizados de Power Automate y solicitudes HTTP estructuradas.

Esta capa cumple una función de coordinación entre los distintos componentes del artefacto y permite:

- Consolidar la información documental capturada.
- Preparar los metadatos requeridos para el procesamiento.
- Transferir la información mediante objetos JSON estructurados.
- Recibir e incorporar las respuestas generadas por el sistema.

La arquitectura resultante facilita la integración de servicios especializados sin requerir modificaciones sustanciales en los sistemas institucionales existentes.

### Figura A4. Integración entre Power Automate y servicios HTTP

![Integración entre Power Automate y HTTP Request](docs/img/03-comparación-arquitecturas.png)

---

## 2.4. Escenarios operativos del flujo de integración

Durante las fases de validación se observaron diferentes comportamientos asociados al estado operativo del entorno de ejecución utilizado por el microservicio de integración.

Las evidencias presentadas a continuación ilustran escenarios representativos de ejecución del flujo documental bajo distintas condiciones operativas. En algunos casos se identificaron incrementos temporales en los tiempos de respuesta asociados a fenómenos de activación del entorno de ejecución (*Cold Start*), mientras que en otros escenarios la disponibilidad previa del servicio permitió respuestas más rápidas.

Estos registros deben interpretarse como ejemplos ilustrativos del comportamiento observado durante la validación funcional y no como mediciones experimentales controladas de rendimiento.

La estrategia implementada para mitigar este comportamiento se describe posteriormente en la Sección 3.4 del presente anexo y en la subsección 4.5 del manuscrito.

### Figura A5. Escenarios operativos del flujo de ejecución

![Escenarios del flujo](docs/img/04-escenarios-flujos.png)

# 3. Evidencia técnica del microservicio de integración y procesamiento semántico

La presente sección documenta los componentes técnicos más relevantes del microservicio de integración utilizado durante la implementación del artefacto. Su objetivo consiste en proporcionar evidencia complementaria de los mecanismos empleados para validar la información intercambiada entre sistemas, parametrizar el modelo fundacional y garantizar la consistencia estructural de los resultados generados.

Los fragmentos de código incluidos han sido sanitizados con el fin de proteger elementos sensibles de infraestructura y seguridad. En consecuencia, los ejemplos presentados reproducen la lógica funcional del sistema sin exponer credenciales, identificadores institucionales o componentes críticos asociados al entorno de producción.

---

## 3.1. Validación estructural de datos mediante Knor

Antes de iniciar cualquier proceso de inferencia, el microservicio verifica que la información procedente del flujo institucional cumpla con una estructura de datos previamente definida. Esta validación tiene como propósito preservar la integridad de la transacción y evitar el procesamiento de solicitudes incompletas o inconsistentes.

Para ello se implementó la librería **Knor**, utilizada como mecanismo de validación de esquemas JSON en el punto de entrada del servicio.

### Fragmento representativo de validación estructural

```javascript
const { k } = require('knor');

const actaSchema = k.object({
    idActa: k.string().required(),
    fechaComite: k.string().required(),
    textoExtraido: k.string().min(100).required(),
    usuarioRemitente: k.string().email()
});

function validarPayload(req, res, next) {
    const validacion = actaSchema.validate(req.body);

    if (!validacion.isValid) {
        return res.status(400).json({
            error: "Estructura de datos inválida",
            detalles: validacion.errors
        });
    }

    next();
}
```

La validación estructural permite asegurar la compatibilidad entre la información enviada desde Power Automate y los requisitos definidos para el procesamiento documental posterior.

---

## 3.2. Parametrización mediante prompt engineering

El comportamiento del modelo fundacional fue controlado mediante instrucciones explícitas orientadas al dominio documental de la administración pública.

Estas instrucciones fueron implementadas mediante un *system prompt* que define:

- El rol funcional del modelo.
- Los productos esperados de la inferencia.
- Las restricciones de formato.
- La estructura de la respuesta devuelta al sistema.

### Fragmento representativo del prompt sistémico

```javascript
const construirPromptSistemico = () => {
    return `
    Actúas como un analista documental institucional.

    Reglas obligatorias:

    1. Genera un resumenEjecutivo.
    2. Identifica compromisos relevantes.
    3. Extrae palabrasClave para indexación documental.

    Restricción:
    Debes responder exclusivamente mediante un objeto JSON válido.
    `;
};
```

La parametrización utilizada tuvo como finalidad favorecer la consistencia estructural de la salida y facilitar su posterior integración con los repositorios documentales institucionales.

---

## 3.3. Integración con el servicio de inferencia

Una vez validada la estructura de entrada y construido el contexto de procesamiento, el microservicio establece una comunicación asíncrona con el servicio de inferencia encargado de ejecutar el análisis semántico del documento.

La interacción se realiza mediante solicitudes HTTP estructuradas que incluyen:

- El contenido documental.
- Las instrucciones del sistema.
- Los parámetros de configuración del modelo.
- Los mecanismos de control de formato de respuesta.

### Fragmento representativo de integración

```javascript
const payload = {
    model: "MODELO_UTILIZADO",
    messages: [
        {
            role: "system",
            content: construirPromptSistemico()
        },
        {
            role: "user",
            content: textoActa
        }
    ],
    temperature: 0.1,
    response_format: {
        type: "json_object"
    }
};
```

La utilización de una temperatura reducida y de un formato de respuesta estructurado tuvo como objetivo favorecer la estabilidad operativa del flujo y garantizar la compatibilidad de los resultados con las etapas posteriores del sistema.

---

## 3.4. Estrategia de disponibilidad y mitigación del fenómeno Cold Start

Durante las pruebas de validación se identificó la existencia de incrementos temporales en los tiempos de respuesta cuando el entorno de ejecución permanecía inactivo durante periodos prolongados.

Con el fin de mitigar este comportamiento, se implementó una estrategia de disponibilidad basada en activaciones periódicas del servicio mediante **ConsoleCron**.

La lógica de funcionamiento consiste en realizar solicitudes de bajo impacto al entorno de ejecución a intervalos regulares para evitar estados prolongados de suspensión.

### Ejemplo conceptual de configuración

```text
Nombre del Job:
Keep-Alive-Middleware

Frecuencia:
*/10 * * * *

Método:
GET

Endpoint:
https://[ENDPOINT_SANITIZADO]/ping
```

Esta estrategia no participa directamente en el procesamiento documental ni modifica la lógica de inferencia. Su propósito es exclusivamente operativo y está orientado a mejorar la continuidad del servicio durante periodos de baja utilización.

# 4. Visualización y explotación analítica de resultados

La fase final del artefacto se orienta a la persistencia, consulta y explotación de la información generada durante el procesamiento documental. Una vez completada la inferencia, los resultados estructurados son reincorporados a los repositorios institucionales para su almacenamiento y posterior utilización en procesos de recuperación documental y análisis de información.

Esta capa complementa la funcionalidad del sistema al permitir que los productos generados durante la inferencia —particularmente los resúmenes ejecutivos y las palabras clave— puedan ser reutilizados en actividades de consulta, seguimiento documental y análisis institucional.

La evidencia visual presentada a continuación ilustra la manera en que los resultados procesados son integrados en un entorno de consulta diseñado para facilitar el acceso a la información almacenada en los repositorios institucionales.

---

## Figura A6. Entorno de visualización y explotación analítica

![Visualización de los datos para apoyo a la gestión documental](docs/img/05-visualización-BI.png)

---

## 4.1. Organización de la información consultable

El entorno de visualización integra la información documental original con los metadatos generados durante el procesamiento semántico. Esta organización permite consultar simultáneamente:

- El documento almacenado en el repositorio institucional.
- Los resúmenes ejecutivos generados por el sistema.
- Las palabras clave asociadas al contenido documental.
- Los metadatos de identificación y clasificación del registro.

La integración de estos elementos favorece la consulta de información histórica y permite mantener la trazabilidad entre los documentos originales y los resultados derivados del procesamiento automatizado.

---

## 4.2. Mecanismos de búsqueda y recuperación de información

Los metadatos generados por el sistema amplían las capacidades tradicionales de búsqueda documental al incorporar nuevas variables de consulta derivadas del análisis semántico.

Entre los criterios de filtrado implementados se encuentran:

- Identificador documental.
- Texto contenido en los resúmenes ejecutivos.
- Palabras clave generadas durante la inferencia.
- Dependencia organizacional.
- Responsable del registro documental.
- Periodos temporales asociados a la documentación almacenada.

Estos mecanismos permiten complementar las búsquedas tradicionales basadas exclusivamente en nombres de archivo o ubicaciones de almacenamiento.

---

## 4.3. Integración entre gestión documental y análisis de información

La incorporación de resúmenes ejecutivos y palabras clave transforma los documentos administrativos en registros enriquecidos con información estructurada adicional.

Desde una perspectiva funcional, esta capa permite aprovechar los resultados del procesamiento documental más allá de la lectura inicial del documento, facilitando procesos posteriores de consulta, recuperación y análisis institucional.

La solución implementada mantiene la vinculación entre el documento original y los productos generados durante la inferencia, preservando la coherencia documental y favoreciendo la reutilización de la información dentro del ecosistema institucional.

---

## 4.4. Relación con el artefacto presentado en el manuscrito

La información visualizada en este entorno corresponde a los resultados producidos por el flujo arquitectónico descrito en el artículo principal.

En consecuencia:

1. Los documentos son capturados y validados mediante la interfaz institucional.
2. La información es procesada a través del flujo de integración y del microservicio especializado.
3. El sistema genera resúmenes ejecutivos y palabras clave estructuradas.
4. Los resultados retornan a los repositorios institucionales.
5. La información enriquecida es puesta a disposición para consulta y análisis posterior.

De esta forma, la capa de visualización y explotación analítica constituye la etapa final del ciclo de procesamiento documental implementado por el artefacto.

# 5. Evidencias de validación funcional del artefacto

La presente sección documenta las evidencias de validación funcional utilizadas durante la evaluación del artefacto. Su propósito consiste en proporcionar soporte empírico complementario a los resultados presentados en el manuscrito, describiendo los principales escenarios de prueba implementados, los mecanismos de control aplicados y los resultados generales observados durante la validación.

La evaluación se desarrolló sobre un conjunto de documentos administrativos heterogéneos y tuvo como objetivo verificar la estabilidad operativa del flujo documental, la capacidad del sistema para completar las transacciones previstas y la consistencia estructural de la información generada.

---
## 5.1. Prueba de rendimiento y tolerancia a fallos

Tabla X. Matriz de rendimiento y tolerancia a fallos del banco de pruebas documental (_N = 30_). La tabla presenta los documentos utilizados durante la validación funcional del artefacto, incluyendo tamaño del archivo, características físicas del soporte documental, tiempos de preprocesamiento, latencia transaccional extremo a extremo y resultado final de la generación automática del resumen. Los casos fallidos corresponden a documentos con limitaciones en la fase de captura y extracción óptica de texto, particularmente en soportes impresos sobre papel color beige o con contenido manuscrito.

| ID | Nombre del archivo | Tamaño (Kb) | Característica física | Grupo | Hora de envío | Tiempo de preprocesamiento (ms) | Hora de recepción | Duración (ms) | Duración (s) | ¿Realizó resumen? |
|:--:|:-----------------:|:-----------:|:----------------------:|--------|---------------|--------------------------------|------------------|---------------|--------------|------------------|
| 14 | 20230626 Acta 1 | 2.926 | Documento impreso en papel color beige | Acta | 16:15:34 | 271 | 16:15:37 | 2.729 | 2,73 | No |
| 15 | 20240216 Acta 5 | 976 | Documento impreso en papel color beige escrito a mano | Acta | 16:26:35 | 174 | 16:26:37 | 1.826 | 1,83 | No |
| 16 | 20240712 Acta 9 | 145 | Documento digitalizado | Acta | 16:37:51 | 115 | 16:37:53 | 1.885 | 1,89 | Sí |
| 17 | 20241217 Acta 14 | 813 | Documento digitalizado | Acta | 16:38:28 | 363 | 16:38:31 | 2.637 | 2,64 | Sí |
| 18 | Acta 07 | 677 | Documento impreso en papel color beige | Acta | 16:40:46 | 141 | 16:40:48 | 1.859 | 1,86 | No |
| 19 | ACTA 1 | 1.320 | Documento digitalizado | Acta | 16:55:32 | 98 | 16:55:34 | 1.902 | 1,90 | Sí |
| 20 | ACTA 4. | 1.173 | Documento digitalizado | Acta | 16:56:40 | 206 | 16:56:43 | 2.794 | 2,79 | Sí |
| 21 | Acta 08 | 281 | Documento digitalizado | Acta | 17:59:14 | 99 | 17:59:15 | 901 | 0,90 | Sí |
| 22 | Acta 14 | 461 | Documento digitalizado | Acta | 18:01:02 | 180 | 18:01:04 | 1.820 | 1,82 | Sí |
| 23 | Acta Comité Interno PP Turismo Sept. 30 2024 | 1.370 | Documento digitalizado | Acta | 18:02:12 | 90 | 18:02:16 | 3.910 | 3,91 | Sí |
| 24 | 3 CC-F-239 Solicitud de Adquisiciones - Rev. | 269 | Documento digitalizado | Legal | 18:04:56 | 105 | 18:04:59 | 2.895 | 2,90 | Sí |
| 25 | 110-20261032 DLLO ECONÓMICO 1439 | 32 | Documento impreso en papel color blanco | Legal | 18:05:33 | 89 | 18:05:35 | 1.911 | 1,91 | Sí |
| 26 | Cambio Supervisor Mayo 25 Zona 2. | 322 | Documento digitalizado | Legal | 18:07:01 | 124 | 18:07:03 | 1.876 | 1,88 | Sí |
| 27 | CC-F-044 Designación Supervisor (1) (1) (9) | 135 | Documento digitalizado | Legal | 18:10:21 | 104 | 18:10:23 | 1.896 | 1,90 | Sí |
| 28 | CC-F-194 Clausulado Anexo (Persona Jurídica) (2) (7) | 408 | Documento digitalizado | Legal | 18:23:28 | 82 | 18:23:31 | 2.918 | 2,92 | Sí |
| 29 | Clausulado anexo convenios de asociación | 742 | Documento digitalizado | Legal | 18:24:43 | 121 | 18:24:46 | 2.879 | 2,88 | Sí |
| 30 | CLAUSULADO | 454 | Documento digitalizado | Legal | 18:25:19 | 121 | 18:25:22 | 2.879 | 2,88 | Sí |
| 31 | Inexistencia Inglés para el trabajo | 157 | Documento digitalizado | Legal | 18:26:36 | 86 | 18:26:37 | 914 | 0,91 | Sí |
| 32 | INFORME 03-02 A 04-01 | 340 | Documento digitalizado | Legal | 18:27:40 | 143 | 18:27:43 | 2.857 | 2,86 | Sí |
| 33 | INFORME DE SUPERVISIÓN ACTA 05 | 269 | Documento digitalizado | Legal | 18:28:58 | 136 | 18:29:01 | 2.864 | 2,86 | Sí |
| 34 | Circular 20260000047 | 433 | Documento digitalizado | Variado | 21:04:32 | 220 | 21:04:33 | 780 | 0,78 | Sí |
| 35 | CIRCULAR ELECCIÓN COMISIÓN DE PERSONAL-2026 | 523 | Documento digitalizado | Variado | 21:06:10 | 103 | 21:06:12 | 1.897 | 1,90 | Sí |
| 36 | Decreto municipal número 20260000500 de marzo 27 de 2026 | 5.581 | Documento digitalizado | Variado | 21:09:22 | 106 | 21:09:26 | 3.894 | 3,89 | Sí |
| 37 | Derecho de Petición | 157 | Documento digitalizado | Variado | 21:11:02 | 112 | 21:11:03 | 888 | 0,89 | Sí |
| 38 | DocModeloInclusion_NOV2025 | 9.230 | Documento digitalizado | Variado | 21:12:13 | 118 | 21:12:18 | 4.882 | 4,88 | Sí |
| 39 | IA para la productividad | 12.707 | Presentación gráfica digitalizada | Variado | 21:14:19 | 97 | 21:14:25 | 5.903 | 5,90 | Sí |
| 40 | Informe SDE (3) | 1.493 | Documento digitalizado | Variado | 21:15:50 | 97 | 21:15:53 | 2.903 | 2,90 | Sí |
| 41 | M S E CV ATS | 69 | Documento digitalizado | Variado | 21:17:19 | 97 | 21:17:20 | 903 | 0,90 | Sí |
| 42 | REGLAMENTO DE PRESTACIÓN DE SERVICIOS (2) | 1.068 | Documento digitalizado | Variado | 08:23:19 | 176 | 08:23:22 | 2.824 | 2,82 | Sí |
| 43 | Segundo_Auto_de_pruebas_T-11.443.237_anonimizado_100426 | 374 | Documento digitalizado | Variado | 08:24:33 | 96 | 08:24:36 | 2.904 | 2,90 | Sí |



## 5.1. Casos de prueba y validación funcional

Como parte del proceso de evaluación se definieron escenarios de prueba orientados a validar los principales componentes de la arquitectura.

Los casos considerados abarcaron:

- Validación de restricciones en la interfaz de captura.
- Integridad de la información intercambiada entre sistemas.
- Comportamiento del microservicio frente a solicitudes inválidas.
- Estabilidad del procesamiento documental.
- Controles básicos de acceso a los servicios expuestos.

La Tabla A1 resume los escenarios representativos utilizados durante las pruebas.

### Tabla A1. Escenarios de validación funcional

| ID | Componente | Escenario evaluado | Resultado esperado | Resultado observado | Estado |
|----------|----------|----------|----------|----------|----------|
| QA-01 | Interfaz de captura | Carga de archivo con formato no admitido | Bloqueo de la operación y notificación al usuario | Restricción aplicada correctamente | ✅ |
| QA-02 | Validación estructural | Envío de solicitud con información incompleta | Rechazo de la transacción | Solicitud rechazada mediante validación estructural | ✅ |
| QA-03 | Procesamiento documental | Documento extenso con contenido textual válido | Generación de respuesta estructurada | Procesamiento completado satisfactoriamente | ✅ |
| QA-04 | Control de acceso | Solicitud sin credenciales válidas | Denegación de acceso al servicio | Acceso restringido correctamente | ✅ |



