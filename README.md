# Notas de la clase

## Action View
El action view es la propuesta de rails para reducir código HTML y es un framework para el uso y manejo de templates que se pueden reutilizar,
action view al ser un framework puede ser utilizado en otros proyectos de ruby y no necesariamente debe ser usado con rails, entonces como resumen
el trabajo de action view es permitir insertar código de ruby dentro de nuestras vistas HTML, el action view construye entonces un html que esta conformado por **Layouts** que dentro tienen **Templates** que a su vez poseen dentro de si **Partials**, esos 3 elementos son por lo general lo 
que conformaran nuestro html construido con el action view.

## Conceptos Básicos
Por defecto el **layout** que se envía como respuestas a las peticiones es el **application.html.erb**, el trabajo del layout es agrupar la parte
de la vista que se comparte entre distintos templates, como por ejemplo la navegación, el footer o elementos que aparezcan en todas las vistas,
los layouts posee helpers que permiten insertar código css o javascript y usar **Yield** para incrustar los templates.

Los **templates** son archivos ubicados en la carpeta views que posee una vista que por lo general son seleccionados específicamente desde un controlador, y muchas veces se seleccionan por convención y no es necesario especificarlo, cuando no se especifica se busca dentro de la carpeta vies un sub carpeta
con el nombre del controlador y un archivo index dentro de la carpeta mencionada, los templates embeben código ruby por defecto usando erb pero este motor
de templates puede ser remplazado por otro si se lo desea.

Los **partials** también llamados template partials son archivos que nos permiten separar nuestro código en porciones mas pequeñas para poder luego formar un template mas complejo y poder reutilizar código entre templates. 

El sistema de templates en este caso **ERB** procesa el código que escribimos en los layouts, templates y partials con código ruby embedido y lo convierte en código HTML 

## Layouts

## Assset Pipeline

## Usando CSS

## Imagenes

## Parciales y metodo render

## Configuracion de layouts y manifiestos
