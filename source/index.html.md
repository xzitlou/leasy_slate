---
title: API Reference

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
  - python

toc_footers:

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentación para API Leasy V2
---

# Introducción

Bienvenido a la documentación de Leasy API V2" en esta sección encontrarás la forma de leer y escribir la información relacionada con los clientes de Leasy. Podrás encontrar información de Contratos" Facturas" Carros" Perfil" y más.

Las llamadas las podrás hacer desde Shell" Python" y JavaScript. Podrás visualizar los ejemplos de cada llamada en cada sección.

## URL base

La URL base del API es `https://leasy.pe/api/v2/`

Nota: Por temas de seguridad muchas de las llamadas deberán realizarse enviando el token del usuario en los Headers.

# Autenticación

> Get token:

```python
import requests

r = requests.post(
    "https://leasy.pe/api/v2/clients/auth/",
    json={
        "document": "document",
        "password": "password",
    }
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())
```

> Response [200]:

```json
{"token": "token"}
```

> Response [400]:

```json
{"error": "Las credenciales ingresadas son incorrectas"}
```

### HTTP Request

`POST https://leasy.pe/api/v2/clients/auth/`

### Body JSON parameters

Parámetro | Valor  | Descripción
--------- |--------| -----------
document | String | El número de documento del cliente.
password | String | La contraseña del cliente.

# Cliente

## Obtener perfil

```python
import requests

r = requests.get(
    "https://leasy.pe/api/v2/clients/profile/",
    headers={
        "Authentication": "token"
    }
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())

```

> Response 200:

```json
{
	"id": 21,
	"first_name": "pruebas",
	"last_name": "apellido",
	"email": "hola@loualcala.com",
	"document": "10442008642",
	"address": "PRUEBAS",
	"phone_number": "987928878",
	"created_at": "2018-09-27",
	"coins": 116.0,
	"token": "<value>",
	"firebase_token": "<value>",
	"contracts": [
		{
			"car": {
				"id": 27,
				"hash": "<value>",
				"model": "Cobalt",
				"brand": "Chevrolet",
				"soat_file": "/static/files/soat/ACFrOgAoaFUTD4A4AnpmopQwUz2-S85W3hGtyH4TRioVEZd8TgD2Qag6y7PMXgqLyNEzEA_uyH61fJ.pdf",
				"secure_file": "/static/files/secure/ACFrOgAoaFUTD4A4AnpmopQwUz2-S85W3hGtyH4TRioVEZd8TgD2Qag6y7PMXgqLyNEz_DAMViCj.pdf",
				"license_plate": "PRUEBAS",
				"year": "2018",
        "photos": ["url", "url", ...]
			},
			"status": "contract_active",
			"created_at": "2019-01-05",
			"hash": "<value>",
			"weekly_amount": 525,
			"auto_charge": true,
			"expired_at": "2021-07-03",
			"next_payment_date": "2023-07-24",
			"fine": 0,
			"delayed_amount": 29910,
			"total_amount": 0,
			"paid_amount": 2
		}
	],
	"phones": [
		{
			"id": 72,
			"phone_number": "10442008642"
		}
	],
	"uber_plan": {
		"name": "Oro",
		"rate": "5%",
		"description": ""
	}
}
```

> Response 400

```json
{"error": "Perfil de cliente no encontrado"}
```

Podrás obtener toda la información relacionada al cliente.

### HTTP Request

`GET https://leasy.pe/api/v2/clients/profile/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

## Actualizar contraseña

```python
import requests

r = requests.patch(
    "https://leasy.pe/api/v2/clients/password/",
    headers={
        "Authentication": "token"
    },
    json={
        "password": "current password",
        "new_password": "new password",
        "confirm_new_password": "confirm new password",
    }
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())

```

> Response 200:

```json
{"status": 200}
```

> Response 400

```json
{"error": ["Error", ...]}
```

Actualizar la contraseña de un usuario.
Las contraseña deben tener un mínimo de 6 caracteres y tener al menos 1 número y 1 letra.

### HTTP Request

`PATCH https://leasy.pe/api/v2/clients/password/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

### Body JSON parameters

