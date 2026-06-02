# Qt + Ollama — Proyecto básico de prueba 

Aplicación de ejemplo desarrollada en **C++ con Qt Widgets** que permite escribir un comando o *prompt* desde una interfaz gráfica, enviarlo a **Ollama** mediante una API local y mostrar la respuesta generada por un modelo de IA.

Este repositorio sirve como base para el **proyecto integrador** del curso de Programación Orientada a Objetos. La intención es qproporcionar un ejemplo funcional y que pueda evolucionar progresivamente aplicando conceptos de POO como clases, objetos, apuntadores, encapsulamiento, herencia, polimorfismo, clases abstractas, miembros estáticos, sobrecarga de operadores y manejo de errores.

![Flujo de operación del proyecto](https://github.com/ProfCDuqueTec/basicQtOllama/blob/main/SimpleDescriptionDiagram.png)


---

## 1. Objetivo del proyecto

Construir una aplicación gráfica en Qt que funcione como puente entre el usuario y un modelo local de IA ejecutado con Ollama.

La primera versión del proyecto permite:

- Escribir un *prompt* desde una ventana Qt.
- Seleccionar el nombre del modelo de Ollama.
- Enviar el *prompt* a la API local de Ollama.
- Recibir una respuesta en formato JSON.
- Mostrar la respuesta dentro de la interfaz gráfica.
- Manejar errores básicos como prompt vacío, conexión fallida o respuesta JSON inválida.

---

## 2. Tecnologías utilizadas

- C++17
- Qt 6
- Qt Widgets
- Qt Network
- Qt JSON
- qmake
- MinGW en Windows
- Visual Studio Code
- Ollama

---

## 3. Estructura del proyecto

```text
Qt_example2/
├── Qt_example2.pro
├── include/
│   └── MainWindow.h
└── src/
    ├── main.cpp
    └── MainWindow.cpp
