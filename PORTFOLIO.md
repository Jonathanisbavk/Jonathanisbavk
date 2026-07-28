# Portafolio técnico — Jonathan Gutiérrez Quispe

Catálogo de mis proyectos públicos, ordenado por relevancia técnica y no por fecha.
Cada ficha indica qué resuelve, con qué está construido y qué decisión de ingeniería
sostiene. Los proyectos de práctica y los ejercicios de curso están identificados como
tales al final: prefiero que se vea la diferencia a inflar el listado.

**Contacto:** [jonathan9579285@gmail.com](mailto:jonathan9579285@gmail.com) ·
[LinkedIn](https://www.linkedin.com/in/jgutierrezq/) ·
[GitHub](https://github.com/Jonathanisbavk)

---

## Índice

- [Arquitectura de microservicios (Java / Spring)](#arquitectura-de-microservicios-java--spring)
- [Blockchain y Web3](#blockchain-y-web3)
- [Inteligencia artificial aplicada](#inteligencia-artificial-aplicada)
- [Productos web full stack](#productos-web-full-stack)
- [Frontend y sitios de negocio](#frontend-y-sitios-de-negocio)
- [Trabajo académico y primeros proyectos](#trabajo-académico-y-primeros-proyectos)
- [Resumen de tecnologías](#resumen-de-tecnologías)

---

## Arquitectura de microservicios (Java / Spring)

Ecosistema de gestión para barberías, repartido en servicios independientes con registro
y descubrimiento centralizado. Es el conjunto donde mejor se ve cómo separo
responsabilidades entre servicios y cómo conecto un frontend SPA con un backend Java.

### [barber-service](https://github.com/Jonathanisbavk/barber-service)

API REST principal del dominio: barberos, clientes, catálogo de servicios y citas.

- **Spring Boot · Java 21 · Gradle y Maven**
- Integración con **LangChain4j** para respuestas generadas por LLM, con degradación a
  una respuesta por defecto cuando no hay clave de API configurada. El servicio no se
  cae por la ausencia de un proveedor externo.

### [barber-dashboard-service](https://github.com/Jonathanisbavk/barber-dashboard-service)

Servicio de panel y reportería, separado del servicio de dominio.

- **Spring Boot · Spring Data JPA · MariaDB · Maven**
- Persistencia con JPA sobre MariaDB y exposición REST propia, de modo que la carga de
  consultas analíticas no compite con la operación transaccional.

### [eureka.server](https://github.com/Jonathanisbavk/eureka.server)

Servidor de descubrimiento de servicios del ecosistema.

- **Spring Cloud Netflix Eureka · Spring Cloud 2023.0.3**
- Registro y localización de los servicios anteriores sin direcciones fijas entre ellos,
  que es lo que permite escalarlos o moverlos sin tocar configuración de los clientes.

### [barber-frontend-angular](https://github.com/Jonathanisbavk/barber-frontend-angular)

Panel de administración web que consume la API: gestión de barberos, clientes y
servicios, agenda de citas y punto de venta con emisión de boletas y facturas con IGV.

- **Angular 21 · TypeScript 5.9 · Angular Material 3 · RxJS · SCSS**
- Nace como **migración de un frontend en HTML/CSS/JS vanilla** a una SPA con
  arquitectura por features, componentes standalone y estado reactivo con signals,
  manteniendo paridad funcional y visual exacta con la versión anterior. La versión
  original sigue publicada en
  [barber-fronted](https://github.com/Jonathanisbavk/barber-fronted), de modo que el
  antes y el después son comparables.

---

## Blockchain y Web3

Sistema de taquilla donde cada entrada es un NFT y cada evento queda registrado en
cadena. Es el proyecto que construí **dos veces**, en dos frameworks distintos, contra
un mismo backend, para contrastar ambos ecosistemas resolviendo un dominio idéntico.

### [frontend_productos_angular](https://github.com/Jonathanisbavk/frontend_productos_angular) — TicketChain

Dashboard de taquilla: gestión de cartelera, registro en cadena, ciclo de vida del
boleto NFT y comprobante en PDF publicado en IPFS.

- **Angular 21 en modo zoneless · Angular Material 3 · ethers v6 · TypeScript**
- Sin `zone.js`: signals como única fuente de estado y `OnPush` en todos los componentes.
  Bundle inicial de **79 kB** comprimidos.
- Arquitectura por capas `core` / `shared` / `features` con dependencias en un solo
  sentido; los componentes nunca acceden a `HttpClient` ni instancian contratos.
- Las transacciones se firman **en el navegador** con MetaMask: el backend no custodia
  claves privadas en ningún momento.
- Validación on-chain que relee el contrato sin gastar gas y compara campo a campo
  contra la base de datos para demostrar que el dato no fue manipulado.

### [fronted_productos_conciertos](https://github.com/Jonathanisbavk/fronted_productos_conciertos)

La implementación gemela del mismo producto, previa a la de Angular.

- **Next.js · React · TypeScript · Tailwind CSS · shadcn/ui**
- Mismo backend, mismo dominio, distinto ecosistema. Comparar ambos repositorios muestra
  cómo se traslada una misma arquitectura entre dos modelos de reactividad diferentes.

### [mod_02_productos_backend](https://github.com/Jonathanisbavk/mod_02_productos_backend)

El backend que ambos frontends consumen, contratos incluidos.

- **Node.js · Express · Solidity · Hardhat · MariaDB · IPFS**
- Contratos `Events` y `EventoNFT` (ERC-721), scripts de despliegue, generación de PDF
  con código QR de verificación y publicación en IPFS.
- Incluye colección de Postman y documentación de la API.

---

## Inteligencia artificial aplicada

Proyectos donde la IA es parte del producto y no una ayuda de desarrollo: pipelines de
extracción, búsqueda semántica y lectura automatizada de documentos.

### [hackathon-unsa-desafio1](https://github.com/Jonathanisbavk/hackathon-unsa-desafio1) — CONECTA UNSA

Plataforma de empleo que conecta egresados universitarios con ofertas laborales
relevantes y con transparencia salarial. Desafío Hackathon UNSA 2026.

**[Ver en producción](https://hackathon-unsa-desafio1.vercel.app)**

- **Next.js · React · TypeScript · Supabase · PostgreSQL · pgvector · Vercel**
- **Pipeline de extracción con IA**: Gemini 2.0 Flash como extractor principal y
  **fallback a Groq/Llama 3.1** para tolerancia a fallos. Convierte texto crudo de
  correos en ofertas estructuradas, descartando ruido automáticamente.
- **Recomendación semántica** con embeddings `text-embedding-004` y similitud coseno
  sobre pgvector.
- Alertas por palabra clave y un mecanismo que sugiere al empleador publicar el salario
  cuando falta.
- Opera íntegramente sobre planes gratuitos, por ser un proyecto para una institución
  pública.

### [web-becas-unsa](https://github.com/Jonathanisbavk/web-becas-unsa) — UNSA Becas

Portal que reúne las becas y convocatorias de la universidad, las lee con IA desde los
afiches publicados y las presenta limpias, buscables y filtrables.

- **Next.js · TypeScript · Prisma · Tailwind CSS**
- **Scraping multifuente** de los portales oficiales, con caída a datos en caché cuando
  la web de origen no responde.
- **IA según el formato del documento**: visión para leer afiches de baja calidad, texto
  para estructurar convocatorias. Extrae título, requisitos, cobertura, nivel y fecha
  límite.
- Panel de administración con historial de ejecuciones de scraping y control de
  publicación.

### [mentoria-frontend](https://github.com/Jonathanisbavk/mentoria-frontend)

Plataforma de gestión de mentorías con autenticación, agenda y notificaciones.

- **Next.js · TypeScript · Supabase · Zustand · Google APIs · Resend**
- Integración con Google APIs para calendario y envío transaccional de correo con Resend.
- Es mi proyecto más extenso en TypeScript por volumen de código.

### [web-dashboard-marketing](https://github.com/Jonathanisbavk/web-dashboard-marketing)

Dashboard de campaña en redes sociales con asistente conversacional integrado.

- **JavaScript · API de Gemini · Vercel**

---

## Productos web full stack

### [mini-erp-nextjs](https://github.com/Jonathanisbavk/mini-erp-nextjs)

Sistema de gestión para pequeñas empresas: CRM, inventario, punto de venta y reportes
ejecutivos.

- **Next.js 14 · Supabase · PostgreSQL · Tailwind CSS · Recharts**
- Incluye esquema SQL versionado y dashboard con gráficos.

### [ruti-app](https://github.com/Jonathanisbavk/ruti-app)

Prototipo móvil de formalización y seguridad para conductores de transporte en
Lima y Callao.

- **Next.js 16 · React 19 · TypeScript · Tailwind CSS 4 · Zustand**
- Registro por voz mediante agente conversacional, alertas de vencimiento de documentos
  (SOAT, licencia), geolocalización y sistema de reputación gamificado.
- Diseñado mobile-first, con persistencia en `localStorage` por tratarse de un prototipo
  de evaluación.

### [agenda-flit](https://github.com/Jonathanisbavk/agenda-flit)

Sustituye la agenda lineal de un evento (un único HTML exportado de WordPress) por una
aplicación donde cada asistente arma su propio plan y descarga un **cronograma
personalizado en PDF**.

- **TypeScript · Python (herramientas de extracción) · HTML**
- Conserva la identidad gráfica oficial del evento y parte del HTML original como fuente
  de datos.

### [web-veterinaria-saas](https://github.com/Jonathanisbavk/web-veterinaria-saas)

Aplicación de gestión veterinaria en modelo SaaS.

- **React · Vite · Firebase (Firestore, Functions, reglas de seguridad)**
- Incluye reglas de Firestore e índices versionados en el repositorio.

### [backend-bodega](https://github.com/Jonathanisbavk/backend-bodega)

Interfaz de gestión para una bodega.

- **Vue 3 · Vite**

### [web-finance-calculator-mkt](https://github.com/Jonathanisbavk/web-finance-calculator-mkt)

Calculadora financiera orientada a marketing, desplegada en Netlify.

- **Next.js · TypeScript**

---

## Frontend y sitios de negocio

Trabajo de front-end orientado a conversión, con foco en diseño, rendimiento y
posicionamiento.

### [website-pasteleria](https://github.com/Jonathanisbavk/website-pasteleria)

Diseño y desarrollo del sitio de una pastelería de tortas personalizadas, con la llamada
a la acción y la conversión como objetivo principal.

- **React · Firebase**

### [venta-motos-kids](https://github.com/Jonathanisbavk/venta-motos-kids)

Landing de venta con trabajo de SEO técnico: `sitemap.xml`, `robots.txt` y verificación
de propiedad en Google Search Console.

- **HTML · CSS**

### [hackaton-juego-ia](https://github.com/Jonathanisbavk/hackaton-juego-ia) — Tiko Rush

Juego web desarrollado en hackathon.

- **Next.js · React · TypeScript**

### [miportafolio-jonathangutierrez](https://github.com/Jonathanisbavk/miportafolio-jonathangutierrez) · [web-jonathan-portfolio](https://github.com/Jonathanisbavk/web-jonathan-portfolio)

Dos iteraciones de mi portafolio personal: la primera en HTML y CSS, la segunda migrada
a **React · TypeScript · Vite**.

---

## Trabajo académico y primeros proyectos

Los incluyo por transparencia cronológica. No representan mi nivel actual.

- **[Proyecto-DuoPie](https://github.com/Jonathanisbavk/Proyecto-DuoPie)** — Java.
  Proyecto del curso de Algoritmos para la Solución de Problemas.
- **[technologi](https://github.com/Jonathanisbavk/technologi)** y
  **[app-backend-technologi](https://github.com/Jonathanisbavk/app-backend-technologi)** —
  sitio de consultoría con backend en Node.js. 2023.
- **[app-elsapato-shoes](https://github.com/Jonathanisbavk/app-elsapato-shoes)** —
  aplicación de venta de calzado.
- **[barber-fronted](https://github.com/Jonathanisbavk/barber-fronted)** y
  **[barber-dashboard-fronted](https://github.com/Jonathanisbavk/barber-dashboard-fronted)** —
  frontend original de la barbería en HTML/CSS/JS vanilla, conservado como referencia de
  la migración a Angular.
- **[portafoliojonathangutierrez.github.io](https://github.com/Jonathanisbavk/portafoliojonathangutierrez.github.io)**
  y **[jonathanisback.github.io](https://github.com/Jonathanisbavk/jonathanisback.github.io)** —
  primeras páginas personales, 2022 y 2023.

---

## Resumen de tecnologías

| Área             | Tecnologías                                                                 |
| ---------------- | --------------------------------------------------------------------------- |
| Lenguajes        | TypeScript, JavaScript, Java, Kotlin, Python, SQL, Solidity                 |
| Frontend         | Angular 21, React, Next.js, Vue 3, Angular Material 3, Tailwind CSS, SCSS   |
| Backend          | Spring Boot, Spring Data JPA, Spring Cloud (Eureka), Node.js, Express        |
| Bases de datos   | PostgreSQL, MariaDB, MongoDB, Supabase, Firebase Firestore, pgvector         |
| Web3             | Solidity, ethers.js, Hardhat, ERC-721, IPFS                                 |
| IA               | Gemini, Groq/Llama, LangChain4j, embeddings y búsqueda semántica            |
| Herramientas     | Git, Maven, Gradle, Vite, Vercel, Netlify, Firebase, Postman                |

---

*Documento disponible en español. Puedo facilitar una versión en inglés si el proceso lo
requiere.*
