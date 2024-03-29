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
En ruby el método **yield** se utiliza para ejecutar el código que es recibido en un bloque cuando este código es pasado como parámetro a una función, 
con base en este conocimiento podemos intuir que en los layouts se renderizaran los templates que nosotros seleccionemos desde el controlador, en donde se encuentre la palabra reservada **yield**, podemos hacer que este renderizado suceda en cualquier parte de nuestro layout.


``` [erb]

  <body>
    <section class="layouts">
      <header>
        <nav>
          <%=link_to "Inicio", root_path%>
          <%=link_to "Artículos", posts_path%>
        </nav>
      </header>
      <main>
        <%= yield %>
      </main>
      <footer>
        <p> Mi Blog &copy <%= Time.now.year %> </p>
      </footer>
    </section>
  </body>

```

ademas de la yield en los layout podemos encontrar etiquetas como **csrf_meta_tags**, **csp_meta_tag**, **stylesheet_link_tag** (Sirve para insertar hojas
de estilo en el layout), **javascript_importmap_tags** (importa JS), cada una de las cuales posee su propia funcionalidad.

## Assset Pipeline
El asset papeline es el nombre que se le da al proceso de como rails maneja los assets, esta conformado por varias gemas que permiten que se encargue 
de las tareas necesarias para manejar tus assets, por defecto el asset pipeline se encarga de:

Concatenar y minificar archivos CSS y JS, en este punto el asset pipeline usa archivos de manifiesto para mapear y definir que archivos deberán ser
minificados, por defecto rails trae ya 2 manifiestos, uno para css y otro para js y se encuentran en la carpeta app/assets, ademas de concatenar
el asset pipeline modifica los archivos sacrificando su legibilidad para optimizar su rendimiento, esto lo logra quitando saltos de lineas, comentarios,
cambiando nombres de variables, etc y por ultimo el asset pipeline firma el archivo con un código único (application-Aasdnsadkhkdh13ksa.js) para que el navegador pueda identificar si debe o no guardar el archivo en cache.

Habilitar el uso de pre - procesadores como SASS

Integración con bundler permite que assest que vengan de otras gemas puedan ser integrados con tu proyecto (Bootstrap, TailwindCss)

## Usando CSS
Para poder añadir bootstrap a nuestro proyecto deberemos agregarlo al gemfile con el comando: 
` bundle add bootstrap `
luego de instalar bootstrap agregaremos un archivo en la ruta de los assests css (app/assets/stylesheets) con el nombre app.scss y dentro de este archivo
importaremos bootstrap.
  
``` [sass]

  @import "bootstrap";
  
```
en el manifiesto como recomendación no se aconseja importar todos los archivos que se encuentran en la carpeta stylesheets, para poder importar y poder
tener un mejor manejo y control de los archivos puedes sustituir la importación completa de todos los archivos por importaciones individuales:

``` [css]

  *= require_tree . (Borrar)
  *= require app.scss 
  *= requiere containers.scss
  
```

## Imágenes
Existen diferentes helper para poder usar imágenes en los templates, **image_tag** **asset_path** entre otros y es la forma correcta de utilizar tus assets en tus views de rails, estos helpers insertan el codigo completo en tu template y se encargan de buscarla en las ubicaciones por defecto que son app/assets vendor o lib/assets, en caso de que decidas crear carpetas dentro de los paths de búsquedas puedes utilizar los helper de la siguiente manera:

` <%= image_tag "caperta/imagen.png" %> `

` <%= asset_path "caperta/imagen.png" %> `

si quisieras usar archivos que no pasen por estos helper puedes guardarlos en tu carpeta public, pero no es recomendado hacer esto, estos helpers antes mencinados no solo pueden ser utilizadon en las visatas html si no que pueden ser usados tambien en los archivos css y js, por ejemplo:

``` [css]

  .cover {
    background-image: url(<%= asset_path "caperta/imagen.png" %>);
  }
  
```

## Parciales y método render
Los parciales son fragmentos de código reutilizables dentro de las templates, para usarlos se debe recurrir a un método render, este método nos permite incrustar un parcial dentro de un template, el path de búsqueda predeterminado es **/partials** y tienen como particularidad que el nombre del archivo siempre deberá iniciar con un (guion bajo) **_nombre.html.erb** 

render puede ser usado con diferentes opciones para incrustar información adicional que puede ser utilizada dentro del parcial, también existe una opción collection que es recibida por el método render y que permite crear un parcial por cada uno de los objectos de la colección.


``` [erb]

  <div>
    <%= render partials:"posts/card", collection: @posts, as: "post" %>
  </div>

```

## Configuración de layouts y manifiestos
existe la posibilidad de cambiar el layout para diferentes controladores de tu aplicación, esto con la finalidad de tener diferentes vistas para diferentes acciones, se puede realizar el cambio de layout desde cada accione del controlador con el comando render o se puede configurar todo el layout para un solo controlador con la palabra layout al inicio del mismo, ademas si no le pasas un string a esa configuración puedes definir un método de la siguiente manera que te permitirá agregar una lógica mas compleja para decidir que layout vas a mostrar:

``` [ruby]

  class homeController < ApplicationController
    layout set_layout
    def index
      @posts = Post.all
    end
  end

  def set_layout
    if params[:action] == "index"
      "home"
    else
      "application"
    end
  end

```

## active storage y action text




