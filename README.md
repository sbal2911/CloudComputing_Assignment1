# Cloud Computing Assignment1 – Docker Two-tier Stack

## What the stack does

This assignment leverages a dual-service cloud stack with Docker Compose. A startup SQL script is necessary to contain the trip data required for initiating a PostgreSQL database container. For executing queries, a Python application container is necessary to be connected to the database. It computes metrics such as total trips, average fares city-wise, and the lengthy trips. The outcome shows up in the terminal and gets saved as a JSON file on the host laptop.

## Exact commands to run and stop

### 1. Make sure Docker Desktop (or Docker Engine + Compose plugin) is installed and running.

### 2. **To build and run the full stack using run.sh file**

```bash
./run.sh
```

### 3. **In case of Makefile**

If the included Makefile is used for execution, convenient shortcuts can be useful:

``` make up ``` → build and start the stack

``` make down ``` → stop and remove services/volumes

``` make clean ``` → remove containers, volumes, and regenerate an empty out directory

### 4. **To build and run the full stack using Docker Compose command**

```bash
docker compose up --build
```
### 5. **To stop the containers and keep data volumes**

```bash
docker compose down
```
### 6. **To stop and also remove volumes and output files (fresh run)**

```bash
docker compose down -v
rm -rf out && mkdir -p out
```

## Example Output

On one successful run of the application, the Python service prints a JSON summary to the terminal 
and also stores it in ``` out/summary.json ```.

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
      "id": 6,
      "city": "San Francisco",
      "minutes": 28,
      "fare": 29.3
    },
    {
      "id": 4,
      "city": "New York",
      "minutes": 26,
      "fare": 27.1
    },
    {
      "id": 2,
      "city": "Charlotte",
      "minutes": 21,
      "fare": 20.0
    },
    {
      "id": 1,
      "city": "Charlotte",
      "minutes": 12,
      "fare": 12.5
    },
    {
      "id": 5,
      "city": "San Francisco",
      "minutes": 11,
      "fare": 11.2
    },
    {
      "id": 3,
      "city": "New York",
      "minutes": 9,
      "fare": 10.9
    }
  ]
}

```
## Where outputs are written

1. The Python app writes its final JSON file to the container path ``` /out/summary.json ```.

2. Through a volume mount, this file is available on the host at:

```bash
./out/summary.json
```
This directory is stored between container runs unless explicitly deleted.

## Troubleshooting

### 1. **Database not ready**

If the Python application exits quickly with a connection error, it may be because the app started before PostgreSQL was fully initialized. To verify the database’s status, you can run ``` docker compose logs db ``` and check for readiness. The ``` healthcheck ``` included in ``` compose.yml ``` is designed to prevent this issue under normal circumstances. However, if the error persists, restarting the stack with ``` docker compose up --build ``` usually resolves the problem.

### 2. **Permission errors on out/ directory**

If the application is unable to write the JSON file, ensure that the ``` out/ ``` directory exists and that your user has write permissions for it. On Mac OS systems with M1 chip, you can correct the permissions by running ``` chmod 755 out ```, which allows the directory to be accessible and writable as needed.

### 3. **Code changes require rebuild**

If you make changes to ``` main.py ``` or any of the SQL scripts, you need to rebuild the Docker images before running the stack to ensure your modifications take effect. This can be done using the command ``` ./run.sh ```
