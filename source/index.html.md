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

La URL base del API es `{{ domain }}/api/v2/`

Nota: Por temas de seguridad muchas de las llamadas deberán realizarse enviando el token del usuario en los Headers.

# Autenticación

> Get token:

```python
import requests

r = requests.post(
    "{{ domain }}/api/v2/clients/auth/",
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

`POST {{ domain }}/api/v2/clients/auth/`

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
    "{{ domain }}/api/v2/clients/profile/",
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
        "photos": [
          "url",
          "url",
          "..."
        ]
      },
      "id": 66,
      "status": "contract_active",
      "created_at": "2019-01-05",
      "hash": "0b935679-d439-4463-b24a-1c9d54ce7542",
      "weekly_amount": 525,
      "auto_charge": true,
      "summary": {
        "total_invoices": 130,
        "total_invoices_paid": 1,
        "total_invoices_pending": 71,
        "pending_penalties": 0
      },
      "kushki_keys": {
        "pk": "..."
      }
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

`GET {{ domain }}/api/v2/clients/profile/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

## Actualizar contraseña

```python
import requests

r = requests.patch(
    "{{ domain }}/api/v2/clients/password/",
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
{"error": ["Error", "..."]}
```

Actualizar la contraseña de un usuario.
Las contraseña deben tener un mínimo de 6 caracteres y tener al menos 1 número y 1 letra.

### HTTP Request

`PATCH {{ domain }}/api/v2/clients/password/`

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
    "{{ domain }}/api/v2/clients/cards/",
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

`GET {{ domain }}/api/v2/clients/cards/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

## Guardar tarjeta kushki

```python
import requests

r = requests.post(
    "{{ domain }}/api/v2/clients/kushki-cards/",
    headers={
        "Authentication": "token",
    },
    json={
        "token": "card token",
    }
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())
```

> Response 200

```json
{
	"id": 1,
	"card_number": "545195XXXXXX5480",
	"expiration_month": null,
	"expiration_year": null,
	"brand": "MASTERCARD",
	"kushki_subscription_id": "1676385575463000",
	"is_kushki_card": true
}
```

Guardar una nueva tarjeta de Kushki tokenizada

### HTTP Request

`POST {{ domain }}/api/v2/clients/kushki-cards/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario


### Body JSON parameters

Parámetro | Valor        | Descripción
--------- |--------------| -----------
token | String | El token de la tarjeta guardada

