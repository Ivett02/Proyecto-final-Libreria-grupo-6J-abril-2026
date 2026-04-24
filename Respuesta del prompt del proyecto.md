Para desarrollar el agente global **.agents** y estructurar tu proyecto de librería con Flutter y Firebase, seguiremos una secuencia lógica de ingeniería de software. Este agente actuará como el orquestador de tus sub-skills (diseño, código y scraping).

---

## 1. Estructura del Agente Global `.agents`

Primero, definimos la identidad del agente y su hoja de ruta.

### Archivo: `SKILL.md`
```markdown
# Agente Global: .agents - Sistema de Automatización de Librería

## Perfil
Este agente coordina tres sub-habilidades para el desarrollo de la App Librería:
1. **Skill de Diseño**: Generación de UI/UX y temas en Flutter.
2. **Skill de Código**: Lógica de negocio, BLoC/Provider y modelos Dart.
3. **Skill de Scraping**: Extracción de metadatos de libros (ISBN, portadas).

## Carpetas del Agente
- /scripts: Automatización de deploys y limpieza de caché.
- /ejemplos: Templates de botones y tarjetas de libros.
- /resources: Assets, iconos y archivos JSON de prueba.
```

---

## 2. Preparación del Entorno (Prerrequisitos)

Antes de codificar, debemos asegurar que el motor está listo. Ejecuta estos comandos en tu terminal de **VS Code** o **Antigravity**:

### Verificación de Herramientas
```bash
flutter --version
dart --version
```

### Instalación de Firebase CLI
Si no tienes las herramientas de Firebase, instálalas globalmente:
1. **Instalar Node.js** (necesario para el CLI).
2. **Instalar CLI**: `npm install -g firebase-tools`
3. **Login**: `firebase login` (Se abrirá el navegador para autenticar).
4. **FlutterFire**: `dart pub global activate flutterfire_cli`

---

## 3. Configuración del Proyecto "proyectolibreria"

Crea el proyecto y accede a la carpeta:
```bash
flutter create proyectolibreria
cd proyectolibreria
```

### Configuración de `pubspec.yaml`
Añade las dependencias necesarias para Firestore y el diseño:

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^3.0.0
  cloud_firestore: ^5.0.0
  cupertino_icons: ^1.0.6

flutter:
  uses-material-design: true
```
*Ejecuta `flutter pub get` después de guardar.*

---

## 4. Arquitectura de Archivos y Código



### A. Modelo de Datos (`lib/models/libro_model.dart`)
```dart
class Libro {
  String id;
  String titulo;
  String autor;
  double precio;

  Libro({required this.id, required this.titulo, required this.autor, required this.precio});

  Map<String, dynamic> toMap() => {
    "titulo": titulo,
    "autor": autor,
    "precio": precio,
  };

  factory Libro.fromFirestore(Map<String, dynamic> data, String id) {
    return Libro(
      id: id,
      titulo: data['titulo'] ?? '',
      autor: data['autor'] ?? '',
      precio: (data['precio'] ?? 0.0).toDouble(),
    );
  }
}
```

### B. Servicio Firestore (`lib/services/firebase_service.dart`)
```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import '../models/libro_model.dart';

class FirebaseService {
  final CollectionReference _db = FirebaseFirestore.instance.collection('libros');

  // CREATE
  Future<void> addLibro(String titulo, String autor, double precio) async {
    await _db.add({'titulo': titulo, 'autor': autor, 'precio': precio});
  }

  // READ (Stream)
  Stream<List<Libro>> getLibros() {
    return _db.snapshots().map((snapshot) =>
        snapshot.docs.map((doc) => Libro.fromFirestore(doc.data() as Map<String, dynamic>, doc.id)).toList());
  }

  // UPDATE
  Future<void> updateLibro(String id, String titulo, String autor, double precio) async {
    await _db.doc(id).update({'titulo': titulo, 'autor': autor, 'precio': precio});
  }

  // DELETE
  Future<void> deleteLibro(String id) async {
    await _db.doc(id).delete();
  }
}
```

---

## 5. UI: Pantallas del CRUD

### C. Pantalla Principal (`lib/screens/home_screen.dart`)
```dart
import 'package:flutter/material.dart';
import '../services/firebase_service.dart';
import '../models/libro_model.dart';

class HomeScreen extends StatelessWidget {
  final FirebaseService _service = FirebaseService();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Librería Global Agent")),
      body: StreamBuilder<List<Libro>>(
        stream: _service.getLibros(),
        builder: (context, snapshot) {
          if (!snapshot.hasData) return Center(child: CircularProgressIndicator());
          var libros = snapshot.data!;
          return ListView.builder(
            itemCount: libros.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(libros[index].titulo),
                subtitle: Text(libros[index].autor),
                trailing: IconButton(
                  icon: Icon(Icons.delete, color: Colors.red),
                  onPressed: () => _service.deleteLibro(libros[index].id),
                ),
                onTap: () => _mostrarFormulario(context, libros[index]),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.add),
        onPressed: () => _mostrarFormulario(context, null),
      ),
    );
  }

  void _mostrarFormulario(BuildContext context, Libro? libro) {
    // Aquí se implementaría el diálogo para Crear/Editar
    // El Agente de Diseño definiría los textfields aquí
  }
}
```

---

## Pruebas de Verificación y Conectividad

1. **Configurar Firebase**: Ejecuta `flutterfire configure`. Selecciona tu proyecto creado en la consola. Esto generará `firebase_options.dart`.
2. **Inicializar**: En tu `main.dart`, asegúrate de llamar a `Firebase.initializeApp()`.
3. **Reglas de Firestore**: En Firebase Console, asegúrate de poner las reglas en **Modo Prueba** para permitir lectura/escritura inicial.

---

### Tareas del Agente Global para el siguiente paso:
* **Script de Scraping**: ¿Deseas que creemos el script en Python o Dart para buscar libros por ISBN y auto-rellenar el CRUD?
* **Diseño**: ¿Aplicamos un tema Dark Mode o estilo Material 3 a la interfaz?
