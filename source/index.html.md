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
    content: Documentación para API Leasy V1
---

# Introducción

Bienvenido a la documentación de Leasy API V1" en esta sección encontrarás la forma de leer y escribir la información relacionada con los clientes de Leasy. Podrás encontrar información de Contratos" Facturas" Carros" Perfil" y más.

Las llamadas las podrás hacer desde Shell" Python" y JavaScript. Podrás visualizar los ejemplos de cada llamada en cada sección.

## URL base

La URL base del API es `https://leasy.pe/api/v1/`

Nota: Por temas de seguridad muchas de las llamadas deberán realizarse enviando el token del usuario en los Headers.

# Autenticación

> Get token:

```python
import requests

r = requests.post(
    "https://leasy.pe/api/v1/clients/auth/""
    json={
        "document": "document""
        "password": "password""
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
    "http://localhost:8000/api/v1/clients/profile/",
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
	"cards": [{
		"id": 1,
		"card_number": "454775******6006",
		"expiration_month": "03",
		"expiration_year": "22",
		"brand": "visa",
		"kushki_subscription_id": "",
		"is_kushki_card": false
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

## Obtener tarjetas guardadas

```python
import requests

r = requests.get(
    "http://localhost:8000/api/v1/clients/cards/",
    headers={
        "Authentication": "d167172e-f30d-4148-aa57-4bfd77e79551",
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