Parámetro | Valor  | Descripción
--------- |--------| -----------
password | String | La contraseña actual.
new_password | String | La nueva contraseña.
confirm_new_password | String | La nueva confirmación de la contraseña.

## Obtener tarjetas guardadas

```python
import requests

r = requests.get(
    "https://leasy.pe/api/v2/clients/cards/",
    headers={
        "Authentication": "token",
    }
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())
```

> Response 200

```json
[{
	"id": 1,
	"card_number": "545195XXXXXX5480",
	"expiration_month": null,
	"expiration_year": null,
	"brand": "MASTERCARD",
	"kushki_subscription_id": "1676385575463000",
	"is_kushki_card": true
}, {
	"id": 2,
	"card_number": "454775******6006",
	"expiration_month": "03",
	"expiration_year": "22",
	"brand": "visa",
	"kushki_subscription_id": "",
	"is_kushki_card": false
}]
```

Podrás obtener todas las tarjetas guardadas por los clientes.

### HTTP Request

`GET https://leasy.pe/api/v2/clients/cards/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

## Obtener recargas

```python
import requests

r = requests.get(
    "https://leasy.pe/api/v2/clients/recharges/",
    headers={
        "Authentication": "token"
    }
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())

```

> Response 200:

```json
{
  "results": [
    {
      "method": "method",
      "amount": 0.0,
      "created_at": "created_at",
      "observation": "observations"
    },
    {
      "method": "method",
      "amount": 0.0,
      "created_at": "created_at",
      "observation": "observations"
    }
  ],
  "page": 1
}
```

> Response 400

```json
{"error": "Perfil de cliente no encontrado"}
```

Obtener todas las recargas que hizo el cliente.
El resultado estará paginado, por lo que es necesario enviar el valor de `page` en el request; si el valor de `page` no es enviado entonces el sistema asumirá su valor como `1`

### HTTP Request

`GET https://leasy.pe/api/v2/clients/recharges/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

### Body JSON parameters

Parámetro | Valor        | Descripción
--------- |--------------| -----------
page | String/Integer | La página de la cuál se obtendrán los resutados de la consulta

## Nuevo lead

```python
import requests

r = requests.post(
    "https://leasy.pe/api/v2/clients/new/",
    json={
        "first_name": "Lou",
        "last_name": "Alcalá",
        "document": "1044200864",
        "phone_number": "987928878",
        "email": "hola@loualcala.com",
        "time_working": "No uso Aplicativo",
        "utm_source": "APP",
        "utm_medium": "iOs"
    }
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())

```

> Response 200:

```json
{
  "status": 200
}
```

> Response 400

```json
{"error": [...]}
```

Registra un nuevo Lead

### HTTP Request

`POST https://leasy.pe/api/v2/clients/new/`

### Headers

No requiere el envío de Headers

### Body JSON parameters

Parámetro | Valor        | Descripción
--------- |--------------| -----------
first_name | String | Los nombres del Lead
last_name | String | Los apellidos del Lead
document | String | El número de documento de identidad del Lead
phone_number | String | El número de teléfono del Lead
email | String | El correo electrónico del Lead
time_working | String | El tiempo que lleva trabajando con aplicativo, es un valor libre, sin embargo actualmente trabajamos con estos valores: `Menos de 4 meses`, `Más de 4 meses`, `Más de 8 meses`, `Más de 12 meses`, `Más de 18 meses` y `No trabajo con aplicativo`
utm_source | String | La fuente del Lead, en este caso debería ser `APP`
utm_medium | String | El medio por el que proviene el Lead, nosotros actualmente usamos `iOs` o `Android`

# Contrato

## Obtener los invoices de un contrato

```python
import requests

r = requests.get(
    "https://leasy.pe/api/v2/contracts/<contract_uuid>/invoices/<status>/",
    headers={
        "Authentication": "token"
    }
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())

```

> Response 200:

```json
{
  "results": {
    "priority": "priority",
    "id": "id",
    "hash": "hash",
    "status": "status_code_name",
    "due_date": "due_date",
    "paid_date": "paid_date",
    "total": 0.0,
    "created_at": "created_at",
    "observations": "observations",
    "type": {
      "name": "type_name",
      "code_name": "type_code_name"
    },
    "contract": "contract_hash",
    "total_with_discount": 0.0,
    "discounts": [
      {
        "amount": 0.0,
        "observations": "observations",
        "type": "discount_type"
      },
      ...
    ]
  }
}
```

