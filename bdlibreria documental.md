Aquí tienes un ejemplo claro y sencillo de cómo diseñar una base de datos **NoSQL (MongoDB)** para una **librería**, incluyendo **colecciones, documentos, atributos y tipos de datos** 👇

---

## 📚 Base de Datos: `libreria`

En MongoDB no hay tablas, sino **colecciones**, y dentro de ellas hay **documentos (JSON)**.

---

# 🧩 1. Colección: `libros`

### 📄 Ejemplo de documento:

```json
{
  "_id": ObjectId("661a1f123abc456789"),
  "titulo": "Cien años de soledad",
  "autor": "Gabriel García Márquez",
  "genero": "Realismo mágico",
  "precio": 250.50,
  "stock": 15,
  "fecha_publicacion": ISODate("1967-05-30"),
  "editorial": "Sudamericana",
  "disponible": true
}
```

### 📌 Atributos y tipos:

* `_id` → ObjectId
* `titulo` → String
* `autor` → String
* `genero` → String
* `precio` → Double
* `stock` → Int
* `fecha_publicacion` → Date
* `editorial` → String
* `disponible` → Boolean

---

# 👤 2. Colección: `clientes`

### 📄 Ejemplo de documento:

```json
{
  "_id": ObjectId("661a2b456def789123"),
  "nombre": "Ailin Ivett",
  "correo": "ailin@email.com",
  "telefono": "6561234567",
  "direccion": {
    "calle": "Av. Tecnológico",
    "ciudad": "Juárez",
    "estado": "Chihuahua"
  },
  "fecha_registro": ISODate("2026-04-27")
}
```

### 📌 Tipos:

* `nombre` → String
* `correo` → String
* `telefono` → String
* `direccion` → Object
* `fecha_registro` → Date

---

# 🧾 3. Colección: `ventas`

### 📄 Ejemplo de documento:

```json
{
  "_id": ObjectId("661a3c789ghi012345"),
  "id_cliente": ObjectId("661a2b456def789123"),
  "libros": [
    {
      "id_libro": ObjectId("661a1f123abc456789"),
      "cantidad": 2,
      "precio_unitario": 250.50
    }
  ],
  "total": 501.00,
  "fecha": ISODate("2026-04-27"),
  "metodo_pago": "Tarjeta"
}
```

### 📌 Tipos:

* `id_cliente` → ObjectId (referencia)
* `libros` → Array de objetos
* `total` → Double
* `fecha` → Date
* `metodo_pago` → String

---

# 🏢 4. Colección: `proveedores`

### 📄 Ejemplo:

```json
{
  "_id": ObjectId("661a4d123jkl678901"),
  "nombre": "Editorial Planeta",
  "contacto": "contacto@planeta.com",
  "telefono": "5551234567",
  "direccion": "CDMX"
}
```

---

# 🎯 Resumen rápido

| Colección   | Descripción               |
| ----------- | ------------------------- |
| libros      | Información de los libros |
| clientes    | Datos de clientes         |
| ventas      | Registro de compras       |
| proveedores | Proveedores de libros     |

---

# 💡 Extra (importante para tu proyecto)

Como estás trabajando con **MongoDB**, recuerda:

* No necesitas relaciones estrictas como en SQL
* Puedes usar **referencias (ObjectId)** o **documentos embebidos**
* Es más flexible para proyectos como tu **librería**

---

Si quieres, en el siguiente paso puedo ayudarte a:
✅ Crear los comandos reales en MongoDB (`insertOne`, `find`, etc.)
✅ Integrarlo con tu proyecto (Node.js, PHP o lo que uses)
✅ O adaptarlo exactamente a tu proyecto de escuela

Solo dime 👍
