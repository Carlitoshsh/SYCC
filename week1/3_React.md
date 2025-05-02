### Creando un Proyecto React con Next.js y Agregando Páginas y un Header

Esta guía te mostrará cómo crear un nuevo proyecto React utilizando Next.js, agregar dos páginas (`contacto` y `lista`), explicar la necesidad de `page.tsx`, y finalmente integrar enlaces a estas páginas en un componente de encabezado (`header.tsx`).

**Pasos:**

1.  **Crea un nuevo proyecto usando el siguiente comando:**
    ```bash
    npx create-next-app@latest
    ```

2.  **Confirma y configura el proyecto:**
    * Responde con `y` para confirmar la creación del proyecto.
    * Ingresa el nombre que desees para tu proyecto y presiona Enter.
    * Presiona Enter para aceptar las configuraciones por defecto para las demás opciones (TypeScript, ESLint, Tailwind CSS, `src` directory, App Router, etc.).
        ![image](https://github.com/user-attachments/assets/f40dba30-1687-4577-b5df-b54ce2a1812e)

3.  **Abre la carpeta del proyecto en Visual Studio Code** y realiza las siguientes modificaciones:
    * **Elimina el contenido de la carpeta `public`:** Esta carpeta contiene archivos SVG que no utilizaremos en este ejemplo. Puedes eliminar todos los archivos dentro de ella.
    * **Reemplaza el contenido del archivo `src/app/page.tsx` con lo siguiente:** Este archivo representa la página de inicio de tu aplicación.
        ```javascript
        export default function Home() {
          return (
            <>
              <div>Mi primera página</div>
            </>
          );
        }
        ```

4.  **Reemplaza el contenido del archivo `src/app/globals.css` con:** Este archivo contiene estilos globales para tu aplicación, incluyendo la importación de Tailwind CSS.
    ```css
    @import "tailwindcss";

    :root {
      --background: #ffffff;
      --foreground: #171717;
    }

    @theme inline {
      --color-background: var(--background);
      --color-foreground: var(--foreground);
      --font-sans: var(--font-geist-sans);
      --font-mono: var(--font-geist-mono);
    }

    body {
      background: var(--background);
      color: var(--foreground);
      font-family: var(--font-sans), Arial, Helvetica, sans-serif;
    }
    ```

5.  **Ejecuta la aplicación:**
    Puedes correr el siguiente comando en tu terminal para iniciar el servidor de desarrollo y ver los cambios en `http://localhost:3000/`.
    ```bash
    npm run dev
    ```

6.  **Crea la carpeta `components` y el archivo `header.tsx`:**
    Dentro de la carpeta `src`, crea una nueva carpeta llamada `components`. Dentro de esta carpeta, crea un nuevo archivo llamado `header.tsx`.

7.  **Crea las páginas `contacto` y `lista`:**
    Dentro de la carpeta `src/app`, crea dos nuevas carpetas llamadas `contacto` y `lista`. Dentro de cada una de estas carpetas, crea un archivo llamado `page.tsx`.

    **Contenido de `src/app/contacto/page.tsx`:**
    ```javascript
    export default function Contacto() {
      return (
        <>
          <div>Página de Contacto</div>
        </>
      );
    }
    ```

    **Contenido de `src/app/lista/page.tsx`:**
    ```javascript
    export default function Lista() {
      return (
        <>
          <div>Página de Lista</div>
        </>
      );
    }
    ```

    **¿Por qué se necesita `page.tsx`?**

    En el **App Router** de Next.js (la estructura predeterminada en las versiones recientes), el archivo `page.tsx` (o `page.jsx` si no estás usando TypeScript) dentro de una carpeta en el directorio `app` **define la ruta que se renderizará cuando se acceda a esa carpeta en el navegador**.

    * Cuando creas la carpeta `src/app/contacto/` y dentro colocas `page.tsx`, Next.js automáticamente interpreta que la URL `/contacto` debe renderizar el componente React exportado por defecto desde ese archivo.
    * De manera similar, `src/app/lista/page.tsx` define el contenido para la ruta `/lista`.
    * El archivo `src/app/page.tsx` es especial porque define la ruta raíz de tu aplicación (`/`).

    Esta convención basada en el sistema de archivos (File System Routing) simplifica la creación de rutas en tu aplicación React. No necesitas configurar archivos de enrutamiento explícitos para las páginas básicas.

8.  **Integra el componente `Header` en el layout y crea los enlaces:**

    Primero, agrega el siguiente contenido al archivo `src/components/header.tsx`:

    ```typescript jsx
    import Link from 'next/link';

    export default function Header() {
      return (
        <header>
          <nav>
            <ul className='bg-teal-800 flex flex-row gap-2 text-white'>
              <li>
                <Link href="/">Inicio</Link>
              </li>
              <li>
                <Link href="/contacto">Contacto</Link>
              </li>
              <li>
                <Link href="/lista">Lista</Link>
              </li>
            </ul>
          </nav>
        </header>
      );
    }
    ```

    **Explicación del `header.tsx`:**

    * Importamos `Link` desde `next/link` para habilitar la navegación del lado del cliente sin recargas de página.
    * El componente `Header` contiene una barra de navegación (`<nav>`) con una lista de enlaces (`<ul>`).
    * Se han agregado clases de Tailwind CSS (`bg-teal-800 flex flex-row gap-2 text-white`) para darle un estilo básico al encabezado.
    * Cada elemento de la lista (`<li>`) contiene un componente `<Link>`. La propiedad `href` de `<Link>` define la ruta a la que se enlazará.

    Ahora, vamos a integrar este componente en el layout de tu aplicación. Abre el archivo `src/app/layout.tsx` (o `layout.jsx`).

    1.  **Importa el componente `Header`:**
        Asegúrate de tener la siguiente línea de importación en la parte superior del archivo, junto con otros imports:

        ```typescript jsx
        // ... otros imports
        import Header from '@/components/header';
        ```

    2.  **Renderiza el componente `Header`:**
        Busca la etiqueta `{children}` dentro del cuerpo (`<body>`) del layout. Inserta el componente `<Header />` justo encima de `{children}`. Tu `return` en `layout.tsx` debería verse similar a esto:

        ```typescript jsx
        <html lang="es">
          <body className={inter.className}>
            <Header />
            {children}
          </body>
        </html>
        ```

    Con estos cambios, el componente `Header` con los enlaces a Inicio, Contacto y Lista aparecerá en la parte superior de todas las páginas de tu aplicación. Al hacer clic en los enlaces, navegarás entre las páginas sin una recarga completa del navegador.

![image](https://github.com/user-attachments/assets/fddb1401-12f3-4178-81cf-0e515c9ebb0d)

Ahora, al ejecutar tu aplicación (`npm run dev`) y visitar `http://localhost:3000/`, verás el texto "Mi primera página" y un encabezado con enlaces a "Inicio", "Contacto", y "Lista". Al hacer clic en estos enlaces, navegarás a las páginas correspondientes que muestran "Página de Contacto" y "Página de Lista" respectivamente.
