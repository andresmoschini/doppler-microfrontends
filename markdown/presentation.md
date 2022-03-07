name: inverse
layout: true
class: center, middle, inverse
---
# Doppler Micro-Frontends 

---
name: hola
layout: false
## Hola...

.left-column.image-500h[
![Troy](troy.png)
]

.right-column[
Tal vez me recuerden de presentaciones cómo

* [_Eso_ que estuvo haciendo el equipo de tecnología de Doppler en 2018](https://andresmoschini.github.io/doppler-it-2018/#p1)

* [Desarmando Doppler](https://andresmoschini.github.io/desarmando-doppler/#p1)
]

---
name: seguir desarmando
layout: false
## Seguir desarmando...


???

Empezamos en 2018 y nos queda mucho camino que recorrer, recordemos por que lo estamos haciendo.

--

### ¿Por qué?

--

* Ciclo de vida independiente

???

Desde el punto de vista del usuario, Doppler es una sola aplicación. Pero desde el punto de vista de los desarrolladores (e idealmente también desde Soporte y Producto) Doppler puede estar compuesto de aplicaciones más pequeñas.

Desarrollarlas y publicarlas de forma independiente no solo permite lanzar mejoras más rápido, sino también reemplazar estas aplicaciones por otras de forma menos traumática.

--

* Simplicidad para la automatización de pruebas y despliegue

???

La ejecución de las pruebas de una aplicación muy grande lleva mucho tiempo, lo mismo que el despliegue. Preparar los entornos es una tarea compleja y es fácil mantener dependencias ya no utilizadas.

---
name: seguir desarmando 2
layout: false
## Seguir desarmando...

### ¿Por qué?

* Ciclo de vida independiente

* Simplicidad para la automatización de pruebas y despliegue

* Evolución tecnológica

???

Al ser aplicaciones más pequeñas, tienen menos dependencias, por lo que es más fácil reemplazar o actualizar partes (o incluso toda la aplicación)

--

* Evitar cuellos de botella

???

Al desplegarse de forma independiente, tenemos discrecionalidad para asignar los recursos a cada una de estas aplicaciones.

--

* Resiliencia

???

No hay un solo punto de fallo. Si una de las aplicaciones tiene problemas, en general, otras partes del sistema seguirán funcionando.


---
name: seguir desarmando 3
layout: false
## Seguir desarmando...

### ¿Por qué?

* Ciclo de vida independiente

* Simplicidad para la automatización de pruebas y despliegue

* Evolución tecnológica

* Evitar cuellos de botella

* Resiliencia

* Desarrollo más efectivo

--

  * Límites claros de la aplicación
  
???

En general, la aplicación provee una API y utiliza otras APIs o almacena datos.

Si dos equipos deben coordinarse, muchas veces es suficiente [con definir bien la API que utilizarán](https://jessitron.com/2021/08/02/better-coordination-or-better-software/) y estar listos para implementaciones parciales.

--

  * Código que _entra en tu cabeza_

???

Doppler es enorme. Es tentador re-usar código. Hay código muy complejo que no sabemos exactamente que hace, y las ramificaciones puede ir tan lejos que es muy difícil entender (al mismo tiempo) todo lo ocurre detrás de una operación (por ejemplo al guardar el contenido de una campaña).

Al tener aplicaciones más pequeñas, es más fácil reducir el contexto de una operación a algo más manejable.

Es cierto que ahora tenemos que tener en cuenta la interacción entre las aplicaciones, pero eso está en un nivel de abstracción superior así que, en este punto, podemos obviar los detalles.

---
name: twelve-factor app
layout: false
.black-center-200p[
[![Twelve Factor App](twelve-factor-app.png)](https://12factor.net)
]

.left-column-b[
**I. Código base (Codebase)**

**II. Dependencias**

**III. Configuraciones**

**IV. Backing services**

**V. Construir, desplegar, ejecutar**

**VI. Procesos**
]

.right-column-b[
**VII. Asignación de puertos**

**VIII. Concurrencia**

**IX. Desechabilidad**

**X. Paridad en desarrollo y producción**

**XI. Historiales**

**XII. Administración de procesos**
]

???

I. Un código base sobre el que hacer el control de versiones y multiples despliegues (1 repo <-> 1 app, o comparten código)

II. Declarar y aislar explícitamente las dependencias (Por ahora, en general estamos tratando de no tener nuestras propias librerías, y duplicar código o que se usen a nivel servicio)

III. Guardar la configuración en el entorno (No respetado en la WebApp)

IV. Tratar a los “backing services” como recursos conectables

V. Separar completamente la etapa de construcción de la etapa de ejecución (No respetado en WebApp, incluíamos la configuración en el build)

VI. Ejecutar la aplicación como uno o más procesos sin estado

VII. Publicar servicios mediante asignación de puertos

VIII. Escalar mediante el modelo de procesos

IX. Hacer el sistema más robusto intentando conseguir inicios rápidos y finalizaciones seguras

X. Mantener desarrollo, preproducción y producción tan parecidos como sea posible

XI. Tratar los historiales como una transmisión de eventos

XII. Ejecutar las tareas de gestión/administración como procesos que solo se ejecutan una vez


---
name: cómo estábamos
layout: false
## ¿Cómo estábamos?

.image-scaled[![Cómo estábamos](comoestabamos.png)]

???

A principios de 2019 ya teníamos la WebApp funcionando, con CI/CD y unit tests.

Queríamos hacer lo mismo con partes del backend.

---
name: cómo estamos
layout: false
## ¿Cómo estamos?

.image-scaled[![Cómo estamos](comoestamos.png)]

???

Desde entonces, logramos crear mini-APIs (algunos micro-servicios 
y otros no porque depende de la DB), todas dockerizadas, con CI/CD, tests unitarios y bastante desacopladas.

_En este gráfico no se ven detalles del DMS y de algunos servicios que trabajan
en background cómo el EAV. Esos servicios también están dockerizados y con CI/CD._

**Importante:** También esta la librería de UI, que la empezamos
para la WebApp pero se está usando también en Doppler, Sitios 
Editores

---
name: traefik
layout: false
## [Traefik](https://doc.traefik.io/traefik/)

.image-scaled[![Traefik](traefik-architecture.png)]

---
name: por que microfrontens
layout: false
## Cómo y por qué

.left-column-b[
## Esquema anterior (Solo WebApp)
]
.right-column-b[
## Micro-frontends (Con loader)
]
--
.left-column-b[
👎 Un nuevo monolito
]
.right-column-b[
👍 _Code that fits in your head_
]
--
.left-column-b[
👎 Build por entorno (p/incluir configuración)
]
.right-column-b[
🤯 Variables globales en `window` para configuraciones
]
--
.left-column-b[
🥱 Nuevo build para actualizar dependencias
]
.right-column-b[
🚀 Release independiente de cada micro-frontend
]
--
.left-column-b[
👎 Container con los archivos

> 💔 Cache
]
.right-column-b[
👍 Todos los archivos publicados a un CDN

> ❤️ Cache (browser y Cloudflare)
>
> 🤯 Rutas con hash / Manifest por versión
]

---
name: potenciales micro-frontends
layout: false
## Potenciales Micro-Frontends

🔥 Nuevas mini-webapps

* Editor de Unlayer (🚧 WIP)
* Login
* Checkout
* Panel de Control
* Otros editores

🔥 UI Library (🚧 WIP)

🔥 Otra lógica de frontend compartida

> Por ejemplo el nuevo trabajo de UTMs

🔥 Componentes reusables en distintas mini-webapps
  
> Menú de Doppler

---
name: detalles
layout: true
class: center, middle, inverse
---
# 🤓
# Los detalles...

_(Aprovechen para escapar los que ya se están aburriendo)_

???

Acá vamos a entrar en detalles sobre cómo se implementará técnicamente y cómo funcionará.

---
name: temas
layout: false
## Temas

* ¿Aplica Twelve Factos aquí?
* Hashes y Cache
* Manifests y Loader
* CI/CD y Versiones
* _"Micro-webapps"_ y Ruteo
* Multiples micro-frontends corriendo juntos
* Ejemplo completo

???

Proponer conversación sobre *12-factors*. Muchos puntos no aplican porque corre en el cliente.

En el punto del manifest vamos a ver lo de configuraciones.

En el punto de "multiples..." vamos a ver una contradicción.

En el punto del ruteo vamos a ver el equivalente de los puertos.

**Ordenar los puntos dependiendo del interés de la audiencia, hablar solo de los puntos necesarios.**

---
name: hashes-cache
layout: false
## Hashes y Cache

* ¿Que son y para que sirven esos molestos sufijos que se agregan a los nombres de archivos en el builds?

  Por ejemplo `main.d6c28435.js`

* Problemas publicando en un container (actual approach de webapp)

* Cloudflare

---
name: manifest-loader
layout: false
## Manifests y Loader

1. Build genera nombres de archivos con hash

2. Build genera un `asset-manifest-{version}.json` con referencias a eso archivos

  Nosotros usamos `asset-manifest-v1.json` para producción, `asset-manifest-main.json` para QA y `asset-manifest-INT`

3. Se publican esos archivos a una ubicación conocida

  Por ejemplo `https://cdn.fromdoppler.com/editors-webapp/asset-manifest-v1.json`

4. Donde se usa el micro-frontend, hacemos referencia a la URL absoluta del manifest y los cargamos con un script (_loader_)

  Para las mini-webapps tendremos archivos HTML estáticos donde hacemos eso.

  En esos HTMLs se definen las variables globales de `window` que configurarán las aplicaciones.

---
name: manifest
layout: false
## El manifest

.image-scaled[![Manifest](manifest.png)]

---
name: manifest-uso
layout: false
## Uso del manifest

.image-scaled[![Uso del Manifest](manifest-uso.png)]

---
name: ci-cd
layout: false
## CI/CD y Versiones

Es similar a [lo que ya hacemos con los micro-servicios](https://github.com/MakingSense/doppler-forms/blob/master/README.md#continuous-deployment-to-test-and-production-environments), pero subiendo archivos con hash y manifests en lugar de imágenes de Docker.

.image-scaled[![Version.txt](version-txt.png)]

---
name: routing
layout: false
## _"Micro-webapps"_ y Ruteo

* Traefik rutea al mismo HTML todas las rutas en un domínio con determinado prefijo ([ver configuración](https://github.com/MakingSense/doppler-swarm/blob/master/webapps-stack/docker-compose.yml#L44)).

  Por ejemplo `https://app.fromdoppler.com/editors/...`

* En el caso de Editors WebApp usamos React Router, por lo que tenemos que definir el basename ([ver ccódigo](https://github.com/FromDoppler/doppler-editors-webapp/blob/30241f739bce1e11e5c122348437590b38a4490a/src/index.tsx#L26)), es el prefijo de las URLs de la app, sin incluir el dominio.

  Por ejemplo `editors`

* Con React Router se utiliza el componente `<Link>` para los links internos y `<a href>` para los demás ([ver código](https://github.com/FromDoppler/doppler-editors-webapp/blob/30241f739bce1e11e5c122348437590b38a4490a/src/components/Main.tsx#L27-L28)).

  Por ejemplo:

  ```html
  <!-- Página interna de la WebApp de editores -->
  <!-- Navegación virtual manejada por JavaScript -->
  <!-- https://app.fromdoppler.com/editors/templates/1 -->
  <Link to="/templates/1">Template 1</Link>

  <!-- Página manejada por al WebApp de Doppler -->
  <!-- Navegación real manejada por el browser -->
  <!-- https://app.fromdoppler.com/login -->
  <a href="/login">Login</a>
  ```
  
---
name: multiples-frontend
layout: false
## Multiples micro-frontends corriendo juntos

### Ejemplos de _Micro-WebApps_

* WebApp de editores
* Potencial App de Login
* Potencial App de Checkout
* . . .

### Otros _Micro-Frontends_

* Librería de UI
* Potencial _Micro-Frontend_ con el menú de Doppler
* Potencial _Micro-Frontend_ con el manejo de UTMs
* Potencial _Micro-Frontend_ con el manejo de la sesión
* . . .

???

Los *micro-servicios* corren en espacios de memoria separados, y la comunicación entre ellos tiene que ser por canales explícitos.

En cambio, los *micro-frontends* son mucho más permeables ya que comparten un dominio (posibilidad de compartir cookies y storage) y podrían compartir un scope global de JavaScript y el DOM.

---
name: actores
layout: false
## Actores y sistemas

.image-scaled[![Actores](actores.png)]

---
name: doppler-mvc
layout: false

.image-630h[![Doppler MVC](diagram1-doppler.png)]

---
name: doppler-mvc-ui-lib
layout: false

.image-630h[![Doppler MVC c/UI Library](diagram2-doppler-ui-lib.png)]

---
name: mseditor
layout: false

.image-630h[![MSEditor](diagram3-mseditor.png)]

---
name: webapp
layout: false

.image-630h[![WebApp](diagram4-webapp.png)]

---
name: editor-webapp
layout: false

.image-630h[![Editor WebApp](diagram5-nuevo-editor.png)]

---
name: fin
layout: true
class: center, middle, black
background-image: url(end.gif)
background-size: contain
---
# The end
