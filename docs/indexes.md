## INDICES

Un indice almacena una estructura ordenada basada en uno o mas campos de una coleccion. MongoDB puede utilizarla para localizar documentos sin recorrer toda la coleccion, lo que mejora ciertas consultas y ordenamientos. Como contrapartida, los indices ocupan espacio y deben actualizarse al insertar, modificar o eliminar documentos.

MongoDB crea automaticamente un indice unico para el campo `_id` de cada coleccion.

| metodo | descripcion |
|--|--|
| __.createIndex(`{campo: orden}`, `{opciones}`)__ | crea un indice en uno o mas campos |
| __.getIndexes()__ | muestra los indices de una coleccion |
| __.dropIndex(`nombre`)__ | elimina un indice por su nombre |
| __.dropIndexes()__ | elimina todos los indices excepto el indice de `_id` |

En un indice ascendente se utiliza `1`; para uno descendente, `-1`.

```js
// indice simple ascendente
db.products.createIndex({ name: 1 });

// indice compuesto: el orden de los campos importa
db.products.createIndex({ category: 1, price: -1 });

// consultar los indices existentes
db.products.getIndexes();

// eliminar el indice name_1
db.products.dropIndex("name_1");
```

### TIPOS Y OPCIONES

| tipo | ejemplo | uso |
|--|--|--|
| Unico | `{ email: 1 }`, `{ unique: true }` | evita que varios documentos tengan el mismo valor |
| Compuesto | `{ category: 1, price: -1 }` | permite indexar varios campos; el orden afecta las consultas que puede atender |
| Multiclave | `{ tags: 1 }` | indexa automaticamente los valores de un campo que contiene un array |
| Texto | `{ name: "text", description: "text" }` | permite realizar busquedas de texto con `$text` |
| Geoespacial | `{ "address.geolocation": "2dsphere" }` | permite consultas sobre datos geoespaciales GeoJSON |

```js
// impedir valores repetidos en sku
db.products.createIndex({ sku: 1 }, { unique: true });

// buscar documentos que contengan los terminos indicados en campos de texto
db.products.createIndex({ name: "text", description: "text" });
db.products.find({ $text: { $search: "teclado mecanico" } });

// indice geoespacial usado por las consultas de ubicacion
db.branches.createIndex({ "address.geolocation": "2dsphere" });
```

Una coleccion puede tener un solo indice de texto, aunque ese indice puede abarcar varios campos. Para datos geoespaciales, el campo indexado debe contener valores GeoJSON compatibles. Puedes ver ejemplos de consultas en [geospatial.md](../scripts/geospatial.md).

[volver](../readme.md)