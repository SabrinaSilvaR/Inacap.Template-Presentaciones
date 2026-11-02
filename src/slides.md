---
theme: default
themeConfig:
  brand: red
title: Modernización de Sistema RBAC — MSC Chile | Proyecto Integrador INACAP
info: |
  ## Modernización de Sistema RBAC — MSC Chile
  Evaluación 1 · Proyecto Integrador (TIHV43) · INACAP.
  Docente: Edgar López.
author: Pablo Herrera, Jesús Rincón, Sabrina Silva, Abraham Tapia, Catalina Cubillos, Jose Morales
keywords: rbac,msc,dotnet,react,fastendpoints,inacap,security,sqlserver,redis
fonts:
  sans: INACAP Nueva Sans
  serif: INACAP Nueva Serif
  provider: none
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
mdc: true
colorSchema: light
class: text-left
---

<div class="flex-between items-center mb-3">
  <div class="eyebrow">Proyecto Integrador · TIHV43 · Evaluación 1</div>
  <span class="chip bg-brand-red/10 text-xs font-semibold text-brand-red-dark border border-brand-red/20 flex items-center gap-1.5">
    <div class="i-mdi-ship-wheel text-sm shrink-0" />
    <span>Código MSC-01</span>
  </span>
</div>

# Modernización Sistema <span class="brand-gradient brand-gradient-flow">RBAC MSC Chile</span>

<p class="mt-2 text-base text-ink/80 font-medium">Propuesta Técnica de Transición y Plataforma Web de Control de Accesos</p>

<div class="mt-4 grid grid-cols-3 gap-3">
  <div class="card p-3 border-l-4 border-brand-red flex flex-col justify-between">
    <div class="flex items-center justify-between text-brand-red">
      <span class="text-2xl font-extrabold">1.000+</span>
      <div class="i-mdi-account-multiple text-xl opacity-80" />
    </div>
    <div class="text-xs font-semibold text-ink-soft mt-1">Usuarios Activos en Plataforma</div>
  </div>
  <div class="card p-3 border-l-4 border-brand-navy flex flex-col justify-between">
    <div class="flex items-center justify-between text-brand-navy">
      <span class="text-2xl font-extrabold">37</span>
      <div class="i-mdi-apps text-xl opacity-80" />
    </div>
    <div class="text-xs font-semibold text-ink-soft mt-1">Sistemas Satélites Dependientes</div>
  </div>
  <div class="card p-3 border-l-4 border-brand-crimson flex flex-col justify-between">
    <div class="flex items-center justify-between text-brand-crimson">
      <span class="text-2xl font-extrabold">94%</span>
      <div class="i-mdi-shield-alert text-xl opacity-80" />
    </div>
    <div class="text-xs font-semibold text-ink-soft mt-1">Dependencia Crítica Operacional</div>
  </div>
</div>

<div class="mt-5 grid grid-cols-2 gap-3 text-xs">
  <div class="card p-3">
    <div class="font-bold text-brand-red-dark mb-1.5 flex items-center gap-1.5">
      <div class="i-mdi-account-group text-base shrink-0" />
      <span>Equipo Consultor</span>
    </div>
    <div class="text-ink/80 leading-relaxed">
      Pablo Herrera · Jesús Rincón · Sabrina Silva<br />
      Abraham Tapia · Catalina Cubillos · Jose Morales
    </div>
  </div>
  <div class="card p-3">
    <div class="font-bold text-brand-navy-dark mb-1.5 flex items-center gap-1.5">
      <div class="i-mdi-school text-base shrink-0" />
      <span>Datos Académicos</span>
    </div>
    <div class="text-ink/80 leading-relaxed">
      <b>Asignatura:</b> Proyecto Integrador (TIHV43)<br />
      <b>Docente Guía:</b> Profesor Edgar López · INACAP
    </div>
  </div>
</div>

<!--
Speaker Notes:
"Muy buenos días profesor Edgar López y compañeros. Imaginen una empresa naviera global como MSC que mueve miles de contenedores diarios por los puertos de nuestro país. Para que un camión ingrese a muelle, se emita una factura o se libere una carga, intervienen cerca de mil colaboradores a través de 37 sistemas informáticos distintos. Lo alarmante es esto: el noventa y cuatro por ciento de esas aplicaciones dependen críticamente de un único sistema para saber si un trabajador tiene o no permiso de operar. Hoy venimos a presentar el diagnóstico formal para la modernización del sistema de control de accesos de MSC Chile, un proyecto donde la continuidad operativa no es una opción, sino una exigencia vital para el negocio."
-->

---

<div class="flex-between items-center mb-1">
  <div class="eyebrow">Diagnóstico As-Is · Dolor Operativo</div>
  <span class="chip bg-slate-100 text-xs font-semibold text-ink-soft border border-slate-200">
    <div class="i-mdi-shield-alert-outline mr-1 text-brand-red" /> Diagnóstico Crítico
  </span>
</div>

# <span class="i-mdi-alert-circle-outline align-middle mr-2 text-brand-red" /> 1 · La Radiografía del Problema

<div class="grid grid-cols-[1.3fr_1fr] gap-4 mt-2">

<div class="card p-3 border-t-3 border-brand-red">
  <div class="text-xs font-bold text-brand-red-dark mb-1 flex items-center gap-1">
    <div class="i-mdi-monitor-multiple text-base text-brand-red" /> Diagrama de Falla Operativa: "Doble Ventana"
  </div>

```mermaid
flowchart TD
  classDef red fill:#FEE2E2,stroke:#E11927,stroke-width:2px,color:#991B1B;
  classDef navy fill:#E0E7FF,stroke:#0A2540,stroke-width:2px,color:#0A2540;
  classDef amber fill:#FEF3C7,stroke:#D97706,stroke-width:2px,color:#92400E;
  classDef dark fill:#1E293B,stroke:#0F172A,stroke-width:2px,color:#FFFFFF;

  Op["Operador Soporte TI"]:::dark --> W1["Ventana 1: Referencia"]:::navy
  Op --> W2["Ventana 2: Nuevo Usuario"]:::navy
  W1 & W2 --> Manual["Cotejo Manual Ojo a Ojo<br/>(Decenas de casillas a mano)"]:::amber
  Manual --> Pain1["Demoras de días en altas"]:::red
  Manual --> Pain2["Fatiga visual y error humano"]:::red
  Manual --> Pain3["Riesgo de escalación indebida"]:::red
```

</div>

