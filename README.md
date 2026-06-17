# Foodstore — Admin (panel de administración)

**Video de presentación:** [carpeta en Google Drive](https://drive.google.com/drive/folders/1VGVLJdY9Qo6D388iF_YlcTaDRHvbVLUC?usp=drive_link)

Panel de administración del proyecto **Foodstore**. Permite al staff gestionar el catálogo (productos, categorías e ingredientes), controlar stock, administrar pedidos por estado, consultar métricas del negocio en un dashboard con gráficos y administrar usuarios según rol.

Consume la API del backend en [`IntegradorBackend`](../IntegradorBackend). La tienda del cliente está en [`store_final`](../store_final).

## Stack

| Categoría | Tecnología |
|-----------|------------|
| Framework UI | React 19 |
| Lenguaje | TypeScript |
| Bundler / dev server | Vite 8 |
| Routing | React Router DOM 7 |
| Datos del servidor | TanStack Query 5 |
| Formularios | TanStack Form |
| Tablas | TanStack Table |
| Gráficos | Recharts |
| Estado de cliente | Zustand (sesión y autenticación) |
| HTTP | Axios (`withCredentials` para cookie JWT) |
| Estilos | Tailwind CSS 4 |
| Linting | ESLint + TypeScript ESLint |

## Requisitos previos

- [Node.js](https://nodejs.org/) (LTS recomendado)
- [pnpm](https://pnpm.io/)
- Backend corriendo en `http://localhost:8000` (ver [`IntegradorBackend`](../IntegradorBackend))

## Cómo correr en local

1. Entrá a la carpeta del proyecto:

```bash
cd IntegradorAdmin
```

2. Instalá dependencias:

```bash
pnpm install
```

3. Configurá variables de entorno copiando `.env.example` a `.env` y ajustando los valores si hace falta:

```env
VITE_API_URL=http://localhost:8000
VITE_WS_URL=ws://localhost:8000/api/v1/ws/pedidos
```

4. Levantá el servidor de desarrollo:

```bash
pnpm dev
```

5. Abrí en el navegador la URL que muestra la terminal (por defecto `http://localhost:5173`).

**Usuario admin de prueba** (creado por el seed del backend):

```txt
email: admin@admin.com
password: admin123
```

### Otros comandos

```bash
pnpm build    # build de producción
pnpm preview  # previsualizar el build
pnpm lint     # ESLint
```

## Qué hay en el repositorio

Arquitectura por **features** más código compartido en `shared/`:

```
src/
  app/             # main.tsx, App.tsx
  router/          # AppRouter y rutas protegidas por rol
  shared/          # cliente Axios, NavBar, ProtectedRoute, useForm
  features/
    auth/          # login y sesión
    catalog/       # productos, categorías, ingredientes
    orders/        # tablero de pedidos y tiempo real
    statistics/    # dashboard de métricas y gráficos
    admin-users/   # gestión de usuarios
    profile/       # perfil del usuario logueado
    uploads/       # subida de imágenes a Cloudinary
```

**Módulos principales**

- **Catálogo:** CRUD de categorías (con subcategorías), ingredientes y productos; upload de imágenes vía Cloudinary.
- **Pedidos:** tablero kanban por estado, cambio de estado y actualización en vivo con WebSocket.
- **Estadísticas:** KPIs y gráficos de ventas, productos top, pedidos por estado e ingresos por forma de pago.
- **Usuarios:** administración de usuarios y roles (solo `ADMIN`).
- **Auth y roles:** acceso restringido a `ADMIN`, `STOCK` y `PEDIDOS`; rutas anidadas con permisos por pantalla.

Los imports usan el alias `@/` → `src/`.

## Roles

| Rol | Acceso principal |
|-----|------------------|
| `ADMIN` | Acceso completo: catálogo, pedidos, dashboard, usuarios |
| `STOCK` | Catálogo y modificación de stock/disponibilidad |
| `PEDIDOS` | Catálogo en lectura y gestión de pedidos |
| `CLIENT` | No puede acceder al panel |

## Rutas

| Ruta | Descripción | Rol |
|------|-------------|-----|
| `/login` | Inicio de sesión | Público |
| `/` | Listado de productos | ADMIN, STOCK, PEDIDOS |
| `/products/:id` | Detalle de producto | ADMIN, STOCK, PEDIDOS |
| `/categories` | Categorías y subcategorías | ADMIN, STOCK, PEDIDOS |
| `/ingredients` | Ingredientes | ADMIN, STOCK, PEDIDOS |
| `/orders` | Tablero de pedidos | ADMIN, PEDIDOS |
| `/dashboard` | Métricas y gráficos | ADMIN |
| `/users` | Administración de usuarios | ADMIN |
| `/profile` | Perfil del usuario | ADMIN, STOCK, PEDIDOS |

## Documentación adicional

- [`ADMIN_APP_FRONTEND_FLUJO.md`](./ADMIN_APP_FRONTEND_FLUJO.md) — flujo funcional del panel
- [`GUIA-TRANSICION-ARQUITECTURA-ADMIN.md`](./GUIA-TRANSICION-ARQUITECTURA-ADMIN.md) — mapa de la arquitectura por features