## Obtener recargas

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/clients/recharges/",
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
      "observation": "observations",
      "type": "cash_in"
    },
    {
      "method": "method",
      "amount": 0.0,
      "created_at": "created_at",
      "observation": "observations",
      "type": "cash_in"
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

`GET {{ domain }}/api/v2/clients/recharges/`

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
    "{{ domain }}/api/v2/clients/new/",
    json={
        "first_name": "Lou",
        "last_name": "Alcalá",
        "document": "1044200864",
        "phone_number": "987928878",
        "email": "hola@loualcala.com",
        "time_working": "",
        "utm_source": "APP",
        "utm_medium": "iOs",
        "car_of_interest": "Nuevo"
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
{"error": ["..."]}
```

Registra un nuevo Lead

### HTTP Request

`POST {{ domain }}/api/v2/clients/new/`

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
car_of_interest | String | `Nuevo` o `Semi nuevo`

# Contrato

## Obtener los invoices de un contrato

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/contracts/<contract_uuid>/invoices/<status>/",
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

`GET {{ domain }}/api/v2/contracts/<contract_uuid>/invoices/<status>/`

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
    "{{ domain }}/api/v2/contracts/<contract_uuid>/car/",
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
	"license_plate": "PRUEBAS",
	"model": "Cobalt",
	"brand": "Chevrolet",
	"year": "2018",
	"photos": [
		"https://leasy.pe/static/files/va_5dd8e6101a2947c1be5b91c0434fbb85_S9SJqOV.jpg"
	],
	"ownership_card": {
		"owner": "Pruebas Apellido",
		"due_date": "2024-12-30",
		"license_plate": "PRUEBAS"
	},
	"soat": {
		"file_url": "https://leasy.pe/static/files/soat/ACFrOgAoaFUTD4A4AnpmopQwUz2-S85W3hGtyH4TRioVEZd8TgD2Qag6y7PMXgqLyNEzEA_uyH61fJ.pdf",
		"label": "SOAT",
		"number": "12345678",
		"owner": null,
		"due_date": "2024-12-30"
	},
	"insurance": {
		"file_url": "https://leasy.pe/static/files/secure/ACFrOgAoaFUTD4A4AnpmopQwUz2-S85W3hGtyH4TRioVEZd8TgD2Qag6y7PMXgqLyNEz_DAMViCj.pdf",
		"label": "Licencia de conducir",
		"number": null,
		"owner": "Pruebas Apellido",
		"due_date": "2024-12-30"
	},
	"driver_license": {
		"file_url": null,
		"label": "Licencia de conducir",
		"number": null,
		"owner": "Pruebas Apellido",
		"due_date": "2024-12-30"
	},
	"technical_inspection": {
		"file_url": null,
		"label": "Revisión técnica",
		"number": null,
		"owner": "Pruebas Apellido",
		"due_date": "2024-12-30"
	}
}
```

> Response 400

```json
{"error": ["Contracto no encontrado"]}
```

Para obtener los detalles del carro de un contrato es necesario enviar el UUID del contrato.

### HTTP Request

`GET {{ domain }}/api/v2/contracts/<contract_uuid>/car/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

### URL parameters

Parámetro | Valor       | Descripción
--------- |-------------| -----------
contract_uuid | String | El UUID del contrato del cliente

# Invoices

## Obtener resumen de pago a efectuar

```python
import requests

r = requests.post(
    "{{ domain }}/api/v2/invoices/pay-summary/",
    headers={
        "Authentication": "token"
    },
    json={
        "invoices": [1, 2, ]
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
	"selected_invoices": [
    # Misma información de Contrato > Obtener los invoices de un contrato"
	],
	"active_fractions": [
		{}
	],
	"invoices_total_amount": 998,
	"fractions_total_amount": 44,
	"total_to_pay": 1042,
  "total_to_recharge": 20
}
```

> Response 400

```json
{"error": ["missing parameters"]}
```
```json
{"error": ["client not found"]}
```

Obtén el resumen de los invoices que se va a pagar, adicionalmente se obtiene las fracciones activas que serán cobradas.
El endpoint también trae los montos a pagar por invoices seleccionados, por fracciones activas y por el total.

### HTTP Request

`POST {{ domain }}/api/v2/invoices/pay-summary/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

### URL parameters

Parámetro | Valor  | Descripción
--------- |--------| -----------
invoices | List   | La lista de todos los invoices seleccionados para pagar

## Pagar varios invoices

```python
import requests

r = requests.post(
    "{{ domain }}/api/v2/invoices/pay-pool/",
    headers={
        "Authentication": "token"
    },
    json={
        "invoices": [1, 2, ]
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
	"message": "Se pagan las facturas: 1, 2 . Por el monto de S/ 499"
}
```

> Response 400

```json
{"error": ["missing parameters"]}
```
```json
{"error": ["client not found"]}
```
```json
{"error": ["El monto a pagar es inválido '0'"]}
```
```json
{"error": ["No tienes suficientes coins: 0"]}
```

Paga varias cuotas seleccionadas.

### HTTP Request

`POST {{ domain }}/api/v2/invoices/pay-pool/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

### URL parameters

Parámetro | Valor  | Descripción
--------- |--------| -----------
invoices | List   | La lista de todos los invoices seleccionados para pagar

# Commons

## Obtener los planes de UBER

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/commons/uber_plans/"
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
			"discount_percent": 0,
      "description": "..."
		},
		{
			"name": "Oro",
			"code_name": "oro",
			"discount_percent": 5,
      "description": "..."
		},
		{
			"name": "Platino",
			"code_name": "platino",
			"discount_percent": 7,
      "description": "..."
		},
		{
			"name": "Diamante",
			"code_name": "diamante",
			"discount_percent": 10,
      "description": "..."
		}
	]
}
```

Obtienes las categorías de los planes de UBER disponibles

### HTTP Request

`GET {{ domain }}/api/v2/commons/uber_plans/`

### Headers

No requiere en envío de Headers

### URL parameters

No requiere el envío de parámetros

## Categoría de gastos

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/commons/expense_categories/"
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

`GET {{ domain }}/api/v2/commons/expense_categories/`

### Headers

No requiere el envío de Headers

### URL parameters

No requiere el envío de parámetros

## Contactos de Emergencias

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/commons/emergency_contacts/"
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())

```

> Response 200:

```json
{
  "accidents": {
    "contacts": [
      {
        "contact_name": "Rigoberto",
        "in_case": "accidents",
        "phone_number": "953884777",
        "description": ""
      },
    ],
    "info": {
      "name": "Accidentes",
      "description": "En caso de accidentes"
    }
  },
  "block": {
    "contacts": [
      {
        "contact_name": "Cobranzas",
        "in_case": "block",
        "phone_number": "991337801",
        "description": ""
      },
    ],
    "info": {
      "name": "Bloqueo de vehículo",
      "description": "En caso de bloqueos por falta de pago"
    }
  },
  "steal": {
    "contacts": [
      {
        "contact_name": "Luis Gómez",
        "in_case": "steal",
        "phone_number": "965448883",
        "description": ""
      },
    ],
    "info": {
      "name": "Robo",
      "description": "En caso de robo de vehículo"
    }
  }
}
```

Obtienes los teléfonos de contacto en caso de emergencias

### HTTP Request

`GET {{ domain }}/api/v2/commons/emergency_contacts/`

### Headers

No requiere el envío de Headers

### URL parameters

No requiere el envío de parámetros

## Soporte mecánico

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/commons/mechanical-contacts/"
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())

```

> Response 200:

```json
{
  "mechanical": {
    "contact_name": "Soporte Mecánico",
    "phone_number": "+51987928878"
  },
  "emergency": {
    "contact_name": "Emergencias",
    "phone_number": "+51987928878"
  }
}
```

Obtienes los teléfonos de los contacto mecánico

### HTTP Request

`GET {{ domain }}/api/v2/commons/mechanical-contacts/`

### Headers

No requiere el envío de Headers

### URL parameters

No requiere el envío de parámetros

## Afiliaciones UBER

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/commons/uber-contacts/"
)

if r.status_code != 200:
    print(r.text)
else:
    print(r.json())

```

> Response 200:

```json
{
  "contact_name": "Afiliaciones UBER",
  "phone_number": "+51987928878"
}
```

Obtienes los teléfonos para solicitar la afiliación a UBER

### HTTP Request

`GET {{ domain }}/api/v2/commons/uber-contacts/`

### Headers

No requiere el envío de Headers

### URL parameters

No requiere el envío de parámetros

## Contactos problemas de servicio

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/commons/service_contacts/"
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
    "label": "Teléfono",
    "code_name": "phone_number",
    "value": "947 947 158"
  },
  {
    "label": "Correo electrónico",
    "code_name": "email_address",
    "value": "ventas@leasyauto.com"
  }
]
```

Obtiene los datos de contacto en caso de tener un problema con el servicio

### HTTP Request

`GET {{ domain }}/api/v2/commons/service_contacts/`

### Headers

No requiere el envío de Headers

### URL parameters

No requiere el envío de parámetros

## F.A.Q

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/commons/faqs/"
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

`GET {{ domain }}/api/v2/commons/faqs/`

### Headers

No requiere el envío de Headers

### URL parameters

No requiere el envío de parámetros

## Categoría de tips

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/commons/tips/categories/"
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

`GET {{ domain }}/api/v2/commons/tips/categories/`

### Headers

No requiere el envío de Headers

### URL parameters

No requiere el envío de parámetros


## Tips de Hoy / Populares

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/commons/tips/today/", # Tips de Hoy
    # "{{ domain }}/api/v2/commons/tips/popular/", # Tips populares
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
		"content": ["...", "..."],
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

`GET {{ domain }}/api/v2/commons/tips/today/`

`GET {{ domain }}/api/v2/commons/tips/popular/`

### Headers

No requiere el envío de Headers

### URL parameters

No requiere el envío de parámetros

## Tips por categoría

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/commons/tips/category/<category_codename>/"
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
		"content": ["...", "..."],
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

`GET {{ domain }}/api/v2/commons/tips/category/<category_codename>/`

### Headers

No requiere el envío de Headers

### URL parameters

Parámetro | Valor       | Descripción
--------- |-------------| -----------
category_codename | String | Codename de la categoría. Revisar el endpoint para obtener todas las categorías

# Finanzas

## Obtener los incomes

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/finances/income/",
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
[
	{
		"amount": "10.00",
		"created_at": "2023-11-07 02:10:25"
	},
	{
		"amount": "12.40",
		"created_at": "2023-11-07 02:09:00"
	}
]
```

> Response 400

```json
{"error": ["..."]}
```

Obtén todos los incomes registrados

### HTTP Request

`GET {{ domain }}/api/v2/finances/income/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

## Registrar incomes

```python
import requests

r = requests.post(
    "{{ domain }}/api/v2/finances/income/",
    headers={
        "Authentication": "token"
    },
    json={
      "amount": 10
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
	"status": true
}
```

> Response 400

```json
{"error": ["..."]}
```

Registra los nuevos incomes

### HTTP Request

`POST {{ domain }}/api/v2/finances/income/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

### URL parameters

Parámetro | Valor   | Descripción
--------- |---------| -----------
amount | Decimal | El monto total a registrar

## Resumen de incomes

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/finances/income/summary/",
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
	"message": "success",
	"start_date": null,
	"end_date": null,
	"month": "November",
	"total": "22.40",
	"details": [
		{
			"amount": "10.00",
			"date": "2023-11-07"
		},
		{
			"amount": "12.40",
			"date": "2023-11-07"
		}
	]
}
```

> Response 400

```json
{"error": ["..."]}
```

Obtén todos los incomes registrados en un periodo

### HTTP Request

`GET {{ domain }}/api/v2/finances/income/summary/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

### URL parameters

Parámetro | Valor   | Descripción
--------- |---------| -----------
month | Integer | El mes que deseas consultar
year | Integer | El año que deseas consultar
start_date | String  | La fecha de inicio YYYY-MM-DD
end_date | String  | La fecha de fin YYYY-MM-DD

## Obtener expenses

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/finances/expense/",
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
[
	{
		"amount": "13.00",
		"created_at": "2023-11-07 02:24:57",
		"category": {
			"name": "testing",
			"icon": "",
			"code_name": "testing"
		}
	},
	{
		"amount": "13.00",
		"created_at": "2023-11-07 02:24:47",
		"category": {
			"name": "testing",
			"icon": "",
			"code_name": "testing"
		}
	}
]
```

> Response 400

```json
{"error": ["..."]}
```

Obtén todos los expenses registrados

### HTTP Request

`GET {{ domain }}/api/v2/finances/expense/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

## Registrar expenses

```python
import requests

r = requests.post(
    "{{ domain }}/api/v2/finances/expense/",
    headers={
        "Authentication": "token"
    },
    json={
      "amount": 13,
      "category": "testing"
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
	"status": true
}
```

> Response 400

```json
{"error": ["..."]}
```

Registra nuevos expenses

### HTTP Request

`POST {{ domain }}/api/v2/finances/expense/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

### URL parameters

Parámetro | Valor   | Descripción
--------- |---------| -----------
amount | Decimal | El monto a registrar
category | String  | El code_name de la categoría

## Obtener categorias de expenses

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/finances/expense/categories/",
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
[
	{
		"name": "testing",
		"icon": "",
		"code_name": "testing"
	}
]
```

> Response 400

```json
{"error": ["..."]}
```

Obtén todas las categorías de los expenses habilitados

### HTTP Request

`GET {{ domain }}/api/v2/finances/expense/categories/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

## Obtener summary de expenses

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/finances/expense/summary/?month=11",
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
	"message": "success",
	"start_date": null,
	"end_date": null,
	"month": "November",
	"total": "26.00",
	"details": [
		{
			"amount": "13.00",
			"category_name": "testing",
			"date": "2023-11-07"
		},
		{
			"amount": "13.00",
			"category_name": "testing",
			"date": "2023-11-07"
		}
	]
}
```

> Response 400

```json
{"error": ["..."]}
```

Obtén todos los expenses en un rango de fecha

### HTTP Request

`GET {{ domain }}/api/v2/finances/expense/summary/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

### URL parameters

Parámetro | Valor   | Descripción
--------- |---------| -----------
month | Integer | El mes que deseas consultar
year | Integer | El año que deseas consultar
start_date | String  | La fecha de inicio YYYY-MM-DD
end_date | String  | La fecha de fin YYYY-MM-DD

## Financial summary

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/finances/summary/",
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
	"message": "success",
	"start_date": null,
	"end_date": null,
	"month": "November",
	"total_income": "22.40",
	"total_expense": "26.00",
	"total_balance": "-3.60",
	"details": {
		"incomes": [],
		"expenses": []
	}
}
```

> Response 400

```json
{"error": ["..."]}
```

Obtén todos los incomes registrados

### HTTP Request

`GET {{ domain }}/api/v2/finances/summary/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

### URL parameters

Parámetro | Valor   | Descripción
--------- |---------| -----------
month | Integer | El mes que deseas consultar
year | Integer | El año que deseas consultar
start_date | String  | La fecha de inicio YYYY-MM-DD
end_date | String  | La fecha de fin YYYY-MM-DD

## Financial balance

```python
import requests

r = requests.get(
    "{{ domain }}/api/v2/finances/summary/balance/",
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
	"total_income": "22.40",
	"total_expense": "26.00",
	"total_balance": "-3.60"
}
```

> Response 400

```json
{"error": ["..."]}
```

Obtén todos los incomes registrados

### HTTP Request

`GET {{ domain }}/api/v2/finances/summary/balance/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario
