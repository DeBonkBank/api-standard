# Noob API Standard
> Please make sure to read the [NOOB wiki](https://gitlab.cmi.hro.nl/technische-informatica/vakken/project-3-4-tinprj0x-34/noob/-/wikis/API) for NOOB requirements like `NOOB-TOKEN` and bank targeting as those will not be explained here. It will also explain in more detail how exactly the NOOB proxy routes your requests.

> If you want to make any changes please [open an issue](https://github.com/DeBonkBank/api-standard/issues) or send a message in the Teams chat.

> ## **RFID Card**
> Standardizatie voor RFID pinpas kaarten.
> - sector 0:
>   - blok 1: eerste deel iban (bijv. `NLXXBONB`)
>   - blok 2: tweede deel iban (bijv. `XXXXXXXXXX`)
> - sector 1: 
>   - blok 4: pas nummer (`XXX`)

## **Health Check**
#### **GET** `/api/noob/health`
Checks if the API is running.

**Response:**
```json
{
  "status": "OK"
}
```

## **User endpoints**
### Error responses
Als er een foutief request wordt verstuurd, moet hierop worden gereageerd met de meest toepasselijke [HTTP-statuscode](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status#client_error_responses), in plaats van de standaard 200 OK-response.
De response moet minimaal de velden error en message bevatten.

> **Let op:** deze data wordt teruggestuurd naar de client en mag daarom **geen** gevoelige informatie bevatten.

Bijvoorbeeld:
```json
HTTP/1.1 401 Unauthorized
{
  "error": "AuthenticationFailed",
  "message": "Invalid credentials."
}
```

### Get User Info
#### **POST** `/api/users/getinfo`
Retrieve account details for a given IBAN.

**Example Request:**  
```
POST {NOOB_SERVER}/api/noob/users/getinfo
```  
**Request Body:**
```json  
{
  "iban": "xxxx",
  "pin": "1234",
  "pasnummer": "123"
}
```

**Example Response:**
```json
{
  "iban": "xxxx",
  "saldo": 1234.00,
  "valuta": "EUR"
}
```

### Withdraw Funds
#### **POST** `/api/users/withdraw`
Withdraws an amount from the specified IBAN.

**Example Request:**  
```
POST {NOOB_SERVER}/api/noob/users/withdraw
``` 
**Request Body:**
```json
{
  "iban": "xxxx",
  "pin": "1234",
  "pasnummer": "123"
  "amount": 100.00
}
```
**Example Response:**
> `message` may be ignored on this response if status code is 200, but can be used to give more info to the user on why a request failed, like `"message": "Insufficient Funds"`.

```json
{
  "iban": "xxxx",
  "message": "Withdrawal successful"
}
```
