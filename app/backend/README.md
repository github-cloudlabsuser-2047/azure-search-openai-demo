# Backend README

This README provides documentation and details for the backend folder in the Azure Search OpenAI Demo repository.

## Overview

The backend folder contains the server-side code and configurations for the Azure Search OpenAI Demo application. It is responsible for handling API requests, processing data, and interacting with the database.

## Installation

To set up the backend, follow these steps:

1. Clone the Azure Search OpenAI Demo repository.
2. Navigate to the `backend` folder.
3. Install the required dependencies by running `npm install`.
4. Configure the environment variables by creating a `.env` file and providing the necessary values. Refer to the `.env.example` file for the required variables.
5. Start the backend server by running `npm start`.

## Usage

The backend provides the following API endpoints:

- `/api/search`: Handles search requests and returns relevant results.
- `/api/documents`: Handles CRUD operations for documents in the database.
- `/api/feedback`: Handles user feedback submissions.

Refer to the API documentation for detailed information on each endpoint.

## Contributing

If you would like to contribute to the backend development, please follow these guidelines:

1. Fork the Azure Search OpenAI Demo repository.
2. Create a new branch for your feature or bug fix.
3. Make your changes and ensure that the code passes all tests.
4. Submit a pull request with a clear description of your changes.

## License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for more information.
