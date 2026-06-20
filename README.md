<div align="center">

  <h1>Sistema Vencana</h1>
  <h3>Control de Inventario y Herramientas de Perforación Direccional</h3>
  
  <p align="center">
    <img src="https://img.shields.io/badge/Estado-Producción-00A2ED?style=flat-square&logoColor=white" alt="Estado" />
    <img src="https://img.shields.io/badge/Arquitectura-Capas_|_MVI-0A192F?style=flat-square&logoColor=white" alt="Arquitectura" />
    <img src="https://img.shields.io/badge/Base_de_Datos-MySQL-E3E9F0?style=flat-square&logoColor=0A192F&labelColor=E3E9F0&color=E3E9F0" alt="Database" />
  </p>

  <br>
  
  <blockquote>
    <b>Nota de Confidencialidad:</b> Por protección de la propiedad intelectual y la lógica operativa del departamento, el código fuente se mantiene privado. Este documento exhibe la arquitectura, el stack tecnológico y los flujos resueltos por el sistema.
  </blockquote>

</div>

<br>

<h2>VISIÓN GENERAL</h2>

Plataforma integral diseñada para el departamento de Coordinación de Perforación Direccional, enfocada en el control preciso y la trazabilidad del inventario rotativo. El sistema automatiza y centraliza la gestión del ciclo de vida operativo: desde la categorización y mantenimiento de componentes, su estructuración en ensambles (BHA), el seguimiento de despliegues en campo y la administración de pozos, hasta el control de acceso de los usuarios y el registro de corridas.

<br>

<h2>ARQUITECTURA Y STACK</h2>

El proyecto está dividido en dos grandes módulos, utilizando el ecosistema de **Kotlin** de extremo a extremo para garantizar un desarrollo fluido, unificado y altamente escalable.

<table width="100%">
  <tr>
    <td width="33%" align="center">
      <b>Frontend (MVI)</b><br><br>
      <img src="https://img.shields.io/badge/Kotlin-005BB5?style=flat-square&logo=kotlin&logoColor=white" /> <br>
      <img src="https://img.shields.io/badge/Compose_Multiplatform-005BB5?style=flat-square&logo=jetpackcompose&logoColor=white" /> <br>
      <img src="https://img.shields.io/badge/Patrón_MVI-005BB5?style=flat-square&logoColor=white" />
    </td>
    <td width="33%" align="center">
      <b>Backend (Monolito)</b><br><br>
      <img src="https://img.shields.io/badge/Kotlin-0A192F?style=flat-square&logo=kotlin&logoColor=white" /> <br>
      <img src="https://img.shields.io/badge/Spring_Boot-0A192F?style=flat-square&logo=springboot&logoColor=white" /> <br>
      <img src="https://img.shields.io/badge/MySQL_&_JPA-0A192F?style=flat-square&logo=mysql&logoColor=white" />
    </td>
    <td width="33%" align="center">
      <b>Seguridad & Auditoría</b><br><br>
      <img src="https://img.shields.io/badge/JWT_Auth-808080?style=flat-square&logo=jsonwebtokens&logoColor=white" /> <br>
      <img src="https://img.shields.io/badge/Spring_Security-808080?style=flat-square&logo=springsecurity&logoColor=white" /> <br>
      <img src="https://img.shields.io/badge/AOP_Audit-808080?style=flat-square&logoColor=white" />
    </td>
  </tr>
</table>

<br>

<h2>MÓDULOS DEL SISTEMA</h2>

El flujo de la aplicación modela la realidad de las operaciones de perforación, dividiendo sus responsabilidades en:

