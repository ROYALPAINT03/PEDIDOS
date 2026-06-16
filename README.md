# Sistema de Pedidos Inteligente - Pinturas OSI & SISI

Este proyecto consiste en una aplicación web responsiva y PWA (Progressive Web App) diseñada para la toma de pedidos rápida y eficiente de Pinturas OSI y SISI. La aplicación cuenta con un asistente de voz inteligente que analiza lenguaje natural libre e integración directa con Supabase para la base de datos de vendedores y clientes.

## 📄 Archivo Principal de la Aplicación

El único archivo activo de la aplicación es:
*   **[pedidos.html](file:///c:/Users/Etiqueta/Desktop/PEDIDOS-main/pedidos.html)**

> [!IMPORTANT]
> El archivo `pedidosnew.html` ha sido eliminado del repositorio para unificar la base de código en un solo archivo principal robusto. Toda nueva modificación o uso debe hacerse directamente sobre `pedidos.html`.

---

## ⚡ Características Principales

1.  **Diseño Dashboard Premium (Glassmorphism):**
    *   Una interfaz limpia con bordes suaves, desenfoque de fondo y sombras profundas.
    *   Paleta de colores estrictamente corporativa: **Azul, Rojo y Blanco** en fondos y acentos.
    *   Navegación secuencial por pasos intuitivos con un indicador de progreso superior.
2.  **Asistente de Voz Natural Libre:**
    *   Escucha y analiza oraciones complejas en lenguaje natural para añadir múltiples productos al pedido automáticamente.
    *   Detecta números (ej. *un, dos, doce, 5, 10*), unidades (ej. *bultos, docenas, unidades*) y nombres/colores de productos de forma flexible.
3.  **Integración con Supabase:**
    *   Persistencia de vendedores y clientes.
    *   Almacenamiento de pedidos procesados directamente en la base de datos.
    *   Importación masiva de clientes a partir del listado de un documento PDF local.
4.  **Soporte PWA (Offline & Instalable):**
    *   Funciona sin conexión a Internet mediante almacenamiento en caché local (`sw.js`).
    *   Es instalable como aplicación nativa en dispositivos móviles y de escritorio.

---

## 🛠️ Configuración de Supabase

El sistema requiere las siguientes credenciales para conectarse a Supabase (puedes configurarlas localmente en `supabase-config.js` o ingresarlas en la interfaz web de la aplicación):

*   **Supabase Project URL:** `https://TU-PROYECTO-REF.supabase.co`
*   **Supabase Publishable Key (Anon Key):** `TU-ANON-PUBLISHABLE-KEY`

> [!NOTE]
> La clave de administración (`Service Role Key`) ya no es necesaria en el frontend. La autenticación y escritura se gestionan de manera segura a través del inicio de sesión con **GitHub OAuth**.

### Script SQL para Inicializar la Base de Datos

Para crear las tablas requeridas y configurar las políticas de seguridad (RLS), ejecute el siguiente script en el editor SQL de su consola de Supabase:

```sql
-- Creación de la Tabla de Vendedores
CREATE TABLE vendedores (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  nombre text NOT NULL UNIQUE,
  created_at timestamp with time zone DEFAULT timezone('utc'::text, now()) NOT NULL
);

-- Creación de la Tabla de Clientes
CREATE TABLE clientes (
  codigo text PRIMARY KEY,
  nombre text NOT NULL,
  rif text,
  zona text NOT NULL,
  telefono text,
  created_at timestamp with time zone DEFAULT timezone('utc'::text, now()) NOT NULL
);

-- Creación de la Tabla de Pedidos
CREATE TABLE pedidos (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  numero_pedido text NOT NULL UNIQUE,
  vendedor text NOT NULL,
  cliente_nombre text NOT NULL,
  cliente_codigo text,
  cliente_telefono text,
  metodo_pago text NOT NULL,
  observaciones text,
  productos jsonb NOT NULL,
  created_at timestamp with time zone DEFAULT timezone('utc'::text, now()) NOT NULL
);

-- Habilitar Seguridad a Nivel de Fila (RLS)
ALTER TABLE vendedores ENABLE ROW LEVEL SECURITY;
ALTER TABLE clientes ENABLE ROW LEVEL SECURITY;
ALTER TABLE pedidos ENABLE ROW LEVEL SECURITY;

-- Políticas de Acceso Público
CREATE POLICY "Allow public select" ON vendedores FOR SELECT USING (true);
CREATE POLICY "Allow public insert" ON vendedores FOR INSERT WITH CHECK (true);
CREATE POLICY "Allow public select" ON clientes FOR SELECT USING (true);
CREATE POLICY "Allow public insert" ON clientes FOR INSERT WITH CHECK (true);
CREATE POLICY "Allow public select" ON pedidos FOR SELECT USING (true);
CREATE POLICY "Allow public insert" ON pedidos FOR INSERT WITH CHECK (true);
```

---

## 🎙️ Instrucciones del Asistente de Voz

El asistente de voz inteligente permite agilizar la toma de pedidos mediante habla natural. Active el micrófono flotante y hable con libertad.

### Ejemplos de uso admitidos:
*   **Añadir múltiples productos a la vez:** 
    *   *"Quiero pedir tres docenas de fucsia tela y cinco unidades de tempera"*
    *   *"Añade dos bultos de tempera osi, también tres de naranja tela y un pote de frambuesa"*
*   **Comandos de sistema globales:**
    *   *"ver carrito"* / *"ir a confirmación"* — Te lleva directamente a la pantalla de resumen del pedido.
    *   *"vaciar pedido"* / *"limpiar pedido"* — Elimina todos los elementos del carrito actual.
    *   *"enviar pedido"* — Prepara y envía el mensaje de pedido vía WhatsApp.
    *   *"buscar azul"* — Filtra el catálogo para mostrar únicamente productos con el nombre "azul".

---

## 📱 Instalación PWA (Fuera de Línea)

Para instalar la aplicación en su dispositivo:
1.  **En Móviles (Android/iOS):** Abra la página web en su navegador. En la cabecera verá un botón verde de **Instalar App** o puede usar el menú del navegador seleccionando *"Añadir a la pantalla de inicio"*.
2.  **En Escritorio (Chrome/Edge):** En la barra de navegación del navegador, haga clic en el icono de instalación (pantalla con flecha) o pulse el botón de **Instalar App** en la cabecera.
