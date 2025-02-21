# Distributed Task Queue in Golang

## Overview

This project is a distributed task queue built in Golang, leveraging Redis for message brokering and MongoDB for task persistence. It enables efficient background task execution using worker goroutines that fetch, process, and update task statuses with automatic retries on failure.

## Tech Stack

Golang – Backend processing

Redis – Message queue for task distribution

MongoDB – Task persistence and status tracking

Goroutines – Concurrent task processing

### Features

Task Enqueue & Dequeue: Tasks are pushed to Redis and popped by workers.
Worker Pool: Multiple workers process tasks concurrently.
Status Tracking: MongoDB maintains task status (queued, in-progress, failed, success).
Automatic Retry: Failed tasks are requeued for reprocessing.
Scalability: Easily scales with additional workers.

### How It Works

Task Enqueue:
Tasks are pushed to the Redis queue.

Example:
err := redis.InsertTask(taskID)

Worker Execution:

Workers fetch tasks from Redis, process them, and update MongoDB.
Failed tasks are requeued for retry.

Status Updates:

Task status is updated in MongoDB (queued, in-progress, failed, success).

```
distributed-task-queue/
│── cmd/                  # Entry points for different services
│   ├── api/              # API server (Task Producer)
│   │   ├── main.go       # Starts the API service
│   ├── worker/           # Worker service
│   │   ├── main.go       # Starts the worker service
│── config/               # Configuration files (env, Redis, MongoDB)
│   ├── config.go         # Loads environment variables
│── internal/             # Core business logic
│   ├── models/           # Database models
│   │   ├── task.go       # Task struct, DB schema
│   ├── queue/            # Redis queue logic
│   │   ├── redis.go      # Push and pull tasks from Redis
│   ├── repository/       # MongoDB interactions
│   │   ├── task_repo.go  # Insert, update, retrieve tasks
│   ├── worker/           # Worker pool logic
│   │   ├── worker.go     # Worker logic to process tasks
│── pkg/                  # Utility functions (helpers)
│   ├── logger/           # Logging setup
│   │   ├── logger.go     # Configures logging
│── api/                  # HTTP handlers
│   ├── handlers.go       # Task submission & status API
│   ├── routes.go         # API route definitions
│── scripts/              # Scripts for setup, DB migrations
│── .env                  # Environment variables (Redis/MongoDB configs)
│── docker-compose.yml    # Docker setup for Redis & MongoDB
│── go.mod                # Go module dependencies
│── README.md             # Project documentation
```