<div class="flex flex-col gap-2.5 text-xs">
  <div class="card p-3 border-l-3 border-brand-red">
    <b class="text-brand-red-dark">Cero Abstracción:</b><br />
    No existen perfiles por cargo. Se asigna permiso por permiso y aplicación por aplicación.
  </div>
  <div class="card p-3 border-l-3 border-brand-crimson">
    <b class="text-brand-crimson">Pérdida de Automatización:</b><br />
    La migración a Azure dejó sin efecto la detección automática de departamentos.
  </div>
  <div class="card p-3 border-l-3 border-brand-navy">
    <b class="text-brand-navy-dark">Impacto Operacional:</b><br />
    Cuellos de botella de hasta 72 horas para habilitar puestos críticos en agencias y puertos.
  </div>
</div>

</div>

<!--
Speaker Notes:
"¿Cuál es el verdadero problema que detectamos? La herramienta actual no ha fallado a nivel de caídas de servidor, pero su diseño operativo es insostenible. Como muestra el diagrama de flujo, cuando un operador de soporte necesita darle a un nuevo trabajador los mismos accesos que a su compañero de área, hoy está forzado a abrir la aplicación de escritorio dos veces, poner una ventana al lado de la otra y cotejar manualmente a ojo, casilla por casilla, decenas de permisos. Esto genera demoras injustificables para el negocio y un peligro constante de error humano o escalación involuntaria de privilegios. A esto se suma que la migración a la nube en Azure dejó sin efecto la detección automática de departamentos. Para entender cómo transformar este dolor en requerimientos precisos, le cedo la palabra a Jesús Rincón."
-->

---

<div class="flex-between items-center mb-2">
  <div class="eyebrow">Levantamiento de Requerimientos · Entrevista Técnica</div>
  <span class="chip bg-slate-100 text-xs font-semibold text-ink-soft border border-slate-200">
    <div class="i-mdi-clipboard-list-outline mr-1 text-brand-navy" /> Entrevista con Cliente
  </span>
</div>

# <span class="i-mdi-clipboard-text-search-outline align-middle mr-2 text-brand-navy" /> 2 · Hechos vs. Restricciones

<div class="grid grid-cols-3 gap-3 mt-2 text-xs">

<div class="card p-3 border-t-3 border-emerald-600">
  <div class="font-bold text-emerald-800 text-sm mb-1.5 flex items-center gap-1">
    <div class="i-mdi-check-decagram text-base text-emerald-600" /> Hechos Confirmados
  </div>
  <ul class="space-y-1.5 text-ink/80 text-[11px]">
    <li>• <b>Stack Base:</b> .NET Legacy + SQL Server relacional.</li>
    <li>• <b>Infraestructura:</b> Uso exclusivo en red Citrix vía Microsoft Edge.</li>
    <li>• <b>Activos Disponibles:</b> Código fuente legado y BD anonimizada entregados para el equipo.</li>
    <li>• <b>Perfil de Uso:</b> Portal interno exclusivo para equipo de TI (sin necesidad de móviles).</li>
  </ul>
</div>

<div class="card p-3 border-t-3 border-amber-500">
  <div class="font-bold text-amber-800 text-sm mb-1.5 flex items-center gap-1">
    <div class="i-mdi-lightbulb-on text-base text-amber-500" /> Necesidades de TI
  </div>
  <ul class="space-y-1.5 text-ink/80 text-[11px]">
    <li>• <b>Modernización Web:</b> Reemplazar cliente desktop por interfaz reactiva.</li>
    <li>• <b>Perfiles por Cargo:</b> Asignación masiva en 1 paso por función.</li>
    <li>• <b>Clonación Guiada:</b> Comparador de permisos en pantalla única.</li>
    <li>• <b>Bitácora Estricta:</b> Registro con autorizador y ticket de respaldo.</li>
  </ul>
</div>

<div class="card p-3 border-t-3 border-brand-red bg-red-50/30">
  <div class="font-bold text-brand-red-dark text-sm mb-1.5 flex items-center gap-1">
    <div class="i-mdi-shield-alert text-base text-brand-red" /> La Restricción Dorada
  </div>
  <div class="p-2 rounded bg-white border border-red-200 text-red-950 font-semibold text-[11px] mb-2 leading-snug">
    CERO DOWNTIME EN PUERTO: Ningún cambio puede romper las 37 aplicaciones satélites.
  </div>
  <p class="text-ink/80 text-[11px]">
    Se autoriza rediseñar la BD, pero los Procedimientos Almacenados consumidos por los satélites deben responder <b>con idéntica firma y resultado</b>.
  </p>
</div>

</div>

<div class="mt-3 p-2 rounded bg-slate-100 border border-slate-200 text-xs text-ink/75 flex items-center gap-2">
  <div class="i-mdi-information-outline text-brand-navy text-lg shrink-0" />
  <span><b>Conclusión del Diagnóstico:</b> No se requiere app móvil ni acceso público; el foco es alta velocidad operativa dentro de Citrix y estabilidad absoluta de APIs.</span>
</div>

<!--
Speaker Notes:
"Gracias Pablo. Al reunirnos con el cliente y realizar las entrevistas técnicas y de negocio, logramos separar los deseos de las restricciones no negociables. El cliente fue categórico: el sistema debe transformarse en una solución web moderna, pero está destinado exclusivamente al equipo interno de TI dentro de la red corporativa Citrix, utilizando Microsoft Edge. Por lo tanto, no requerimos interfaces para móviles ni acceso público. Pero el hallazgo más determinante es la restricción dorada de compatibilidad: tenemos libertad para rediseñar la base de datos y optimizarla, pero los procedimientos almacenados que hoy consumen los 37 sistemas satélites deben seguir funcionando de forma idéntica. Si un cambio nuestro altera una columna o firma de esos procedimientos, detenemos la operación de MSC."
-->

---

<div class="flex-between items-center mb-1">
  <div class="eyebrow">Stakeholders · Análisis de Dominio</div>
  <span class="chip bg-slate-100 text-xs font-semibold text-ink-soft border border-slate-200">
    <div class="i-mdi-account-group-outline mr-1 text-brand-navy" /> Ecosistema de Usuarios
  </span>
</div>

# <span class="i-mdi-account-network-outline align-middle mr-2 text-brand-navy" /> 2 · Mapa de Actores y Flujo de Interacción

<div class="grid grid-cols-[1.4fr_1fr] gap-4 mt-2">

<div class="card p-3 border-t-3 border-brand-navy">
  <div class="text-xs font-bold text-brand-navy-dark mb-1 flex items-center gap-1">
    <div class="i-mdi-sitemap text-base text-brand-navy" /> Matriz de Relación con el Sistema Central
  </div>

