
Full-Stack Chat Application – Kubernetes Deployment Project

This project involved designing, containerizing, and deploying a full-stack real-time chat application in a Kubernetes environment, demonstrating end-to-end skills in DevOps, container orchestration, and cloud-native application deployment.

The application architecture consisted of three main components: MongoDB as the database, a Node.js/Express backend API, and a JavaScript frontend built with a modern framework (React/Vue). The objective was to set up a scalable and production-ready deployment workflow, ensuring proper networking, persistent storage, and secure routing for both internal and external services.

1. Environment & Namespace Setup

To isolate resources and maintain better organization, a dedicated Kubernetes namespace chat-app was created. All subsequent deployments, services, and ingress rules were applied within this namespace, ensuring logical separation and ease of cleanup.

2. Database Deployment (MongoDB)

MongoDB was deployed as a stateful container using the official mongo:latest image. Environment variables were configured to set up authentication (MONGO_INITDB_ROOT_USERNAME and MONGO_INITDB_ROOT_PASSWORD). A PersistentVolumeClaim (PVC) was created to ensure database data remained intact even if the MongoDB pod was restarted or rescheduled.

A ClusterIP service was created to enable internal communication between MongoDB and the backend API. The default MongoDB port 27017 was exposed internally, keeping the database secure from external access.

3. Backend Deployment (Node.js API)

The backend API was built as a multi-stage Docker image to ensure smaller image size and production readiness. The build stage installed dependencies and copied source code, while the runtime stage ran the application as a non-root user for better security.

This service was exposed on port 5001 via a ClusterIP, enabling the frontend and other internal services to communicate with it. The backend environment variables included the MongoDB connection string and other app-specific configurations.

4. Frontend Deployment

The frontend was designed to interact with the backend API. A production build (npm run build) was generated before creating the Docker image. The build artifacts were served via a lightweight HTTP server inside the container.

The frontend service was exposed on port 80 via a ClusterIP for internal routing.

5. Ingress & Routing

To enable external access, NGINX Ingress Controller was deployed. An Ingress resource (chatapp-ingress) was configured with the domain chat-tws.com, routing:

/ → frontend service (port 80)

/api → backend service (port 5001)

This setup ensured clear separation of traffic and seamless integration between frontend and backend. The local /etc/hosts file was updated to map the domain to the Minikube/cluster IP for testing.

6. Testing & Troubleshooting

During testing, the backend successfully connected to MongoDB. Initial issues like missing frontend/dist/index.html were resolved by ensuring proper frontend build steps before deployment. Logs were checked using kubectl logs, and services were debugged with kubectl describe and kubectl port-forward.

7. Cleanup & Maintenance

For environment resets, all Kubernetes resources in the namespace were deleted using:

kubectl delete all --all -n chat-app

Outcome

This project successfully demonstrated the ability to deploy a full-stack, database-driven application in Kubernetes with persistent storage, internal/external networking, domain routing, and container security best practices.