<table width="100%">
  <tr>
    <td width="50%">
      <b>Gestión de Componentes</b><br>
      Registro de herramientas, control de mantenimientos (preventivos y correctivos) y manejo de estados (activo/inhabilitado).
    </td>
    <td width="50%">
      <b>Control de Ensambles (BHA)</b><br>
      Agrupación lógica de componentes bajo estrictas reglas de compatibilidad (medidas, modelos, cantidad de piezas).
    </td>
  </tr>
  <tr>
    <td width="50%">
      <b>Despliegues y Corridas en Pozo</b><br>
      Asignación de ensambles a operaciones en locaciones específicas, con registro detallado de las corridas ejecutadas.
    </td>
    <td width="50%">
      <b>Auditoría y Reportes</b><br>
      Historial inmutable de interacciones del sistema, generación de registros en PDF y procesamiento de inventarios masivos vía Excel.
    </td>
  </tr>
  <tr>
    <td width="50%">
      <b>Gestión de Pozos</b><br>
      Administración centralizada de locaciones y áreas de perforación para la correcta asignación topográfica de las herramientas desplegadas.
    </td>
    <td width="50%">
      <b>Gestión de Usuarios</b><br>
      Control de acceso basado en roles y permisos, garantizando que las acciones críticas y modificaciones del inventario sean ejecutadas únicamente por personal autorizado.
    </td>
  </tr>
</table>

<br>

<h2>RETOS TÉCNICOS SUPERADOS</h2>

<p>La complejidad del dominio petrolero requirió soluciones arquitectónicas específicas para garantizar la integridad de los datos:</p>

<details>
  <summary><kbd>Ver detalle</kbd> <b>Auditoría de Acciones Críticas con AOP</b></summary>
  <blockquote>
    Para no contaminar la lógica de negocio en los <code>Services</code>, se implementó <b>Programación Orientada a Aspectos (AOP)</b> con anotaciones personalizadas (<code>@Auditable</code>). Esto permite interceptar los cambios de estado en componentes y ensambles, registrando automáticamente en la base de datos el usuario, la acción y la fecha exacta del movimiento.
  </blockquote>
</details>

<details>
  <summary><kbd>Ver detalle</kbd> <b>Manejo de Estado Complejo (MVI) en UI</b></summary>
  <blockquote>
    La asignación dinámica de componentes a los distintos tipos de ensambles genera múltiples combinaciones de validación en la interfaz debido a las posibilidades de arquitectura segun modelos o cantidad de piezas. El uso del patrón <b>MVI (Model-View-Intent)</b> centralizó todas estas validaciones en un único flujo de estado unidireccional, evitando condiciones de carrera al actualizar listas filtradas desde el backend y manteniendo la UI siempre predecible.
  </blockquote>
</details>

<details>
  <summary><kbd>Ver detalle</kbd> <b>Integridad del Inventario Rotativo</b></summary>
  <blockquote>
    El backend gestiona validaciones transaccionales severas para evitar que componentes inhabilitados o ya en uso sean asignados a nuevos ensambles o despliegues en campo simultáneos, asegurando una "única fuente de la verdad" operativa.
  </blockquote>
</details>

<br>

<h2>IMPACTO EN EL NEGOCIO</h2>

* <kbd>Trazabilidad</kbd> **Control de Vida Útil:** Seguimiento exacto desde el almacén hasta la locación del pozo, previniendo fallas por falta de mantenimiento.
* <kbd>Seguridad</kbd> **Historial Inmutable:** Visibilidad total sobre quién y cuándo modificó registros sensibles del inventario.
* <kbd>Optimización</kbd> **Decisiones Rápidas:** Las interfaces reactivas y filtros de estado permiten al departamento de coordinación localizar disponibilidad de herramientas en tiempo real.

<br>

<h2>DEMOSTRACIÓN VISUAL</h2>

<div align="center">
  <i>A continuación se adjunta el link hacia un video de muestra del funcionamiento de la interfaz y gestión de componentes.</i>
  <br><br>
  <a href="https://youtu.be/2TNs9WWyPsI" target="_blank">
    <img src="https://img.shields.io/badge/Ver_Video_en-YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white" alt="Demostración en YouTube" />
  </a>
</div>