```mermaid
flowchart TD
  classDef red fill:#FEE2E2,stroke:#E11927,stroke-width:2px,color:#991B1B;
  classDef navy fill:#E0E7FF,stroke:#0A2540,stroke-width:2px,color:#0A2540;
  classDef dark fill:#1E293B,stroke:#0F172A,stroke-width:2px,color:#FFFFFF;
  classDef light fill:#F8FAFC,stroke:#0A2540,stroke-width:1.5px,color:#0A2540;

  Core["<b>Sistema RBAC Central MSC</b><br/>(Portal Web + API .NET 10)"]:::red
  Soporte["<b>Soporte TI</b> (Directo)<br/>Necesidad: Clonación en 1 clic"]:::navy -->|Administra accesos| Core
  Seguridad["<b>Seguridad TI</b> (Auditor)<br/>Necesidad: Auditoría inmutable"]:::dark -->|Supervisa tickets| Core
  Core -->|Autorización < 20 ms| Satelites["<b>37 Apps Satélites</b><br/>Necesidad: Cero downtime"]:::navy
  Core -->|Habilitación Día 1| Usuarios["<b>1.000+ Colaboradores</b><br/>Necesidad: Alta inmediata"]:::light
```

</div>

<div class="flex flex-col gap-2 text-xs">
  <div class="card p-2.5 border-l-3 border-brand-red">
    <b class="text-brand-red-dark">Soporte TI:</b><br />
    Agilidad operativa sin abrir dos ventanas paralelas.
  </div>
  <div class="card p-2.5 border-l-3 border-brand-navy">
    <b class="text-brand-navy-dark">Seguridad:</b><br />
    Trazabilidad de auditoría: quién autorizó, cuándo y bajo qué ticket.
  </div>
  <div class="card p-2.5 border-l-3 border-brand-crimson">
    <b class="text-brand-crimson">Satélites:</b><br />
    Continuidad operacional absoluta y respuesta ultra rápida.
  </div>
  <div class="card p-2.5 border-l-3 border-emerald-600">
    <b class="text-emerald-900">1.000+ Colaboradores:</b><br />
    Alta inmediata en su primer día laboral sin demoras burocráticas.
  </div>
</div>

</div>

<!--
Speaker Notes:
"Con estas reglas claras, estructuramos a los cuatro actores del ecosistema como muestra este mapa interactivo. El usuario directo es el operador de TI, cuyo dolor es la lentitud del proceso manual. El responsable es la Jefatura de Seguridad, que no puede auditar con precisión quién autorizó una excepción ni bajo qué ticket formal. Los interesados técnicos son los equipos de los 37 sistemas satélites, que no tienen tiempo ni presupuesto para reprogramar sus aplicaciones. Y finalmente, los beneficiarios son los mil trabajadores de MSC que necesitan sus accesos a tiempo. ¿Cómo convertimos estas necesidades en valor concreto para el negocio? Mi compañera Sabrina Silva nos presentará las Historias de Usuario seleccionadas."
-->

---

<div class="flex-between items-center mb-2">
  <div class="eyebrow">Especificación Ágil · Historias de Usuario</div>
  <span class="chip bg-slate-100 text-xs font-semibold text-ink-soft border border-slate-200">
    <div class="i-mdi-playlist-check mr-1 text-brand-red" /> Historias Clave
  </span>
</div>

# <span class="i-mdi-card-bulleted-settings-outline align-middle mr-2 text-brand-red" /> 3 · Historias de Usuario Operativas

<div class="grid grid-cols-3 gap-3 mt-2 text-xs">

<div class="card p-3 border-t-3 border-brand-red">
  <div class="font-bold text-brand-red-dark text-sm mb-1">
    HU01 · Asignación por Cargo
  </div>
  <p class="text-ink/70 text-[11px] italic mb-2">
    "Como Soporte TI, quiero asignar un perfil de cargo predefinido para dar de alta accesos en un solo paso."
  </p>
  <div class="p-2 rounded bg-slate-50 border border-slate-200 text-[11px]">
    <b>Impacto:</b> Elimina la selección manual casilla por casilla. Un facturador recibe de inmediato el set estándar de sus aplicaciones.
  </div>
</div>

<div class="card p-3 border-t-3 border-brand-navy">
  <div class="font-bold text-brand-navy-dark text-sm mb-1">
    HU02 · Clonación Ágil (1 Clic)
  </div>
  <p class="text-ink/70 text-[11px] italic mb-2">
    "Como Soporte TI, quiero comparar y clonar permisos entre usuarios en una sola pantalla para erradicar la doble ventana."
  </p>
  <div class="p-2 rounded bg-slate-50 border border-slate-200 text-[11px]">
    <b>Impacto:</b> Vista de pantalla dividida con diferenciador visual. Clona atributos validados sin errores de cotejo.
  </div>
</div>

<div class="card p-3 border-t-3 border-brand-crimson">
  <div class="font-bold text-brand-crimson text-sm mb-1">
    HU03 · Excepciones Controladas
  </div>
  <p class="text-ink/70 text-[11px] italic mb-2">
    "Como Soporte TI, quiero otorgar permisos temporales fuera de cargo para resolver casos puntuales sin romper el estándar."
  </p>
  <div class="p-2 rounded bg-slate-50 border border-slate-200 text-[11px]">
    <b>Impacto:</b> Permite flexibilidad (ej. secretaria de gerencia) exigiendo motivo obligatorio y ticket de respaldo.
  </div>
</div>

</div>

<div class="mt-3 card p-3 border-l-4 border-emerald-600 text-xs">
  <b class="text-emerald-800">Principio de Menor Privilegio:</b> El cargo define la línea base institucional. Cualquier atribución extra no ensucia el perfil global, sino que queda auditada como excepción individual trazable.
</div>

<!--
Speaker Notes:
"Muchas gracias Jesús. El corazón de nuestro diagnóstico fue asegurar que cada historia de usuario resuelva un dolor real de la operación. Seleccionamos tres historias operacionales fundamentales: Primero, la Historia de Usuario 1 permite asignar permisos por cargo. Si ingresa un 'Facturador Marítimo', el operador selecciona ese perfil y el sistema otorga de golpe todas las atribuciones en las aplicaciones pertinentes. Segundo, la Historia de Usuario 2 ataca directamente el peor dolor levantado: la clonación de permisos. En una sola vista dividida, el operador selecciona al usuario de referencia y al nuevo, ve las diferencias y clona todo en un clic. Adiós a las dos ventanas abiertas en paralelo. Y tercero, la Historia 3 maneja excepciones: permite dar permisos extraordinarios pero etiquetándolos con un motivo obligatorio."
-->

