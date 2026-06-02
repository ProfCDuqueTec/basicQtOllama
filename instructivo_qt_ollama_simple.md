# Instructivo para compilar y ejecutar `Qt_example2` con Ollama

**Curso:** TC1030 — Programación Orientada a Objetos

## Propósito

Este instructivo guía al alumno para preparar el entorno, verificar Ollama, abrir el proyecto en Visual Studio Code, compilar la aplicación Qt y validar su funcionamiento.

## Paso 0 — Herramientas que deben estar instaladas

Cada alumno debe tener instalado lo siguiente:

### 1. Qt

Debe tener instalado Qt con soporte para MinGW.

Ejemplo esperado:

```text
C:\Qt\6.11.1\mingw_64\
```

o una ruta similar.

Dentro de Qt debe existir `qmake.exe`, por ejemplo:

```text
C:\Qt\6.11.1\mingw_64\bin\qmake.exe
```

### 2. MinGW incluido con Qt

Debe existir `mingw32-make.exe`.

Normalmente está en una ruta parecida a:

```text
C:\Qt\Tools\mingw1310_64\bin\mingw32-make.exe
```

La carpeta puede cambiar según la versión instalada.

### 3. Visual Studio Code

Se usará como editor y terminal de trabajo.

### 4. Ollama

Debe estar instalado en Windows.

Para verificarlo, abre una terminal y ejecuta:

```text
ollama --version
```

Si responde con una versión, Ollama está instalado correctamente.

### 5. Un modelo descargado en Ollama

Por ejemplo:

```text
ollama pull llama3.2
```

Este paso descarga el modelo que usará la aplicación Qt.

## Paso 1 — Abrir Ollama

Hay dos formas.

### Forma recomendada

Abrir Ollama desde Windows:

```text
Inicio -> Buscar "Ollama" -> Abrir
```

Ollama quedará corriendo en segundo plano.

### Forma alternativa desde terminal

En una terminal puedes ejecutar:

```text
ollama run llama3.2
```

Esto abre el modo interactivo de Ollama.

Verás algo como:

```text
>>>
```

Ese modo sirve para escribir prompts, por ejemplo:

```text
>>> Explicame herencia en C++
```

Pero recuerda: ahí no se compila el proyecto Qt.

## Paso 2 — Verificar que la API de Ollama está activa

Abre una terminal normal de Windows o de VSCode.

Ejecuta:

```text
curl http://127.0.0.1:11434
```

Resultado esperado:

```text
Ollama is running
```

Si ves ese mensaje, la aplicación Qt podrá conectarse a Ollama.

## Paso 3 — Probar Ollama con un prompt desde terminal

Ejecuta:

```text
curl http://127.0.0.1:11434/api/generate -d "{\"model\":\"llama3.2\",\"prompt\":\"Explica polimorfismo en C++ en 3 lineas\",\"stream\":false}"
```

Resultado esperado:

```text
{
  "model": "llama3.2",
  "response": "...",
  "done": true
}
```

No importa que la respuesta sea larga. Lo importante es que aparezca el campo:

```text
"response"
```

## Paso 4 — Abrir el proyecto en VSCode

Abre VSCode.

Luego selecciona:

```text
File -> Open Folder
```

Abre la carpeta del proyecto:

```text
Qt_example2
```

No abras solo un archivo. Debes abrir la carpeta completa.

## Paso 5 — Verificar la estructura del proyecto

En VSCode debes ver algo parecido a:

```text
Qt_example2/
|-- Qt_example2.pro
|-- include/
|   `-- MainWindow.h
`-- src/
    |-- main.cpp
    `-- MainWindow.cpp
```

El archivo importante para compilar con qmake es:

```text
Qt_example2.pro
```

## Paso 6 — Revisar el archivo `.pro`

Abre:

```text
Qt_example2.pro
```

Debe contener algo como esto:

```text
TEMPLATE = app
TARGET = Qt_example2

QT += core gui widgets network
CONFIG += c++17 console

INCLUDEPATH += $$PWD/include

SOURCES += \
    src/main.cpp \
    src/MainWindow.cpp

HEADERS += \
    include/MainWindow.h
