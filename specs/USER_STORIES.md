# User Stories — Hermes Landing Builder

## US-1: Registro de Usuario
**COMO** usuario nuevo
**QUIERO** registrarme con email y contraseña
**PARA** acceder al panel de constructor de páginas

**Criterios de aceptación:**
- [ ] Formulario de registro con email, nombre y contraseña
- [ ] Validación de email (formato válido, no duplicado)
- [ ] Contraseña mínima 8 caracteres
- [ ] Creación automática de usuario en BD
- [ ] Redirección al dashboard tras registro exitoso
- [ ] Mostrar errores de validación (email ya registrado, contraseña débil)

## US-2: Inicio de Sesión
**COMO** usuario registrado
**QUIERO** iniciar sesión con mis credenciales
**PARA** acceder a mis proyectos de landing pages

**Criterios de aceptación:**
- [ ] Formulario de login con email y contraseña
- [ ] JWT token devuelto en respuesta exitosa
- [ ] Token almacenado en localStorage
- [ ] Redirección al dashboard tras login
- [ ] Mostrar error si credenciales inválidas
- [ ] Protección de rutas con AuthGuard

## US-3: Crear un Proyecto de Landing Page
**COMO** usuario autenticado
**QUIERO** crear un nuevo proyecto de landing page
**PARA** empezar a diseñar mi página desde un template

**Criterios de aceptación:**
- [ ] Botón "Nuevo Proyecto" en el dashboard
- [ ] Modal/formulario con nombre del proyecto, slug y selección de template
- [ ] Template por defecto "Blank" (sin secciones precargadas)
- [ ] 4+ templates predefinidos (Startup, Portfolio, Evento, SaaS)
- [ ] Al crear, redirigir al editor del proyecto
- [ ] Proyecto creado en estado "draft"

## US-4: Editor de Secciones
**COMO** usuario autenticado
**QUIERO** añadir, reordenar y eliminar secciones en mi landing page
**PARA** construir la estructura visual de mi página

**Criterios de aceptación:**
- [ ] Panel lateral con tipos de sección disponibles (Hero, Features, Pricing, Testimonials, FAQ, CTA, Footer)
- [ ] Botón "Añadir sección" que agrega al final del layout
- [ ] Reordenación drag-and-drop con actualización de posición
- [ ] Botón eliminar sección con confirmación
- [ ] Cada sección muestra su tipo y contenido precargado
- [ ] Posiciones persistidas al servidor tras cada cambio
- [ ] Loading state mientras se guarda la ordenación

## US-5: Personalizar Componentes de Sección
**COMO** usuario autenticado
**QUIERO** editar el contenido, colores y estilos de cada sección
**PARA** adaptar el diseño a mi marca

**Criterios de aceptación:**
- [ ] Click en una sección abre panel de edición lateral
- [ ] Editor de texto para títulos, subtítulos y cuerpo
- [ ] Selector de color para fondo, texto y acento de la sección
- [ ] Campos específicos por tipo (ej: precio en Pricing, testimonio en Testimonials)
- [ ] Cambios visibles en la previsualización en tiempo real
- [ ] Botón "Reset" para restaurar valores por defecto de la sección
- [ ] JSONB flexible schema para almacenar datos variables por tipo de sección

## US-6: Vista Previa en Tiempo Real
**COMO** usuario autenticado
**QUIERO** ver una previsualización en vivo de mi landing page mientras edito
**PARA** saber exactamente cómo se verá publicada

**Criterios de aceptación:**
- [ ] Panel de previsualización junto al editor (split screen en desktop)
- [ ] Actualización automática al cambiar contenido en el editor
- [ ] Botón para alternar entre vista desktop y mobile (375px)
- [ ] La previsualización renderiza todas las secciones ordenadas
- [ ] Los colores y estilos editados se reflejan instantáneamente
- [ ] Empty state si no hay secciones: "Añade tu primera sección"

## US-7: Publicar Landing Page
**COMO** usuario autenticado
**QUIERO** publicar mi landing page para hacerla accesible públicamente
**PARA** compartirla con mi audiencia

**Criterios de aceptación:**
- [ ] Botón "Publicar" visible solo en estado "draft"
- [ ] Confirmación: "¿Estás seguro de publicar esta página?"
- [ ] Al publicar, se genera una URL pública única: `/p/[project-slug]`
- [ ] Estado cambia de "draft" → "published"
- [ ] Botón "Despublicar" visible en estado "published" que revierte a "draft"
- [ ] Loading state mientras se publica
- [ ] Toast de éxito: "¡Página publicada!"
- [ ] Error state: "Error al publicar" + reintentar

## US-8: Configurar SEO
**COMO** usuario autenticado
**QUIERO** configurar meta tags, título SEO y descripción de mi landing page
**PARA** mejorar el posicionamiento en buscadores

**Criterios de aceptación:**
- [ ] Pestaña "SEO" en el editor del proyecto
- [ ] Campos: Título SEO, Meta Descripción, Palabras Clave
- [ ] Campos: Open Graph title, description, image URL
- [ ] Vista previa de cómo se verá en Google Search y redes sociales
- [ ] Validación: título SEO máx. 60 caracteres, descripción máx. 160 caracteres
- [ ] Persistencia automática al cambiar campos (debounced save)
- [ ] OG tags renderizados en el header de la página pública

## US-9: Dashboard de Proyectos
**COMO** usuario autenticado
**QUIERO** ver todos mis proyectos en un dashboard con su estado y estadísticas
**PARA** gestionar mis landing pages de un vistazo

**Criterios de aceptación:**
- [ ] Lista/tarjetas de todos los proyectos del usuario
- [ ] Cada tarjeta muestra: nombre, estado (draft/published), última modificación, visitas totales
- [ ] Búsqueda por nombre de proyecto
- [ ] Filtro por estado (todos, draft, published)
- [ ] Botones: Editar, Publicar/Despublicar, Eliminar, Ver página pública
- [ ] Loading skeleton mientras cargan proyectos
- [ ] Empty state: "No tienes proyectos aún. ¡Crea tu primer landing page!"
- [ ] Error state: "Error al cargar proyectos" + botón reintentar

## US-10: Panel de Analíticas
**COMO** usuario autenticado
**QUIERO** ver estadísticas de visitas de mis landing pages publicadas
**PARA** medir el rendimiento de mis campañas

**Criterios de aceptación:**
- [ ] Pestaña "Analíticas" en el editor del proyecto
- [ ] Total de visitas (all-time)
- [ ] Gráfico de visitas por día (últimos 7/30 días)
- [ ] Tabla de visitas recientes con fecha y hora
- [ ] Loading skeleton mientras cargan datos
- [ ] Empty state para proyectos no publicados: "Publica tu página para ver analytics"
- [ ] Error state si falla la carga

## US-11: Template Library
**COMO** usuario autenticado
**QUIERO** explorar y usar templates predefinidos para mis landing pages
**PARA** empezar rápidamente con diseños profesionales

**Criterios de aceptación:**
- [ ] Galería de templates en el dashboard (grid de tarjetas)
- [ ] Cada template muestra nombre, descripción y tipo
- [ ] Vista previa del template antes de seleccionarlo
- [ ] Al seleccionar un template, se crea proyecto con secciones precargadas
- [ ] Template "Blank" crea proyecto vacío sin secciones
- [ ] 4+ templates predefinidos con contenido demo
- [ ] Loading state mientras se carga la galería