---

<div class="flex-between items-center mb-2">
  <div class="eyebrow">Criterios de Aceptación & Gobernanza</div>
  <span class="chip bg-slate-100 text-xs font-semibold text-ink-soft border border-slate-200">
    <div class="i-mdi-shield-check mr-1 text-brand-red" /> Estándar y Auditoría
  </span>
</div>

# <span class="i-mdi-shield-check-outline align-middle mr-2 text-brand-red" /> 3 · Gobernanza y Control Técnico

<div class="grid grid-cols-3 gap-3 mt-2 text-xs">

<div class="card p-3 border-t-3 border-brand-navy">
  <div class="font-bold text-brand-navy-dark text-sm mb-1 flex items-center gap-1">
    <div class="i-mdi-history text-base text-brand-navy" /> HU04 · Auditoría Inmutable
  </div>
  <p class="text-[11px] text-ink/80 mb-2">
    Trazabilidad integral de cada alta, baja o modificación de permisos.
  </p>
  <div class="p-2 rounded bg-slate-50 border border-slate-200 text-[11px] space-y-1">
    <div>• Registro automático: operador, IP, fecha y hora.</div>
    <div>• Campo obligatorio de motivo o ticket formal.</div>
    <div>• Adjunto digital del correo o aprobación.</div>
  </div>
</div>

<div class="card p-3 border-t-3 border-brand-red">
  <div class="font-bold text-brand-red-dark text-sm mb-1 flex items-center gap-1">
    <div class="i-mdi-speedometer text-base text-brand-red" /> HU05 · API Satélites (&lt; 20 ms)
  </div>
  <p class="text-[11px] text-ink/80 mb-2">
    Endpoint REST moderno para consulta de permisos efectivos por usuario.
  </p>
  <div class="p-2 rounded bg-slate-50 border border-slate-200 text-[11px] space-y-1">
    <div>• Latencia P95 &lt; 20 ms garantizada.</div>
    <div>• Respuesta serializada en JSON plano.</div>
    <div>• Desacoplamiento de llamadas pesadas a BD.</div>
  </div>
</div>

<div class="card p-3 border-t-3 border-brand-crimson">
  <div class="font-bold text-brand-crimson text-sm mb-1 flex items-center gap-1">
    <div class="i-mdi-database-cog text-base text-brand-crimson" /> HU06 · Catálogo Autónomo
  </div>
  <p class="text-[11px] text-ink/80 mb-2">
    CRUD desacoplado de cargos y departamentos administrado por TI.
  </p>
  <div class="p-2 rounded bg-slate-50 border border-slate-200 text-[11px] space-y-1">
    <div>• Independencia de errores de Azure AD.</div>
    <div>• Normalización de nombres de cargos.</div>
    <div>• Mapeo 1:N con aplicaciones satélites.</div>
  </div>
</div>

</div>

<div class="mt-3 card p-2.5 border-l-4 border-brand-navy text-xs flex items-center gap-2">
  <div class="i-mdi-check-decagram text-brand-navy text-lg shrink-0" />
  <span><b>Gobernanza TI:</b> Los módulos garantizan que la velocidad operativa nunca comprometa la seguridad, trazabilidad ni el cumplimiento normativo exigido por MSC Global.</span>
</div>

<!--
Speaker Notes:
"Pero una solución corporativa no solo debe ser ágil; debe ser segura y gobernable. En la Historia de Usuario 4 establecimos criterios estrictos de auditoría: ningún permiso puede ser creado o modificado sin que el sistema capture de forma inmutable quién fue el operador, la fecha exacta y el correo o ticket de respaldo adjunto. Además, en la Historia 5, definimos que el nuevo endpoint para satélites debe responder en menos de 20 milisegundos, para no ralentizar las operaciones en muelle. Y con la Historia 6, devolvemos a TI el control de su catálogo de departamentos sin depender de los datos mal formateados de Microsoft Graph. Para ver cómo estos requerimientos se formalizan bajo estándares internacionales, dejo con ustedes a Abraham Tapia."
-->

---

<div class="flex-between items-center mb-2">
  <div class="eyebrow">Ingeniería de Requerimientos · Priorización</div>
  <span class="chip bg-slate-100 text-xs font-semibold text-ink-soft border border-slate-200">
    <div class="i-mdi-filter-variant mr-1 text-brand-navy" /> Priorización MoSCoW
  </span>
</div>

# <span class="i-mdi-format-list-checks align-middle mr-2 text-brand-navy" /> 4 · Requerimientos Funcionales (MoSCoW)

<div class="grid grid-cols-[1.4fr_1fr] gap-4 mt-2 text-xs">

<div class="card p-3 border-t-3 border-brand-red">
  <div class="font-bold text-brand-red-dark text-sm mb-2 flex items-center gap-1.5">
    <div class="i-mdi-star-check text-base text-brand-red" /> MUST HAVE (Críticos e Irrenunciables)
  </div>

  <div class="grid grid-cols-2 gap-2 text-[11px]">
    <div class="p-2 rounded bg-slate-50 border border-slate-200">
      <b>RF01 · Perfiles de Cargo:</b> Creación y edición de paquetes estándar de permisos.
    </div>
    <div class="p-2 rounded bg-slate-50 border border-slate-200">
      <b>RF02 · Asignación Masiva:</b> Vinculación inmediata de usuario con perfil.
    </div>
    <div class="p-2 rounded bg-slate-50 border border-slate-200">
      <b>RF03 · Clonación Ágil:</b> Copia y comparación en interfaz única de 1 clic.
    </div>
    <div class="p-2 rounded bg-slate-50 border border-slate-200">
      <b>RF04 · Excepciones:</b> Permisos temporales fuera de cargo con motivo.
    </div>
    <div class="p-2 rounded bg-slate-50 border border-slate-200">
      <b>RF05 · API Satélites:</b> Consulta de permisos efectivos en &lt; 20 ms.
    </div>
    <div class="p-2 rounded bg-slate-50 border border-slate-200">
      <b>RF06 · Auditoría Inmutable:</b> Log histórico protegido con respaldo.
    </div>
  </div>
  
  <div class="mt-2 p-1.5 rounded bg-red-50 border border-red-200 text-red-950 font-semibold text-[11px]">
    <b>RF08 · Fachada de SPs:</b> Capa de compatibilidad SQL Server para las 37 aplicaciones.
  </div>
</div>

<div class="flex flex-col gap-3">
  <div class="card p-3 border-t-3 border-amber-500">
    <div class="font-bold text-amber-800 text-sm mb-1 flex items-center gap-1">
      <div class="i-mdi-plus-box text-base text-amber-500" /> SHOULD HAVE (Deseables)
    </div>
    <div class="p-2 rounded bg-slate-50 border border-slate-200 text-[11px]">
      <b>RF07 · Catálogo Desacoplado:</b> Módulo administrativo autónomo para jerarquías y áreas sin depender de Azure.
    </div>
  </div>

  <div class="card p-3 border-t-3 border-slate-400 bg-slate-50/50">
    <div class="font-bold text-slate-700 text-sm mb-1 flex items-center gap-1">
      <div class="i-mdi-close-circle-outline text-base text-slate-500" /> WON'T HAVE (Fuera de Alcance)
    </div>
    <div class="p-2 rounded bg-white border border-slate-200 text-[11px] text-ink/75">
      <b>Sincronización Bidireccional con RRHH:</b> Se descarta integración automática con sistemas externos para blindar los plazos semestrales.
    </div>
  </div>
</div>

</div>

<!--
Speaker Notes:
"Gracias Sabrina. En ingeniería de requerimientos, priorizar es decidir qué hace viable un proyecto. Siguiendo la metodología MoSCoW, clasificamos siete requerimientos como 'Must Have'. Entre ellos destacan el RF03 para la clonación inmediata de accesos, el RF06 para la bitácora inalterable y el RF08, que obliga al backend a convivir con los procedimientos almacenados de base de datos actuales. Dejamos deliberadamente como un 'Should Have' la administración avanzada de departamentos y fuera del alcance cualquier integración automática con sistemas de Recursos Humanos, protegiendo así los tiempos de entrega del semestre."
-->

---

<div class="flex-between items-center mb-2">
  <div class="eyebrow">Estándar Internacional · Calidad de Software</div>
  <span class="chip bg-slate-100 text-xs font-semibold text-ink-soft border border-slate-200">
    <div class="i-mdi-standard-definition mr-1 text-brand-navy" /> Norma ISO/IEC 25010
  </span>
</div>

# <span class="i-mdi-certificate-outline align-middle mr-2 text-brand-navy" /> 4 · Requerimientos No Funcionales (ISO 25010)

<div class="grid grid-cols-4 gap-3 mt-3 text-xs">

<div class="card p-3 border-t-3 border-brand-red">
  <div class="font-bold text-brand-red-dark text-sm mb-1 flex items-center gap-1">
    <div class="i-mdi-speedometer text-base text-brand-red" /> Rendimiento
  </div>
  <div class="font-mono text-xs font-bold text-brand-red mb-1">RNF01 & RNF02</div>
  <ul class="space-y-1 text-ink/80 text-[11px]">
    <li>• <b>Latencia P95 &lt; 20 ms</b> en consultas de autorización satélite.</li>
    <li>• <b>Descarga de Cómputo:</b> Traslado de lógica pesada desde SQL Server hacia C#.</li>
  </ul>
</div>

<div class="card p-3 border-t-3 border-brand-navy">
  <div class="font-bold text-brand-navy-dark text-sm mb-1 flex items-center gap-1">
    <div class="i-mdi-shield-lock text-base text-brand-navy" /> Seguridad
  </div>
  <div class="font-mono text-xs font-bold text-brand-navy mb-1">RNF03 & RNF04</div>
  <ul class="space-y-1 text-ink/80 text-[11px]">
    <li>• <b>Autenticación OIDC</b> en entorno corporativo Citrix.</li>
    <li>• <b>Auditoría Append-Only:</b> Tablas de solo lectura (prohibido UPDATE y DELETE).</li>
  </ul>
</div>

<div class="card p-3 border-t-3 border-brand-crimson">
  <div class="font-bold text-brand-crimson text-sm mb-1 flex items-center gap-1">
    <div class="i-mdi-sync text-base text-brand-crimson" /> Compatibilidad
  </div>
  <div class="font-mono text-xs font-bold text-brand-crimson mb-1">RNF05</div>
  <ul class="space-y-1 text-ink/80 text-[11px]">
    <li>• <b>100% Retrocompatibilidad</b> con firmas históricas de SPs consumidos.</li>
    <li>• Ninguna modificación requerida en las 37 aplicaciones cliente.</li>
  </ul>
</div>

<div class="card p-3 border-t-3 border-emerald-600">
  <div class="font-bold text-emerald-800 text-sm mb-1 flex items-center gap-1">
    <div class="i-mdi-laptop text-base text-emerald-600" /> Usabilidad
  </div>
  <div class="font-mono text-xs font-bold text-emerald-800 mb-1">RNF06</div>
  <ul class="space-y-1 text-ink/80 text-[11px]">
    <li>• <b>Optimizado para Edge</b> sobre sesiones remotas de Citrix.</li>
    <li>• Interfaz reactiva fluida sin recargas completas de ventana.</li>
  </ul>
</div>

</div>

<div class="mt-3 card p-2.5 border-l-4 border-brand-red text-xs flex items-center justify-between">
  <div class="flex items-center gap-2">
    <div class="i-mdi-shield-check text-brand-red text-lg shrink-0" />
    <span><b>Compromiso de Calidad:</b> Garantía de continuidad operacional, latencia submilisegundo y cumplimiento de auditoría global.</span>
  </div>
  <span class="chip bg-emerald-100 text-emerald-800 text-[10px] font-bold">100% Verificable</span>
</div>

<!--
Speaker Notes:
"Un buen diagnóstico no solo define qué hace el software, sino cómo debe comportarse. Aplicando la norma internacional ISO 25010, definimos requerimientos no funcionales muy rigurosos. En rendimiento, exigimos un tiempo de respuesta menor a 20 milisegundos para las consultas de los satélites. Pero la decisión más potente de ingeniería radica en el RNF02: hoy la base de datos está estresada porque ejecuta cálculos pesados en procedimientos almacenados sin índices. Nuestro requerimiento exige trasladar toda esa lógica a código C#, aliviando el motor de datos. En seguridad, garantizamos que la auditoría sea 'Append-Only': nadie, ni siquiera un administrador, podrá modificar o borrar un registro histórico. ¿Cómo se implementa esto a nivel de arquitectura y código? Mi compañera Catalina Cubillos nos explicará el diseño To-Be."
-->

---

<div class="flex-between items-center mb-1">
  <div class="eyebrow">Transición · Restricciones y decisiones preliminares</div>
  <span class="chip bg-slate-100 text-xs font-semibold text-ink-soft border border-slate-200">
    <div class="i-mdi-shield-check-outline mr-1 text-brand-red" /> Continuidad operativa
  </span>
