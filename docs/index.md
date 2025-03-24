# Noob API Standard
> Please make sure to read the [NOOB wiki](https://gitlab.cmi.hro.nl/technische-informatica/vakken/project-3-4-tinprj0x-34/noob/-/wikis/API) for NOOB requirements like `NOOB-TOKEN` and bank targeting as those will not be explained here. It will also explain in more detail how exactly the NOOB proxy routes your requests.

> If you want to make any changes please [open an issue](https://github.com/DeBonkBank/api-standard/issues) or send a message in the Teams chat.

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
### Get User Info
#### **POST** `/users/getinfo`
Retrieve account details for a given IBAN.

**Example Request:**  
```
POST {NOOB_SERVER}/api/noob/users/getinfo
```  
**Request Body:**
```json  
{
  "iban": "xxxx",
  "pin": 1234
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
#### **POST** `/users/withdraw`
Withdraws an amount from the specified IBAN.

**Example Request:**  
```
POST {NOOB_SERVER}/api/noob/users/withdraw
``` 
**Request Body:**
```json
{
  "iban": "xxxx",
  "pin": 1234,
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
