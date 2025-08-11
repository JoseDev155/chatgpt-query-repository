El problema puede deberse a que el navegador no entiende el tipo de script text/babel de forma nativa. Este tipo de script requiere un transformador como Babel, que convierte el código JSX en JavaScript comprensible por el navegador. Cuando usas React desde un CDN, necesitas incluir Babel también para manejar JSX.

Aquí está cómo puedes solucionarlo:

## Pasos para solucionar el problema:
- Incluye Babel desde un CDN: Agrega el siguiente **script** antes de tu script de React, para que el navegador pueda transformar el código JSX:

```html
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
```

- Cambia el tipo del script a text/babel: Mantén tu script con el tipo text/babel, ya que esto le indica a Babel que debe procesar el código.

## Estructura final de tu archivo index.html:
Asegúrate de que tu archivo tenga esta estructura:

```html
<!DOCTYPE html>
<html lang="en">
	<head>
	    <meta charset="UTF-8">
	    <meta name="viewport" content="width=device-width, initial-scale=1.0">
	    <title>React con CDN</title>
	</head>
	<body>
	    <div id="app"></div>
	    <!-- React y ReactDOM desde CDN -->
	    <script src="https://unpkg.com/react@17/umd/react.development.js"></script>
	    <script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
	    <!-- Babel desde CDN -->
	    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
	    <!-- Tu script con JSX -->
	    <script type="text/babel">
	        const $app = document.getElementById('app');
	        const h = React.createElement;
	
	        const Avatar = params => {
	            const src = `https://randomuser.me/api/portraits/women/${params.id}.jpg`;
	            return <img src={src} />;
	        };
	
	        ReactDOM.render(
	            <div>
	                <Avatar id={10} />
	                <Avatar id={11} />
	            </div>,
	            $app
	        );
	    </script>
	</body>
</html>
```

## Nota sobre Babel en producción:

o	Este enfoque con Babel en el navegador es solo para propósitos de desarrollo o aprendizaje.
o	En producción, debes usar herramientas como Parcel, Webpack, o Vite para compilar tu JSX antes de desplegarlo.

Prueba esto y verifica si ya puedes ver las imágenes renderizadas. Si necesitas más ayuda, ¡aquí estoy! 😊

