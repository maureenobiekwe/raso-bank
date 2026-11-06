# Nigerian Banks API

An HTTP API for looking up Nigerian bank directory information, including bank codes, slugs, USSD codes, and locally hosted logos. The data can be used to populate bank selectors and other fintech interfaces.


## What this project does

The server reads bank records from `banks.json` and serves them as JSON. It also serves the logo images in `logos/`.

This is a read-only bank directory, not a banking or payment service. It does not create accounts, hold balances, authenticate customers, make transfers, or connect to bank payment networks. Bank details are reference data; verify them with the relevant institution before relying on them.

## Requirements

- Go (the version specified in `go.mod`)
- Docker and Docker Compose, if you prefer to run the container

## Run locally

Clone this repository and enter its directory:

```sh
git clone https://github.com/maureenobiekwe/raso_bank.git
cd raso_bank
```

Set the server configuration using environment variables or a `.env` file:

```dotenv
PORT=9090
HOST=http://localhost:9090
```

`HOST` is used to construct the logo URLs returned by the API. Set it to the base URL where this server can be reached.

Start the server:

```sh
go run .
```

The API will be available at `http://localhost:9090` with the configuration above.

## Run with Docker Compose

The included Compose configuration publishes the service on port `9090`. Set `HOST` if clients will access the service at a different base URL:

```sh
docker compose up --build
```

For example, to use a deployed hostname:

```dotenv
HOST=https://api.example.com
```

## API

### List all banks

```http
GET /
```

Example response:

```json
[
  {
    "name": "Access Bank",
    "slug": "access-bank",
    "code": "044",
    "ussd": "*901#",
    "logo": "http://localhost:9090/logo/access-bank.png"
  }
]
```

The response contains the full bank directory. Each record includes:

| Field | Description |
| --- | --- |
| `name` | Bank or financial institution name |
| `slug` | URL-friendly identifier |
| `code` | Bank code as a string |
| `ussd` | USSD code, if available |
| `logo` | URL of the bank logo or the default image |

### Look up a bank

Filter by bank code:

```http
GET /?code=044
```

Filter by slug:

```http
GET /?slug=access-bank
```

Both filters can be provided together. When both are present, a record is returned only if both match:

```http
GET /?code=044&slug=access-bank
```

A successful lookup returns one bank object. If there is no match, the response is JSON `null`.

### Bank logos

Logo images are served under `/logo/`, for example:

```text
GET /logo/access-bank.png
```

If a bank does not have a matching image in `logos/`, its API response points to `default-image.png`.

## Data

Bank records are maintained in [`banks.json`](./banks.json). Logo files are stored in [`logos/`](./logos/). The server uses these local files; it does not fetch live bank data.

## License

This project is licensed under the GNU General Public License, version 3 or (at your option) any later version. See [LICENSE](./LICENSE) for the full text. When distributing modified versions, you must follow the license terms, including its requirements for source code and licensing.

## Maintainer

**Maureen Obiekwe**  
[GitHub](https://github.com/maureenobiekwe) · [Portfolio](https://maureenobiekwe.com)
# raso_bank
# raso_bank
