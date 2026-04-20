# CivicFusion

CivicFusion is a centralized, full-stack platform designed to bridge the gap between citizens and government officials. It facilitates transparent governance by allowing users to report civic issues, track project progress, and manage budget allocations in real time.

## Tech Stack

The project utilizes a scalable architecture to handle real-time community engagement and data processing.

### Frontend

- Framework: React.js
- Styling/UI: Modern CSS features (optimized for responsive design and clear UI/UX)
- Routing: React Router

### Backend

- Runtime & Framework: Node.js with Express.js
- Database: MongoDB (NoSQL) with Mongoose ODM
- Authentication: JSON Web Tokens (JWT) for secure, role-based access control
- API Testing & Seeding: Postman

## Key Functionalities

### 1. User Authentication & Role Management

- Multi-Role Access: Dedicated workflows and permissions tailored for Citizens and Officials.
- Secure Authorization: JWT-based login to protect user data and restrict administrative actions.
- Profile Management: Seamless credential updates, requiring "new password" and "confirm password" validation for secure account modifications.

### 2. Civic Issue Tracking

- Issue Reporting: Citizens can submit detailed reports regarding local civic problems.
- Real-time Interaction: Integrated commenting system fostering direct communication and discussion between officials and citizens.
- Status Updates: Officials can respond to reported issues and dynamically update their resolution progress.

### 3. Project & Budget Management

- Project Initiation: Officials have the authority to create and document new civic infrastructure projects.
- Budget Allocation: Transparent financial tracking for resources assigned to specific community goals.
- Progress Monitoring: Centralized tracking to view the current developmental stage of active projects.

### 4. Data Seeding & API Architecture

- Automated Seeding: Optimized for Postman collection imports, allowing developers to set up environments, run authentication, and seed the MongoDB database in a specific execution order.
- RESTful Endpoints: Robust backend routing to handle complex operations like project creation and nested comment threading.

## Repository Structure

```text
Civic-Fusion/
├── backend/
│   ├── controllers/    # API request handling logic
│   ├── models/         # MongoDB schemas (User, Project, Issue, Comment)
│   ├── routes/         # Express endpoint definitions
│   ├── middleware/     # Auth and validation logic
│   ├── config/         # Database and environment configurations
│   └── server.js       # Entry point for the backend server
└── frontend/
    ├── public/         # Static assets
    ├── src/
    │   ├── components/ # Reusable UI elements
    │   ├── pages/      # Main application views
    │   ├── services/   # API integration logic
    │   └── App.js      # Main React component
    └── package.json
```

## Local Setup Instructions

To run CivicFusion locally, you will need to start both the backend server and the frontend development server.

### Prerequisites

- Node.js installed
- MongoDB running locally or a MongoDB Atlas URI
- Postman (optional, for backend API testing and seeding)

### 1. Clone the repository

```bash
git clone https://github.com/rajbaibhav/Civic-Fusion.git
cd Civic-Fusion
```

### 2. Backend Setup

Open a new terminal and navigate to the backend directory:

```bash
cd backend
```

Install backend dependencies:

```bash
npm install
```

Create a `.env` file in the `backend/` directory and configure the following variables:

```env
PORT=5000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret_key
```

Start the backend server:

```bash
npm start
# The backend will typically run on http://localhost:5000
```

Note for testing: You can now use Postman to import the project collections, set up your environment variables, and run the authentication/creation requests in sequence to populate the database.

### 3. Frontend Setup

Open a second terminal window and navigate to the frontend directory:

```bash
cd frontend
```

Install frontend dependencies:

```bash
npm install
```

Create a `.env` file in the `frontend/` directory (if needed to specify the API URL):

```env
REACT_APP_API_URL=http://localhost:5000/api
```

Start the React development server:

```bash
npm start
# The frontend will typically run on http://localhost:3000
```

The application is now up and running. Navigate to `http://localhost:3000` in your browser to interact with the CivicFusion platform.
