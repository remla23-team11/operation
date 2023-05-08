# operation
This repository contains all deployment and management files for the whole project for REMLA 2023.

# Local single deployment of the service stack
To deploy the service stack locally in a single deployment you can use the following docker compose as a start of:
```
services:
  backend:
    image: ghcr.io/remla23-team11/app:latest
    ports:
      - 5000

  frontend:
    image: ghcr.io/remla23-team11/model-service:latest
    ports:
      - 8080
    environment:
      - REACT_APP_API_URL=backend:8080
```

## Variables

* REACT_APP_API_URL — Used to set the backend URL that the frontend has to communicate with.