# Task Manager API — Prueba Técnica

API REST de gestión de tareas construida con **FastAPI + SQLAlchemy + SQLite**.

## Requisitos

- Python 3.11+

## Instalación

```bash
pip install -r requirements.txt
```

## Ejecutar la API

```bash
uvicorn app.main:app --reload
```

Documentación interactiva disponible en: http://localhost:8000/docs

## Endpoints

| Método | Ruta        | Descripción                        |
| ------ | ----------- | ---------------------------------- |
| POST   | /tasks/     | Crear tarea                        |
| GET    | /tasks/     | Listar tareas (filtro: `?status=`) |
| GET    | /tasks/{id} | Obtener tarea por ID               |
| PATCH  | /tasks/{id} | Actualizar tarea parcialmente      |
| DELETE | /tasks/{id} | Eliminar tarea                     |

**Valores válidos para `status`:** `pending`, `in_progress`, `done`  
**Valores válidos para `priority`:** `1` (baja) a `5` (alta)

## Tu misión

Esta API tiene **3 comportamientos incorrectos**. Tu tarea es:

1. Identificar y corregir los 3 problemas.
2. Verificar tus correcciones ejecutando los tests:

```bash
python -m pytest tests/ -v
```

Los 3 tests deben pasar para completar el desafío.

### Pistas

- Hay **2 bugs en la capa de repositorio** (`app/repositories/`).
- Hay **1 bug en la capa de rutas** (`app/routers/`).
- La API arranca sin errores y los endpoints responden — los problemas son de comportamiento, no de sintaxis.
- Usa `/docs` para explorar y probar los endpoints manualmente.

**No se permite el uso de inteligencia artificial.**

## Arquitectura

```
app/
├── main.py            # Entry point
├── database.py        # Configuración SQLAlchemy
├── dependencies.py    # Inyección de dependencias
├── models/            # Modelos ORM
├── schemas/           # Esquemas Pydantic
├── repositories/      # Acceso a datos
├── services/          # Lógica de negocio
└── routers/           # Endpoints HTTP
```

## Solución

- Se agregó el codigo de respuesta HTTP 201 al endpoint de crear tareas. Dado que no se estaba especificando un código de respuesta por defecto y no se respetaba el estado HTTP correcto (201 recurso creado). (POST /tasks/).

- Se cambió la lógica del filtro por estatus en el repositorio de tareas. Ahora se filtra por estado == status y se ordenan por prioridad. Antes se usaba el operador != que devolvía todas las tareas excepto las venian de la query (status), al cambiar el operador por == se aplica el filtro deseado. (GET /tasks/).

- Se corrigió el orden al actualizar una tarea. Antes se ejecutaba db.refresh(task) y luego db.commit(). Ahora se ejecuta primero el commit y luego el refresh para que se reflejen correctamente los cambios. De este modo primero se guardan los cambios y luego se recuperan los datos actualizados. (PATCH /tasks/{id})
