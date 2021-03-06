# so-project-template
Template para facilitar la creación de proyectos en C con una biblioteca 
compartida.

## Features

Ambos makefiles:

✔️ Compilan solamente los archivos fuente necesarios, incluso detecta cuándo las
bibliotecas hechas por el usuario fueron modificadas para volver a compilar
todos los objetos.

✔️ Permiten el uso de subcarpetas.

✔️ Se pueden importar desde Eclipse, CLion, VSCode o cualquier editor de texto u
 entorno de desarrollo.

✔️ Permiten que el repo se pueda deployar usando el script 
[so-deploy](https://github.com/sisoputnfrba/so-deploy).

✔️ No requieren que se especifique el nombre del proyecto al que pertenecen 
(alcanza con copiar la carpeta `/project/` o `/utils/` y modificar su nombre para 
configurar el nombre del proyecto).

✔️ Incluyen flags de debug para `make all` y flags de release para `make release`.

✔️ ¡No hace falta configurar variables de entorno! La biblioteca compartida es una
static library, que luego es compilada directamente en el binario final del
proyecto. Para saber más info sobre static vs shared libraries, podés
[ver este video](https://www.youtube.com/watch?v=JbHmin2Wtmc).

Además, el makefile del proyecto incluye:

✔️ Ejecución con Valgrind mediante `make memcheck` y `make helgrind`.

## Uso

### Estructura de archivos

Los proyectos funcionan bajo la siguiente estructura, adaptada para que 
[so-deploy](https://github.com/sisoputnfrba/so-deploy) funcione:
```
.
│  
└─── <project_1>/
|     └─── src/
|     └─── makefile
└─── <project_2>/
|     └─── src/
|     └─── makefile
└─── ...
└─── <project_n>/
|     └─── src/
|     └─── makefile
└─── <static_lib>/
      └─── src/
      └─── makefile
```

### ¿Cómo creo mis proyectos?

- Para crear un proyecto estándar podés descargar la carpeta `project/` desde las 
[releases](https://github.com/RaniAgus/so-project-template/releases) (recordá 
cambiar `<project_name>` por el nombre de tu proyecto antes de ejecutar):

```
wget -qO- https://github.com/RaniAgus/so-project-template/releases/latest/download/project.tar.gz\
 | tar -xvzf - --strip-components 1 --one-top-level=<project_name>
```

Luego, deberás [incluir las bibliotecas](#cómo-incluyo-una-biblioteca) que utilices.

- Para crear una static library se deberá hacer el mismo procedimiento, en este
caso mediante la carpeta `utils/` (recordá cambiar `<lib_name>` por el nombre de 
tu biblioteca antes de ejecutar):

```
wget -qO- https://github.com/RaniAgus/so-project-template/releases/latest/download/utils.tar.gz\
 | tar -xvzf - --strip-components 1 --one-top-level=<lib_name>
```
Luego, deberás [incluir esa library](#cómo-incluyo-una-biblioteca) en los proyectos 
que la utilicen.

### ¿Cómo incluyo una biblioteca?

Para incluir una biblioteca alcanza con editar la macro `LIBRARIES` del makefile. 
También, para incluir una biblioteca propia se debe editar tanto `LIBRARIES` como 
`LIBRARY_PATHS`. 

Por ejemplo, para incluir las `commons` y una biblioteca propia llamada `utils`
ubicada en el mismo repo se deberá agregar lo siguiente:

```make
# Include libraries here
LIBRARIES=utils commons pthread

# Include custom library paths here
LIBRARY_PATHS=../utils
```

### ¿Cómo importo los proyectos en el IDE?

- [Cómo importar en Eclipse](../../wiki/Eclipse)

### ¿Cómo ejecuto Valgrind usando el makefile?

Para ejecutar Valgrind utilizamos las reglas `memcheck` o `helgrind`:

```
$ make memcheck

$ make helgrind
```

¡También se pueden pasar parámetros al `main()`!
```
$ make memcheck ARGS="arg1 arg2 arg3"

$ make helgrind ARGS="arg1 arg2 arg3"
```

### ¿Cómo hago para acordarme de todos estos comandos?

¡No hace falta! Podés usar la regla `help`:

```
$ make help

COMMANDS:
    make / make all -- Build project using debug flags.
    make project    -- Build project using release flags.
    make clean      -- Remove generated files from file system.
    make memcheck   -- Run using valgrind memcheck tool. Output will be redirected to an external log file.
    make helgrind   -- Run using valgrind helgrind tool. Output will be redirected to an external log file.
VARIABLES:
    ARGS          -- Arguments to be passed to main() using valgrind tools (eg: 'make helgrind ARGS="arg1 arg2 arg3"').
    LIBRARIES     -- External libraries to be included and linked, separated by spaces (eg: 'pthread commons utils').
    LIBRARY_PATHS -- Relative path to own static libraries root, separated by spaces (eg: '../utils').
    PROJECT       -- Your project name. By default it will be your pwd basename.
```


## Contacto

Si encontrás algun error en los makefiles o tenés alguna sugerencia, ¡no dudes 
en levantar un issue en este repositorio!