```

La parte más importante es:

```text
QT += core gui widgets network
```

Si falta `network`, el proyecto no podrá compilar porque usa clases de red de Qt.

## Paso 7 — Abrir una terminal normal en VSCode

En VSCode abre una nueva terminal:

```text
Terminal -> New Terminal
```

Debe aparecer algo como:

```text
PS C:\Users\...\Qt_example2>
```

o:

```text
C:\Users\...\Qt_example2>
```

Si ves esto:

```text
>>>
```

entonces estás dentro de Ollama. Esa no es la terminal correcta para compilar.

Para salir del chat de Ollama escribe:

```text
/bye
```

o presiona:

```text
Ctrl + C
```

## Paso 8 — Ubicarse en la carpeta del proyecto

En la terminal de VSCode, verifica que estás en la carpeta donde está el archivo `.pro`.

Ejecuta:

```text
dir
```

Debes ver:

```text
Qt_example2.pro
include
src
```

Si no estás en la carpeta correcta, usa `cd`.

Ejemplo:

```text
cd "C:\Users\TuUsuario\Documents\Qt_example2"
```

## Paso 9 — Cómo compilar y ejecutar

### Opción A — Si `qmake` y `mingw32-make` ya están en el PATH

Ejecuta:

```text
qmake Qt_example2.pro -spec win32-g++ -o Makefile
```

Luego:

```text
mingw32-make release
```

Finalmente ejecuta:

```text
.\release\Qt_example2.exe
```

### Opción B — Si `qmake` no se reconoce

Usa la ruta completa a `qmake.exe`.

Ejemplo:

```text
& "C:\Qt\6.11.1\mingw_64\bin\qmake.exe" Qt_example2.pro -spec win32-g++ -o Makefile
```

Después ejecuta `mingw32-make`.

Si `mingw32-make` sí está disponible:

```text
mingw32-make release
```

Si no está disponible, usa la ruta completa.

Ejemplo:

```text
& "C:\Qt\Tools\mingw1310_64\bin\mingw32-make.exe" release
```

Finalmente:

```text
.\release\Qt_example2.exe
```

### Opción C — Agregar Qt y MinGW al PATH temporalmente

Esto solo afecta la terminal actual.

Ejemplo:

```text
$env:Path += ";C:\Qt\6.11.1\mingw_64\bin"
$env:Path += ";C:\Qt\Tools\mingw1310_64\bin"
```

Después ya puedes ejecutar:

```text
qmake Qt_example2.pro -spec win32-g++ -o Makefile
mingw32-make release
.\release\Qt_example2.exe
```

Si tu carpeta de MinGW tiene otro nombre, ajusta la ruta.

## Paso 10 — Pruebas y validación

### Prueba 1 — La app abre correctamente

Ejecuta:

```text
.\release\Qt_example2.exe
```

Resultado esperado:
- Se abre una ventana de Qt.
- Aparece un campo para escribir el modelo.
- Aparece un campo para escribir el prompt.
- Aparece el botón *Enviar a Ollama*.
- Aparece un área de conversación.

### Prueba 2 — Prompt vacío

Presiona el botón sin escribir nada.

Resultado esperado:
- El campo de prompt está vacío.

### Prueba 3 — Ollama encendido

Escribe como modelo:

```text
llama3.2
```

Escribe como prompt:

```text
Explicame polimorfismo en C++ con una analogia sencilla.
```

Presiona:

```text
Enviar a Ollama
```

Resultado esperado:

```text
Tu:
Explicame polimorfismo en C++ con una analogia sencilla.

IA:
[respuesta generada por Ollama]
```

### Prueba 4 — Ollama apagado

Cierra Ollama desde el ícono de la barra de Windows:

```text
Clic derecho sobre Ollama -> Quit Ollama
```

Luego intenta enviar un prompt desde la app.

Resultado esperado:

```text
Error:
No se pudo obtener respuesta de Ollama.
```

Este error es correcto. Significa que la app sí intentó conectarse, pero Ollama estaba apagado.

### Prueba 5 — Modelo inexistente

En el campo de modelo escribe:

```text
modelo_inventado
```

Envía cualquier prompt.

Resultado esperado:

```text
Error Ollama:
model 'modelo_inventado' not found
```

o un mensaje equivalente.

## Paso 11 — Errores frecuentes y solución

### Error 1 — Estoy viendo `>>>`

Eso significa que estás dentro del chat de Ollama.

No escribas ahí:

```text
qmake
mingw32-make
```

Solución:

```text
/bye
```

o abre otra terminal en VSCode.

### Error 2 — `qmake` no se reconoce

Mensaje típico:

```text
qmake is not recognized...
```

Solución: usar la ruta completa:

```text
& "C:\Qt\6.11.1\mingw_64\bin\qmake.exe" Qt_example2.pro -spec win32-g++ -o Makefile
```

### Error 3 — `mingw32-make` no se reconoce

Mensaje típico:

```text
mingw32-make is not recognized...
```

Solución: buscar `mingw32-make.exe`.

En PowerShell:

```text
Get-ChildItem C:\Qt -Recurse -Filter mingw32-make.exe
```

Luego usar la ruta encontrada:

```text
& "RUTA_ENCONTRADA\mingw32-make.exe" release
```

### Error 4 — No aparece el ejecutable

Después de compilar, verifica:

```text
dir release
```

Debes ver:

```text
Qt_example2.exe
```

Si no aparece, la compilación falló. Revisa los mensajes anteriores en la terminal.

### Error 5 — La app abre, pero no responde Ollama

Verifica que Ollama esté activo:

```text
curl http://127.0.0.1:11434
```

Debe responder:

```text
Ollama is running
```

Si no responde, abre Ollama desde Windows o ejecuta:

```text
ollama run llama3.2
```
