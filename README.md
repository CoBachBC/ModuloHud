# ModuloHud
Módulo para mostrar notificaciones emergentes en las aplicaciones MVC C# de CoBachBC

![](https://raw.githubusercontent.com/CoBachBC/ModuloHud/main/demo.gif)

## Implementación

1. Incluye la vista parcial `_ModuloHud.cshtml` como primera hija del contenedor principal de la aplicación (típicamente `<div id="main">`).
1. Incluye el script `moduloHud.js` al final de tu plantilla antes de cerrar la etiqueda `<body>`.
1. Incluye el estilo `moduloHud.css` en el elemento `<head>` de tu plantilla.
1. Borra todas las referencias a `TempData["message"]` que tengas en tus vistas para que se muestren solo una vez.

## Uso

Muestra en una notificación emergente durante 5 segundos el contenido de `TempData["message"]`. Puedes formatear el texto de tu `TempData["message"]` para utilizar los estilos predefinidos:
- **Éxito**: se muestra en color verde. Debes inicar el contenido de tu cadena `TempData["message"]` con **`Rx.`** seguido inmediatamente del texto que deseas que muestre la notificación.
- **Notificación**: se muestra en color azul. Debes inicar el contenido de tu cadena `TempData["message"]` con **`Nx.`** seguido inmediatamente del texto que deseas que muestre la notificación.
- **Advertencia**: se muestra en amarillo. Debes inicar el contenido de tu cadena `TempData["message"]` con **`Wx.`** seguido inmediatamente del texto que deseas que muestre la notificación.
- **Error**: se muestra en color rojo. Debes inicar el contenido de tu cadena `TempData["message"]` con **`Ex.`** seguido inmediatamente del texto que deseas que muestre la notificación.
- **No definido**: se muestra en color blanco. Esta opción se muestra por defecto cuando la evaluación de los 3 primeros caracteres de la cadena no coincide con las opciones predeterminadas, mostrará íntegra la totalidad de la cadena contenida en `TempData["message"]`.

Típicamente el contenido de `TempData["message"]` proviene de la variable `res` en los archivos de `repository`, por lo que es en éstos donde se tiene que realizar el cambio para que funcione adecuadamente, por ejemplo:

Cambiar `res = "OK";` por `res = "Rx.OK;"` producirá un mensaje de tipo `Éxito` con el texto `OK`.

## Funcionamiento

La vista `_ModuloHud.cshtml` atrapa el valor de `TempData["message"]` y lo evalúa. Cuando `TempData["message"]` es diferente a `null` lo evalúa con `switch` en busca de los siguientes caracteres al inicio de la cadena:

- __Rx__ asigna la clase `hudExito`.
- __Ex__ asigna la clase `hudError`.
- __Nx__ asigna la clase `hudNotificacion`.
- __Wx__ asigna la clase `hudAdvertencia`.

Si no evalúa a uno de esas opciones asigna la clase defecto `hudNeutro.`

El script `moduloHud.js` busca la clase `fadeHud` y con jQuery la muestra por 5000 milisegundos para después desvanecerla. El tiempo de duración se puede modificar en este mismo archivo. La duración se aplica a todas las notificaciones.
