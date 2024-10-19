# ChatMetric: LLM Analytics Middleware


ChatMetric is a powerful LLM (Large Language Model) analytics solution that acts as middleware, enabling the collection and analysis of valuable data from LLM interactions. This repository contains the documentation for the ChatMetric API, allowing developers to integrate robust analytics into their LLM-powered applications.

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [API Documentation](#api-documentation)
  - [Authentication](#authentication)
  - [Endpoints](#endpoints)
    - [Initialize Session](#initialize-session)
    - [Log Interaction](#log-interaction)
    - [Retrieve Analytics](#retrieve-analytics)
    - [Generate Report](#generate-report)
    - [Update Configuration](#update-configuration)
- [Examples](#examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Introduction

ChatMetric seamlessly integrates with your existing LLM infrastructure, providing valuable insights into user interactions, model performance, and usage patterns. By leveraging ChatMetric, you can optimize your LLM applications, improve user experience, and make data-driven decisions.

## Features

- Real-time analytics collection
- Customizable metrics and dimensions
- Performance monitoring and anomaly detection
- User behavior analysis
- Integration with popular LLM platforms
- Scalable architecture for high-volume applications
- Comprehensive reporting and visualization tools

## Getting Started

### Prerequisites

- Node.js (v14.0.0 or higher)
- npm (v6.0.0 or higher)
- An active ChatMetric account (Sign up at [https://chatmetric.net](https://chatmetric.net))

### Installation

1. Install the ChatMetric SDK:

```bash
npm install chatmetric-sdk
```

2. Initialize the SDK in your project:

```javascript
const ChatMetric = require('chatmetric-sdk');
const chatMetric = new ChatMetric('YOUR_API_KEY');
```

## API Documentation

### Authentication

All API requests must include your ChatMetric API key in the `Authorization` header:

```
Authorization: Bearer YOUR_API_KEY
```

### Endpoints

#### Initialize Session

```
POST /api/v1/sessions
```

Initialize a new analytics session for a user interaction.

**Request Body:**

```json
{
  "user_id": "string",
  "model_id": "string",
  "session_metadata": {
    "key1": "value1",
    "key2": "value2"
  }
}
```

**Response:**

```json
{
  "session_id": "string",
  "created_at": "ISO8601 timestamp"
}
```

#### Log Interaction

```
POST /api/v1/interactions
```

Log a single interaction between the user and the LLM.

**Request Body:**

```json
{
  "session_id": "string",
  "type": "user_input" | "model_response",
  "content": "string",
  "timestamp": "ISO8601 timestamp",
  "metadata": {
    "key1": "value1",
    "key2": "value2"
  }
}
```

**Response:**

```json
{
  "interaction_id": "string",
  "recorded_at": "ISO8601 timestamp"
}
```

#### Retrieve Analytics

```
GET /api/v1/analytics
```

Retrieve analytics data for a specific time range and set of metrics.

**Query Parameters:**

- `start_date` (required): ISO8601 timestamp
- `end_date` (required): ISO8601 timestamp
- `metrics` (required): Comma-separated list of metric names
- `dimensions` (optional): Comma-separated list of dimension names
- `filters` (optional): JSON-encoded filter object

**Response:**

```json
{
  "data": [
    {
      "timestamp": "ISO8601 timestamp",
      "metric1": 123,
      "metric2": 456,
      "dimension1": "value1",
      "dimension2": "value2"
    },
    // ... more data points
  ],
  "metadata": {
    "total_count": 1000,
    "aggregations": {
      "metric1": {
        "sum": 12345,
        "avg": 123.45,
        "min": 10,
        "max": 500
      },
      // ... more metric aggregations
    }
  }
}
```

#### Generate Report

```
POST /api/v1/reports
```

Generate a comprehensive analytics report.

**Request Body:**

```json
{
  "report_type": "daily" | "weekly" | "monthly" | "custom",
  "start_date": "ISO8601 timestamp",
  "end_date": "ISO8601 timestamp",
  "metrics": ["metric1", "metric2"],
  "dimensions": ["dimension1", "dimension2"],
  "filters": {
    "model_id": ["model1", "model2"],
    "user_id": ["user1", "user2"]
  },
  "format": "pdf" | "csv" | "json"
}
```

**Response:**

```json
{
  "report_id": "string",
  "status": "processing" | "completed" | "failed",
  "download_url": "string",
  "created_at": "ISO8601 timestamp"
}
```

#### Update Configuration

```
PATCH /api/v1/config
```

Update the ChatMetric configuration for your account.

**Request Body:**

```json
{
  "sampling_rate": 0.1,
  "custom_metrics": [
    {
      "name": "response_quality",
      "type": "float",
      "aggregation": "avg"
    }
  ],
  "alerts": [
    {
      "metric": "error_rate",
      "threshold": 0.05,
      "condition": "greater_than",
      "notification_channel": "email"
    }
  ]
}
```

**Response:**

```json
{
  "status": "success",
  "updated_at": "ISO8601 timestamp"
}
```

## Examples

Here's a simple example of how to use the ChatMetric SDK to log an interaction:

```javascript
const ChatMetric = require('chatmetric-sdk');
const chatMetric = new ChatMetric('YOUR_API_KEY');

async function logConversation(userId, modelId, userInput, modelResponse) {
  try {
    // Initialize session
    const session = await chatMetric.initializeSession(userId, modelId);

    // Log user input
    await chatMetric.logInteraction(session.session_id, 'user_input', userInput);

    // Log model response
    await chatMetric.logInteraction(session.session_id, 'model_response', modelResponse);

    console.log('Interaction logged successfully');
  } catch (error) {
    console.error('Error logging interaction:', error);
  }
}

// Usage
logConversation('user123', 'gpt-3.5-turbo', 'What is the capital of France?', 'The capital of France is Paris.');
```

## Best Practices

1. Initialize a new session for each unique conversation or user interaction.
2. Log both user inputs and model responses to get a complete picture of the interaction.
3. Use custom metadata to enrich your analytics data with application-specific information.
4. Implement error handling and retry mechanisms for API calls to ensure data integrity.
5. Regularly review your analytics data and generate reports to identify trends and areas for improvement.

## Troubleshooting

If you encounter any issues while using ChatMetric, please check the following:

1. Ensure your API key is valid and has the necessary permissions.
2. Check your network connection and firewall settings.
3. Verify that you're using the latest version of the ChatMetric SDK.
4. Review the API documentation for any recent changes or updates.

If you still need assistance, please contact our support team at support@chatmetric.ai or open an issue in this repository.

## Contributing

We welcome contributions to improve ChatMetric! Please follow these steps to contribute:

1. Fork the repository
2. Create a new branch for your feature or bug fix
3. Make your changes and commit them with descriptive commit messages
4. Push your changes to your fork
5. Submit a pull request to the main repository

Please ensure that your code follows our coding standards and includes appropriate tests.

## License

ChatMetric is released under the MIT License. See the [LICENSE](LICENSE) file for details.

---

For more information, visit our website at [https://chatmetric.net](https://chatmetric.net) or contact our sales team at info@chatmetric.net.