> Response 400

```json
{"error": ["Contracto no encontrado"]}
```

Para obtener los invoices de un contrato es necesario enviar el uuid del contrato del cliente, y luego enviar el code name del estado de invoice que se desea solicitar.

### HTTP Request

`GET https://leasy.pe/api/v2/contracts/<contract_uuid>/invoices/<status>/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

### URL parameters

Parámetro | Valor       | Descripción
--------- |-------------| -----------
contract_uuid | String | El UUID del contrato del cliente
status | String | El estado del invoice puede ser uno de los siguientes: `"pendings", "fractions", "freezed", "paid"`, tener en cuenta que es sensitivo al texto, por lo que se debe enviar el minúsculas

## Obtener detalles del carro

```python
import requests

r = requests.get(
    "https://leasy.pe/api/v2/contracts/<contract_uuid>/car/",
    headers={
        "Authentication": "token"
    }
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())

```

> Response 200:

```json
{
  "license_plate": "license_plate",
  "model": "model",
  "brand": "brand",
  "year": "year",
  "soat_file": "url",
  "secure_file": "url",
  "photos": ["url", "url", ...]
}
```

> Response 400

```json
{"error": ["Contracto no encontrado"]}
```

Para obtener los detalles del carro de un contrato es necesario enviar el UUID del contrato.

### HTTP Request

`GET https://leasy.pe/api/v2/contracts/<contract_uuid>/car/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

### URL parameters

Parámetro | Valor       | Descripción
--------- |-------------| -----------
contract_uuid | String | El UUID del contrato del cliente

# Commons

## Obtener los planes de UBER

```python
import requests

r = requests.get(
    "https://leasy.pe/api/v2/commons/uber_plans/"
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())

```

> Response 200:

```json
{
	"uber_plans": [
		{
			"name": "Azul",
			"code_name": "azul",
			"discount_percent": 0
		},
		{
			"name": "Oro",
			"code_name": "oro",
			"discount_percent": 5
		},
		{
			"name": "Platino",
			"code_name": "platino",
			"discount_percent": 7
		},
		{
			"name": "Diamante",
			"code_name": "diamante",
			"discount_percent": 10
		}
	]
}
```

Obtienes las categorías de los planes de UBER disponibles

### HTTP Request

`GET https://leasy.pe/api/v2/commons/uber_plans/`

### Headers

No requiere en envío de Headers

### URL parameters

No requiere el envío de parámetros

## Categoría de gastos

```python
import requests

r = requests.get(
    "https://leasy.pe/api/v2/commons/expense_categories/"
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())

```

> Response 200:

```json
{
	"categories": [
		{
			"name": "Mantenimiento",
			"code_name": "mantenimiento"
		},
		{
			"name": "Gastos Personales",
			"code_name": "gastos_personales"
		},
		{
			"name": "Combustible",
			"code_name": "combustible"
		},
		{
			"name": "Otros",
			"code_name": "otros"
		}
	]
}
```

Obtienes las categorías de los gastos

### HTTP Request

`GET https://leasy.pe/api/v2/commons/expense_categories/`

### Headers

No requiere el envío de Headers

### URL parameters

No requiere el envío de parámetros

## Contactos de Emergencias

```python
import requests

r = requests.get(
    "https://leasy.pe/api/v2/commons/emergency_contacts/"
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())

```

> Response 200:

```json
{
	"steal": [
		{
			"contact_name": "Luis Gómez",
			"in_case": "steal",
			"phone_number": "965448883",
			"description": ""
		},
		{
			"contact_name": "Rigoberto",
			"in_case": "steal",
			"phone_number": "953884777",
			"description": ""
		},
		{
			"contact_name": "Fernando Rey",
			"in_case": "steal",
			"phone_number": "915111333",
			"description": ""
		}
	],
	"block": [
		{
			"contact_name": "Cobranzas",
			"in_case": "block",
			"phone_number": "991337801",
			"description": ""
		}
	],
	"accidents": [
		{
			"contact_name": "Rigoberto",
			"in_case": "accidents",
			"phone_number": "953884777",
			"description": ""
		},
		{
			"contact_name": "Fernando Rey",
			"in_case": "accidents",
			"phone_number": "915111333",
			"description": ""
		},
		{
			"contact_name": "Gianluca",
			"in_case": "accidents",
			"phone_number": "963237491",
			"description": ""
		}
	]
}
```

