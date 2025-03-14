# React Flow Microservice

## Overview

This project is designed as a microservice that integrates with a Vue.js application. It processes data provided by the Vue app, creates interactive workflows using React Flow, and generates dashboards based on the outputs of various nodes. The microservice exposes APIs that trigger backend jobs and create visualizations, which are then rendered within the Vue project.

## Features

- **React Flow Integration**: Create and manage interactive workflows with drag-and-drop nodes.
- **API Endpoints**: RESTful API for data processing, workflow management, and dashboard generation.
- **Vue.js Compatibility**: Designed to be embedded and rendered within a Vue.js application.
- **Microservice Architecture**: Modular, scalable, and independently deployable.

## Architecture

This microservice follows these architectural principles:

- **Single Responsibility**: Focuses solely on workflow creation and dashboard generation.
- **API-First Design**: All functionality is accessible via well-defined API endpoints.
- **Stateless Operation**: Designed to be horizontally scalable.
- **Containerization**: Packaged as a Docker container for easy deployment.

## Setup Instructions

1. **Clone the Repository**

   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. **Install Dependencies**

   Ensure you have Bun installed. Then run:

   ```bash
   bun install
   ```

3. **Configure Environment Variables**

   Create a `.env` file in the root directory and add necessary environment variables:

   ```plaintext
   PORT=3000
   API_KEY=your_api_key
   BACKEND_URL=http://your-backend-url
   CORS_ORIGIN=http://your-vue-app-url
   ```

4. **Run the Development Server**

   Start the development server with:

   ```bash
   bun dev
   ```

5. **Build for Production**

   To build the project for production, use:

   ```bash
   bun build
   ```

6. **Containerize the Service**

   Build and run the Docker container:

   ```bash
   docker build -t react-flow-microservice .
   docker run -p 3000:3000 react-flow-microservice
   ```

## API Endpoints

### Workflow Management

- `GET /api/workflows`: Retrieve all workflows
- `GET /api/workflows/:id`: Retrieve a specific workflow
- `POST /api/workflows`: Create a new workflow
- `PUT /api/workflows/:id`: Update an existing workflow
- `DELETE /api/workflows/:id`: Delete a workflow

### Node Operations

- `POST /api/workflows/:id/nodes`: Add a node to a workflow
- `PUT /api/workflows/:id/nodes/:nodeId`: Update a node
- `DELETE /api/workflows/:id/nodes/:nodeId`: Remove a node

### Dashboard Generation

- `GET /api/dashboards/:workflowId`: Generate a dashboard from a workflow
- `POST /api/execute/:workflowId`: Execute a workflow and return results

## Integration with Vue.js

To integrate this microservice with your Vue.js application:

1. **Embed the React Flow Component**:

   ```javascript
   // In your Vue component
   <template>
     <div ref="reactFlowContainer" style="width: 100%; height: 600px;"></div>
   </template>

   <script>
   import { onMounted, onBeforeUnmount } from 'vue';
   import { createRoot } from 'react-dom/client';
   import { ReactFlowComponent } from 'react-flow-microservice';

   export default {
     setup() {
       let root = null;
       
       onMounted(() => {
         const container = document.getElementById('reactFlowContainer');
         root = createRoot(container);
         root.render(<ReactFlowComponent />);
       });
       
       onBeforeUnmount(() => {
         if (root) {
           root.unmount();
         }
       });
       
       return {};
     }
   }
   </script>
   ```

2. **Make API Calls**:

   ```javascript
   // Example of calling the microservice API
   async function fetchWorkflows() {
     const response = await fetch('http://microservice-url:3000/api/workflows');
     const data = await response.json();
     return data;
   }
   ```

## Docker Deployment

This microservice includes a Dockerfile for containerization:

```dockerfile
FROM jarredsumner/bun:latest

WORKDIR /app

COPY package.json bun.lockb ./
RUN bun install

COPY . .

EXPOSE 3000

CMD ["bun", "dev"]
```

## Future Plans

- Enhance the dashboard with more visualization options
- Implement real-time updates using WebSockets
- Add user authentication and authorization
- Improve API performance and scalability
- Implement service discovery for microservice orchestration

## Contributing

If you wish to contribute, please fork the repository and submit a pull request.

## License

This project is licensed under the MIT License.
