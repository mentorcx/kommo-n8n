# {Nombre del recurso}

> Plantilla. Copia este archivo para documentar un endpoint nuevo.

## Resumen

Una o dos líneas sobre qué representa este recurso en Kommo.

## Ruta base

```
/api/v4/{recurso}
```

## Métodos disponibles

| Método | Ruta                          | Descripción                  |
|--------|-------------------------------|------------------------------|
| GET    | `/{recurso}`                  | Listar                       |
| GET    | `/{recurso}/{id}`             | Obtener por ID               |
| POST   | `/{recurso}`                  | Crear (uno o varios)         |
| PATCH  | `/{recurso}`                  | Actualizar (lote)            |
| PATCH  | `/{recurso}/{id}`             | Actualizar uno               |
| DELETE | `/{recurso}/{id}`             | Eliminar                     |

## Parámetros de consulta comunes

| Parámetro    | Tipo    | Descripción                                    |
|--------------|---------|------------------------------------------------|
| `with`       | string  | Subrecursos a incluir (lista separada por comas)|
| `page`       | int     | Página                                         |
| `limit`      | int     | Tamaño de página (máx. 250 normalmente)        |
| `query`      | string  | Búsqueda libre                                 |
| `filter[..]` | varios  | Filtros específicos                            |
| `order[..]`  | string  | Orden (`asc` / `desc`)                         |

## Ejemplos

### Listar

```http
GET /api/v4/{recurso}?limit=50&with=contacts
Authorization: Bearer <token>
```

### Crear

```http
POST /api/v4/{recurso}
Authorization: Bearer <token>
Content-Type: application/json

[
  {
    "name": "Ejemplo"
  }
]
```

### Actualizar

```http
PATCH /api/v4/{recurso}/{id}
Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "Nuevo nombre"
}
```

## Estructura de la respuesta

```json
{
  "_page": 1,
  "_links": { },
  "_embedded": {
    "{recurso}": [
      {
        "id": 0,
        "name": "..."
      }
    ]
  }
}
```

## Restricciones específicas

- Limite máximo por lote.
- Campos obligatorios al crear/actualizar.
- Permisos requeridos.

## Errores frecuentes

Documenta aquí los errores típicos de este recurso (400/403/404 con causas reales observadas).

## Notas y truquitos

Comportamientos no documentados o gotchas que descubras en la práctica.

## Referencias

- Documentación oficial: https://developers.kommo.com/