Obtienes los teléfonos de contacto en caso de emergencias

### HTTP Request

`GET https://leasy.pe/api/v2/commons/emergency_contacts/`

### Headers

No requiere el envío de Headers

### URL parameters

No requiere el envío de parámetros

## F.A.Q

```python
import requests

r = requests.get(
    "https://leasy.pe/api/v2/commons/faqs/"
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())

```

> Response 200:

```json
[
	{
		"title": "Esto no lo es",
		"content": [
			"Hola a todos",
			"Hola a todos 2",
			"Hola a todos 3"
		],
		"created_at": "2023-07-19"
	},
	{
		"title": "Hola a todos",
		"content": [
			"Si, esto es el primero content"
		],
		"created_at": "2023-07-19"
	}
]
```

Obtienes las preguntas frecuentes

### HTTP Request

`GET https://leasy.pe/api/v2/commons/faqs/`

### Headers

No requiere el envío de Headers

### URL parameters

No requiere el envío de parámetros

## Categoría de tips

```python
import requests

r = requests.get(
    "https://leasy.pe/api/v2/commons/tips/categories/"
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())

```

> Response 200:

```json
[
	{
		"name": "Mantenimiento",
		"code_name": "mantenimiento",
		"description": ""
	},
	{
		"name": "Reparaciones",
		"code_name": "reparaciones",
		"description": ""
	},
	{
		"name": "Limpieza",
		"code_name": "limpieza",
		"description": ""
	},
	{
		"name": "Multas y tránsito",
		"code_name": "multas_y_transito",
		"description": ""
	},
	{
		"name": "Seguridad vehicular",
		"code_name": "seguridad_vehicular",
		"description": ""
	},
	{
		"name": "Presupuesto vehicular",
		"code_name": "presupuesto_vehicular",
		"description": ""
	}
]
```

Obtienes las categorías disponibles para los tips

### HTTP Request

`GET https://leasy.pe/api/v2/commons/tips/categories/`

### Headers

No requiere el envío de Headers

### URL parameters

No requiere el envío de parámetros


## Tips de Hoy / Populares

```python
import requests

r = requests.get(
    "https://leasy.pe/api/v2/commons/tips/today/", # Tips de Hoy
    # "https://leasy.pe/api/v2/commons/tips/popular/", # Tips populares
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())

```

> Response 200:

```json
[
	{
		"title": null,
		"content": "Hola a todos, esto es una prueba",
		"is_popular": false,
		"category": {
			"name": "Mantenimiento",
			"code_name": "mantenimiento",
			"description": ""
		},
		"created_at": "2023-07-19"
	}
]
```

Obtienes los tips de hoy o los populares en orden de creación

### HTTP Request

`GET https://leasy.pe/api/v2/commons/tips/today/`

`GET https://leasy.pe/api/v2/commons/tips/popular/`

### Headers

No requiere el envío de Headers

### URL parameters

No requiere el envío de parámetros

## Tips por categoría

```python
import requests

r = requests.get(
    "https://leasy.pe/api/v2/commons/tips/category/<category_codename>/"
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())

```

> Response 200:

```json
[
	{
		"title": null,
		"content": "Hola a todos, esto es una prueba",
		"is_popular": true,
		"category": {
			"name": "Mantenimiento",
			"code_name": "mantenimiento",
			"description": ""
		},
		"created_at": "2023-07-19"
	}
]
```

Obtienes los tips por categoría. "No tiene paginación"

### HTTP Request

`GET https://leasy.pe/api/v2/commons/tips/category/<category_codename>/`

### Headers

No requiere el envío de Headers

### URL parameters

Parámetro | Valor       | Descripción
--------- |-------------| -----------
category_codename | String | Codename de la categoría. Revisar el endpoint para obtener todas las categorías

