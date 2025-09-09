# Cloud Computing Assignment1 – Docker Two-tier Stack

## What the stack does

This assignment leverages a dual-service cloud stack with Docker Compose. A startup SQL script is necessary to contain the trip data required for initiating a PostgreSQL database container. For executing queries, a Python application container is necessary to be connected to the database. It computes metrics such as total trips, average fares city-wise, and the lengthy trips. The outcome shows up in the terminal and gets saved as a JSON file on the host laptop.

## Exact commands to run and stop

1. Make sure Docker Desktop (or Docker Engine + Compose plugin) is installed and running.

### 2. **To build and run the full stack**

```bash
docker compose up --build
```
### 3. **To stop the containers and keep data volumes**

```bash
docker compose down
```
### 4. **To stop and also remove volumes and output files (fresh run)**

```bash
docker compose down -v
rm -rf out && mkdir -p out
```
### 5. **In case of Makefile**

If you are using the included Makefile, you can use convenient shortcuts:
make up → build and start the stack
make down → stop and remove services/volumes
make clean → remove containers, volumes, and regenerate an empty out directory

## Example Output

When the application runs successfully, the Python service prints a JSON summary to the terminal 
and also stores it in out/summary.json.

Output:

```bash
{

  "total_trips": 6,

  "avg_fare_by_city": [

    {

      "city": "New York",

      "avg_fare": 19.0

    },

    {

      "city": "San Francisco",

      "avg_fare": 20.25

    },

    {

      "city": "Charlotte",

      "avg_fare": 16.25

    }

  ],
  "top_by_minutes": [

    {

      "city": "San Francisco",

      "minutes": 28,

      "fare": 29.3

    },

    {

      "city": "New York",

      "minutes": 26,

      "fare": 27.1

    },

    {

      "city": "Charlotte",

      "minutes": 21,

      "fare": 20.0

    },

    {

      "city": "Charlotte",

      "minutes": 12,

      "fare": 12.5

    },

    {

      "city": "San Francisco",

      "minutes": 11,

      "fare": 11.2

    },

    {

      "city": "New York",

      "minutes": 9,

      "fare": 10.9

    }

  ]

}

```
## Where outputs are written
