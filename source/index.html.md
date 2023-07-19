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

La URL base del API es `https://leasy.pe/api/v1/`

Nota: Por temas de seguridad muchas de las llamadas deberán realizarse enviando el token del usuario en los Headers.

# Autenticación

> Get token:

```python
import requests

r = requests.post(
    "https://leasy.pe/api/v1/clients/auth/",
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

`POST https://leasy.pe/api/v1/clients/auth/`

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
    "https://leasy.pe/api/v1/clients/profile/",
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
	"id": 1,
	"first_name": "first_name",
	"last_name": "last_name",
	"email": "email",
	"document": "document",
	"address": "address",
	"phone_number": "phone_number",
	"created_at": "2018-09-27",
	"coins": 0.0,
	"token": "token",
	"firebase_token": "firebase_token",
	"contracts": [{
		"car": {
			"id": 1,
			"hash": "hash",
			"model": "Cobalt",
			"brand": "Chevrolet",
			"soat_file": "/static/files/soat/soat_file.pdf",
			"secure_file": "/static/files/secure/secure_file.pdf",
			"license_plate": "license_plate",
			"year": "2018"
		},
		"status": "contract_active",
		"created_at": "2019-01-05",
		"hash": "hash",
		"weekly_amount": 525,
		"auto_charge": true
	}],
	"phones": []
}
```

> Response 400

```json
{"error": "Perfil de cliente no encontrado"}
```

Podrás obtener toda la información relacionada al cliente.

### HTTP Request

`GET https://leasy.pe/api/v1/clients/profile/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

## Actualizar contraseña

```python
import requests

r = requests.patch(
    "https://leasy.pe/api/v1/clients/password/",
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

`GET https://leasy.pe/api/v1/clients/password/`

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
    "https://leasy.pe/api/v1/clients/cards/",
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

`GET https://leasy.pe/api/v1/clients/cards/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

## Obtener recargas

```python
import requests

r = requests.get(
    "https://leasy.pe/api/v1/clients/recharges/",
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

`GET https://leasy.pe/api/v1/clients/recharges/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

### Body JSON parameters

Parámetro | Valor        | Descripción
--------- |--------------| -----------
page | String/Integer | La página de la cuál se obtendrán los resutados de la consulta

# Contrato

## Obtener los invoices de un contrato

```python
import requests

r = requests.get(
    "https://leasy.pe/api/v1/contracts/<contract_uuid>/invoices/<status>/",
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

`GET https://leasy.pe/api/v1/contracts/<contract_uuid>/invoices/<status>/`

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
    "https://leasy.pe/api/v1/contracts/<contract_uuid>/car/",
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
  "secure_file": "url"
}
```

> Response 400

```json
{"error": ["Contracto no encontrado"]}
```

Para obtener los detalles del carro de un contrato es necesario enviar el UUID del contrato.

### HTTP Request

`GET https://leasy.pe/api/v1/contracts/<contract_uuid>/car/`

### Headers

Parameter | Default | Description
--------- |---------| -----------
Authentication | String  | Se debe enviar el token del usuario

### URL parameters

Parámetro | Valor       | Descripción
--------- |-------------| -----------
contract_uuid | String | El UUID del contrato del cliente