</div>

# <span class="i-mdi-swap-horizontal align-middle mr-2 text-brand-red" /> 5 · Modernizar sin alterar los contratos actuales

<div class="grid grid-cols-3 gap-3 mt-3 text-xs">
  <div class="card p-3 border-t-3 border-brand-navy">
    <div class="font-bold text-brand-navy-dark text-sm mb-2 flex items-center gap-1.5">
      <div class="i-mdi-server-network text-base text-brand-navy" /> Contexto actual
    </div>
    <p class="text-ink/80 leading-relaxed">
      Sistema legado sobre .NET y SQL Server. El equipo de TI lo utiliza dentro de Citrix con Microsoft Edge.
    </p>
  </div>

  <div class="card p-3 border-t-3 border-brand-crimson">
    <div class="font-bold text-brand-crimson text-sm mb-2 flex items-center gap-1.5">
      <div class="i-mdi-link-variant text-base text-brand-crimson" /> Restricción del cliente
    </div>
    <p class="text-ink/80 leading-relaxed">
      Los 37 sistemas satélite consumen procedimientos almacenados. Se debe conservar su firma y resultado, sin modificar esas aplicaciones.
    </p>
  </div>

  <div class="card p-3 border-t-3 border-brand-red">
    <div class="font-bold text-brand-red-dark text-sm mb-2 flex items-center gap-1.5">
      <div class="i-mdi-clipboard-check-outline text-base text-brand-red" /> Decisión preliminar
    </div>
    <p class="text-ink/80 leading-relaxed">
      Enfocar el proyecto en un portal web interno para administrar accesos y perfiles. La tecnología concreta queda por definir.
    </p>
  </div>
</div>

<div class="mt-3 card p-3 border-l-4 border-brand-red bg-brand-red/5 text-xs flex items-start gap-2">
  <div class="i-mdi-progress-question text-brand-red text-lg shrink-0" />
  <p class="text-ink/80 leading-relaxed">
    <b class="text-brand-red-dark">Pendiente de validar:</b> alternativas de arquitectura e integración con el código legado y el esquema de base de datos anonimizado entregados al equipo. En esta EVA definimos el problema y sus límites; el diseño técnico corresponde a una etapa posterior.
  </p>
</div>

<!--
Speaker Notes:
"Gracias Abraham. En esta etapa no estamos cerrando una arquitectura. Partimos de lo levantado con el cliente: el sistema actual se apoya en .NET y SQL Server, y el equipo de TI lo usa dentro de Citrix con Microsoft Edge. La restricción principal es la continuidad: 37 sistemas satélite consumen procedimientos almacenados y se debe mantener su firma y resultado, sin modificar esas aplicaciones. A partir de esa necesidad, proponemos centrar el alcance en un portal web interno para administrar accesos y perfiles. La tecnología concreta y el diseño de integración quedan pendientes; deberán decidirse tras revisar el código legado y el esquema anonimizado que recibió el equipo, y comprobar que la compatibilidad se mantiene. En la siguiente diapositiva revisaremos los riesgos relacionados con los sistemas satélite y los controles que debemos considerar."
-->
---

<div class="flex-between items-center mb-1">
  <div class="eyebrow">Estrategia de Coexistencia · Patrón Fachada</div>
  <span class="chip bg-slate-100 text-xs font-semibold text-ink-soft border border-slate-200">
    <div class="i-mdi-transit-connection mr-1 text-brand-red" /> Continuidad Operacional
  </span>
</div>

# <span class="i-mdi-source-fork align-middle mr-2 text-brand-red" /> 5 · Coexistencia con Satélites y Riesgos

<div class="grid grid-cols-[1.4fr_1fr] gap-4 mt-1">

<div class="card p-3 border-t-3 border-brand-navy">
  <div class="text-xs font-bold text-brand-navy-dark mb-1 flex items-center gap-1">
    <div class="i-mdi-transit-connection-variant text-base text-brand-navy" /> Flujo del Patrón "SP Fachada": Cero Rotura
  </div>

```mermaid
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'fontFamily': 'INACAP Nueva Sans, system-ui, sans-serif',
    'fontSize': '12px',
    'actorBkg': '#0A2540',
    'actorBorder': '#0A2540',
    'actorTextColor': '#FFFFFF',
    'actorLineColor': '#0A2540',
    'signalColor': '#0A2540',
    'signalTextColor': '#1E293B',
    'labelBoxBkgColor': '#FEE2E2',
    'labelBoxBorderColor': '#E11927',
    'labelTextColor': '#991B1B',
    'noteBkgColor': '#FEE2E2',
    'noteBorderColor': '#E11927',
    'noteTextColor': '#991B1B',
    'activationBkgColor': '#E0E7FF',
    'activationBorderColor': '#0A2540',
    'sequenceNumberColor': '#FFFFFF'
  }
}}%%
sequenceDiagram
  autonumber
  participant Sat as 37 Apps Satélites
  participant SP as SP Fachada (SQL Server)
  participant Redis as Redis Cache
  participant Net as Backend .NET 10
  Sat->>SP: EXEC sp_ObtenerPermisos (Misma firma)
  SP->>Redis: Consulta en memoria (< 5 ms)
  Redis-->>SP: Retorna permisos precalculados
  SP-->>Sat: Resultado idéntico (Cero rotura)
  Note over Net,Redis: .NET 10 sincroniza e invalida caché
```

</div>

<div class="flex flex-col gap-2 text-xs">
  <div class="card p-2.5 border-l-4 border-brand-red !bg-white shadow-sm">
    <div class="flex items-center gap-1.5 font-bold text-brand-red-dark mb-0.5">
      <div class="i-mdi-alert-circle text-brand-red text-sm shrink-0" />
      <span>Riesgo 1: Caída de Satélites</span>
    </div>
    <div class="text-ink/80 text-[11px] leading-snug">
      <b>Control:</b> Tests de regresión automatizados sobre contratos de SPs.
    </div>
  </div>
  <div class="card p-2.5 border-l-4 border-amber-500 !bg-white shadow-sm">
    <div class="flex items-center gap-1.5 font-bold text-amber-800 mb-0.5">
      <div class="i-mdi-shield-alert text-amber-500 text-sm shrink-0" />
      <span>Riesgo 2: Escalación de Accesos</span>
    </div>
    <div class="text-ink/80 text-[11px] leading-snug">
      <b>Control:</b> RBAC estricto interno y autenticación corporativa OIDC.
    </div>
  </div>
  <div class="card p-2.5 border-l-4 border-brand-navy !bg-white shadow-sm">
    <div class="flex items-center gap-1.5 font-bold text-brand-navy mb-0.5">
      <div class="i-mdi-database-alert text-brand-navy text-sm shrink-0" />
      <span>Riesgo 3: Sobrecarga en BD</span>
    </div>
    <div class="text-ink/80 text-[11px] leading-snug">
      <b>Control:</b> Cómputo trasladado a C# y caché selectiva en Redis.
    </div>
  </div>
</div>

</div>

<!--
Speaker Notes:
"¿Cómo resolvemos el gran dilema de no romper las 37 aplicaciones existentes mientras modernizamos la base de datos? Como pueden ver en este diagrama de secuencia, aplicamos el patrón fachada: los procedimientos almacenados que los satélites ya conocen seguirán existiendo en SQL Server con exactamente los mismos nombres y parámetros, pero ya no ejecutarán consultas lentas ni bucles pesados; simplemente leerán de tablas precalculadas y de Redis en memoria. Con esto logramos tres victorias: la base de datos se descarga, los satélites siguen operando sin enterarse del cambio y la seguridad queda blindada bajo autenticación corporativa. Para explicarnos cómo garantizamos la viabilidad del proyecto y sus límites, le doy el pase a Jose Morales."
-->

---

<div class="flex-between items-center mb-2">
  <div class="eyebrow">Planificación Ágil · Seguridad de Pruebas</div>
  <span class="chip bg-slate-100 text-xs font-semibold text-ink-soft border border-slate-200">
    <div class="i-mdi-shield-check-outline mr-1 text-brand-navy" /> Estrategia de Entrega
  </span>
</div>

# <span class="i-mdi-calendar-check-outline align-middle mr-2 text-brand-navy" /> 6 · Viabilidad, Datos Dummy y Alcance

<div class="grid grid-cols-2 gap-4 mt-2 text-xs">

<div class="card p-3 border-t-3 border-brand-navy flex flex-col gap-2.5">
  <div class="font-bold text-brand-navy-dark text-sm flex items-center gap-1.5">
    <div class="i-mdi-test-tube text-base text-brand-navy" /> Metodología y Pruebas con Datos Dummy
  </div>
  <div class="p-2 rounded bg-slate-50 border border-slate-200 text-[11px] text-ink/85 leading-relaxed">
    <b>Entorno Seguro Replicado:</b> MSC prohíbe el acceso a datos productivos. Replicaremos el esquema DDL entregado en SQL Server local con datos sintéticos:
  </div>
  <div class="grid grid-cols-3 gap-2 text-center">
    <div class="card p-2 bg-slate-50 border border-slate-200">
      <div class="text-xl font-extrabold text-brand-navy">50</div>
      <div class="text-[10px] font-semibold text-ink-soft">Usuarios Dummy</div>
    </div>
    <div class="card p-2 bg-slate-50 border border-slate-200">
      <div class="text-xl font-extrabold text-brand-red">10</div>
      <div class="text-[10px] font-semibold text-ink-soft">Departamentos</div>
    </div>
    <div class="card p-2 bg-slate-50 border border-slate-200">
      <div class="text-xl font-extrabold text-brand-crimson">37</div>
      <div class="text-[10px] font-semibold text-ink-soft">Satélites Mocks</div>
    </div>
  </div>
  <div class="p-2.5 rounded bg-slate-100 border border-slate-200 text-[11px] text-ink/80 flex items-center gap-2">
    <div class="i-mdi-git text-brand-navy text-base shrink-0" />
    <span><b>Pipeline Ágil:</b> GitHub Projects (Kanban) + Trunk-Based CI con integración continua.</span>
  </div>
  <div class="p-2.5 rounded bg-blue-50/60 border border-blue-200/70 text-[11px] text-ink/80 flex items-center gap-2">
    <div class="i-mdi-shield-check text-brand-navy text-base shrink-0" />
    <span><b>Validación Continua:</b> Tests automáticos sobre contratos de SPs para garantizar cero rotura.</span>
  </div>
</div>

<div class="card p-3 border-t-3 border-brand-red flex flex-col justify-between">
  <div>
    <div class="font-bold text-brand-red-dark text-sm mb-2 flex items-center gap-1.5">
      <div class="i-mdi-arrow-decision-outline text-base text-brand-red" /> Delimitación Honesta de Alcance
    </div>
    <div class="card p-2.5 border-l-4 border-emerald-600 bg-emerald-50/40 mb-2.5">
      <div class="text-[11px] font-bold text-emerald-800 mb-1 flex items-center gap-1">
        <div class="i-mdi-check-decagram text-emerald-600 text-sm" /> Compromisos Clave (Dentro del Alcance)
      </div>
      <div class="space-y-1 text-[11px] text-ink/85">
        <div class="flex items-start gap-1.5">
          <div class="i-mdi-check-circle text-emerald-600 text-sm shrink-0 mt-0.5" />
          <span><b>Portal Web React SPA:</b> Interfaz fluida para soporte TI dentro de Citrix.</span>
        </div>
        <div class="flex items-start gap-1.5">
          <div class="i-mdi-check-circle text-emerald-600 text-sm shrink-0 mt-0.5" />
          <span><b>Backend .NET 10:</b> API REPR de alto rendimiento con aceleración en Redis.</span>
        </div>
        <div class="flex items-start gap-1.5">
          <div class="i-mdi-check-circle text-emerald-600 text-sm shrink-0 mt-0.5" />
          <span><b>Capa de Coexistencia:</b> Retrocompatibilidad total con SPs legados de satélites.</span>
        </div>
      </div>
    </div>
    <div class="card p-2.5 border-l-4 border-brand-red bg-rose-50/40">
      <div class="text-[11px] font-bold text-brand-red-dark mb-1 flex items-center gap-1">
        <div class="i-mdi-close-octagon text-brand-red text-sm" /> Exclusiones Justificadas (Fuera del Alcance)
      </div>
      <div class="space-y-1 text-[11px] text-ink/85">
        <div class="flex items-start gap-1.5">
          <div class="i-mdi-close-circle text-brand-red text-sm shrink-0 mt-0.5" />
          <span><b>Código Satélites:</b> Cero alteración a las 37 aplicaciones existentes.</span>
        </div>
        <div class="flex items-start gap-1.5">
          <div class="i-mdi-close-circle text-brand-red text-sm shrink-0 mt-0.5" />
          <span><b>App Móvil:</b> Restringido exclusivamente a terminales de escritorio Citrix.</span>
        </div>
        <div class="flex items-start gap-1.5">
          <div class="i-mdi-close-circle text-brand-red text-sm shrink-0 mt-0.5" />
          <span><b>Sincronización RRHH:</b> Catálogo desacoplado para asegurar plazos del semestre.</span>
        </div>
      </div>
    </div>
  </div>
</div>

</div>

<!--
Speaker Notes:
"Muchas gracias Catalina. Ningún diagnóstico es útil si no establece cómo se hará realidad. Dado que la política de MSC Chile prohíbe el acceso a datos productivos reales, nuestra estrategia de pruebas se basa en replicar el esquema DDL entregado por el cliente en un entorno de desarrollo local, poblándolo con un set de datos dummy representativo de 50 usuarios, 10 departamentos y las 37 aplicaciones. Esto nos permite simular el estrés del sistema sin poner en riesgo información corporativa sensible. Asimismo, delimitamos con estricta honestidad el alcance: queda fuera modificar las aplicaciones satélites o crear apps móviles innecesarias. Todo nuestro esfuerzo semestral está concentrado en entregar un portal administrativo veloz, seguro y totalmente compatible."
-->

---

<div class="flex-between items-center mb-1">
  <div class="eyebrow">Retorno de Inversión · Trazabilidad de Valor</div>
  <span class="chip bg-slate-100 text-xs font-semibold text-ink-soft border border-slate-200">
    <div class="i-mdi-finance mr-1 text-brand-navy" /> Retorno de Valor
  </span>
</div>

# <span class="i-mdi-trending-up align-middle mr-2 text-brand-navy" /> 6 · De la Necesidad al Valor del Negocio

<div class="grid grid-cols-3 gap-3 mt-2 text-xs">

<div class="card p-3 border-t-3 border-brand-red flex flex-col justify-between">
  <div>
    <div class="font-bold text-brand-red-dark text-sm mb-1.5 flex items-center gap-1">
      <div class="i-mdi-alert-octagon text-base text-brand-red" /> 1. El Dolor Inicial
    </div>
    <div class="p-2 rounded bg-slate-50 border border-slate-200 text-[11px] space-y-1 mb-2">
      <div>• Doble ventana abierta en paralelo.</div>
      <div>• Cotejo manual casilla por casilla.</div>
      <div>• Demoras de días en habilitar personal.</div>
      <div>• Base de datos SQL Server estresada.</div>
    </div>
  </div>
  <span class="chip bg-red-100 text-red-800 text-[10px] font-bold w-fit">Riesgo Alto</span>
</div>

<div class="card p-3 border-t-3 border-brand-navy flex flex-col justify-between">
  <div>
    <div class="font-bold text-brand-navy-dark text-sm mb-1.5 flex items-center gap-1">
      <div class="i-mdi-code-braces text-base text-brand-navy" /> 2. La Solución Técnica
    </div>
    <div class="p-2 rounded bg-slate-50 border border-slate-200 text-[11px] space-y-1 mb-2">
      <div>• Portal React optimizado para Edge.</div>
      <div>• .NET 10 + FastEndpoints (REPR).</div>
      <div>• Clonación y perfiles por cargo en 1 clic.</div>
      <div>• Redis Cache + Fachada de SPs.</div>
    </div>
  </div>
  <span class="chip bg-blue-100 text-blue-800 text-[10px] font-bold w-fit">Arquitectura Limpia</span>
</div>

<div class="card p-3 border-t-3 border-emerald-600 flex flex-col justify-between">
  <div>
    <div class="font-bold text-emerald-800 text-sm mb-1.5 flex items-center gap-1">
      <div class="i-mdi-check-all text-base text-emerald-600" /> 3. Impacto en MSC Chile
    </div>
    <div class="p-2 rounded bg-slate-50 border border-slate-200 text-[11px] space-y-1 mb-2">
      <div>• <b>Altas en segundos:</b> cero esperas.</div>
      <div>• <b>Cero error humano:</b> validación estricta.</div>
      <div>• <b>Auditoría 100% trazable:</b> quién y por qué.</div>
      <div>• <b>Continuidad 24/7 en puerto asegurada.</b></div>
    </div>
  </div>
  <span class="chip bg-emerald-100 text-emerald-800 text-[10px] font-bold w-fit">Valor Estratégico</span>
</div>

</div>

<div class="mt-3 p-3 rounded-xl bg-slate-900 text-white text-xs flex items-center justify-between shadow-lg">
  <div>
    <b class="text-brand-red-alt text-sm">Resultado del Diagnóstico:</b> Propuesta 100% viable, trazable y sin riesgo de interrupción operativa en puerto.
  </div>
</div>

<!--
Speaker Notes:
"Para concluir, como ilustra esta cadena de valor, el mérito de este diagnóstico radica en su trazabilidad total: cada decisión técnica responde directamente a un dolor del cliente. Transformamos la frustrante doble ventana de soporte en una clonación inteligente de un solo clic. Transformamos la asignación manual dispersa en perfiles basados en cargo. Y transformamos una base de datos estresada en una arquitectura moderna en .NET 10 con Redis que garantiza que los puertos de MSC sigan operando sin interrupciones. Como equipo de seis integrantes, dominamos cada arista de este análisis y estamos plenamente preparados para defender y justificar las decisiones de este diagnóstico. Muchas gracias por su atención, y abrimos la ronda para las preguntas del docente y la comisión."
-->

---
layout: end
transition: fade
---

<div class="flex-center flex-col text-center">

<div class="eyebrow mb-2">Proyecto Integrador · TIHV43 · Evaluación 1</div>

# ¡Muchas <span class="brand-gradient brand-gradient-flow">Gracias!</span>

<p class="mt-2 text-lg text-ink/75">Modernización Sistema RBAC · (MSC Chile)</p>

<div class="mt-8 p-3.5 rounded-lg bg-white/70 border border-black/5 text-xs text-ink/80">
  <b>Docente Guía:</b> Profesor Edgar López &nbsp;·&nbsp; <b>Institución:</b> INACAP Sede Central &nbsp;·&nbsp; <b>Año:</b> 2026
</div>

<div class="mt-6 text-base font-bold text-brand-red-dark flex items-center justify-center gap-2">
  <div class="i-mdi-comment-question-outline text-xl" /> Quedamos a disposición para responder las dudas que existan
</div>

</div>

<!--
Speaker Notes:
"Agradecemos al profesor Edgar López y a la comisión evaluadora por su tiempo. Quedamos a disposición para responder las dudas que existan."
-->
