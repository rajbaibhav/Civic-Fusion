# Civic Fusion - Complete Postman Testing Guide

## Overview
This guide provides step-by-step instructions to test the entire Civic Fusion API, including sample data for each endpoint. Follow these steps to understand the complete flow from signup to admin operations.

---

## 🔧 Prerequisites

1. **Postman** installed
2. **Backend running** on `http://localhost:5000`
3. **MongoDB** running locally

### Start Backend
```bash
cd backend
npm run dev
```

---

## 📋 Environment Setup

### Create Postman Environment

1. Click **Environments** (left sidebar)
2. Click **+** to create new environment
3. Name it: `Civic Fusion Local`
4. Add these variables:

| Variable | Initial Value | Notes |
|----------|---------------|-------|
| `base_url` | `http://localhost:5000/api` | API base URL |
| `citizen_token` | `` | Will be filled after citizen login |
| `official_token` | `` | Will be filled after official login |
| `admin_token` | `` | Will be filled after admin login |
| `project_id` | `` | Will be filled after project creation |
| `issue_id` | `` | Will be filled after issue creation |
| `comment_id` | `` | Will be filled after comment creation |
| `budget_id` | `` | Will be filled after budget creation |
| `user_id` | `` | Will be filled after signup |
| `official_user_id` | `` | User to promote to official |
| `admin_user_id` | `` | User to promote to admin |

5. Click **Save**

---

## 🚀 Step-by-Step Testing Workflow

### ✅ STEP 1: Create Admin User (First Time Setup)

**Run the seed script to create default admin:**

```bash
npm run seed:admin
```

**Default Admin Credentials:**
- Email: `admin@civicfusion.com`
- Password: `admin123`

---

### ✅ STEP 2: Test Admin Login

**Request 1: Admin Login**

```
Method: POST
URL: {{base_url}}/auth/login
```

**Body (JSON):**
```json
{
  "email": "admin@civicfusion.com",
  "password": "admin123"
}
```

**Post-request Script (Save in Postman):**
```javascript
var jsonData = pm.response.json();
pm.environment.set("admin_token", jsonData.token);
pm.environment.set("admin_user_id", jsonData.user.id);
```

**Expected Response:**
```json
{
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "67...",
    "name": "System Admin",
    "email": "admin@civicfusion.com",
    "role": "admin"
  }
}
```

---

### ✅ STEP 3: Create Test Users

#### 3a. Create Citizen User

**Request 2: Signup - Citizen**

```
Method: POST
URL: {{base_url}}/auth/signup
```

**Body (JSON):**
```json
{
  "name": "John Doe",
  "email": "citizen1@test.com",
  "password": "password123",
  "role": "citizen"
}
```

**Post-request Script:**
```javascript
var jsonData = pm.response.json();
pm.environment.set("citizen_token", jsonData.token);
pm.environment.set("citizen_user_id", jsonData.user.id);
```

**Expected Response:** Token + User info with role `citizen`

---

#### 3b. Create Volunteer User (Future Official)

**Request 3: Signup - Volunteer (To be promoted to Official)**

```
Method: POST
URL: {{base_url}}/auth/signup
```

**Body (JSON):**
```json
{
  "name": "Sarah Smith",
  "email": "official1@test.com",
  "password": "password123",
  "role": "volunteer"
}
```

**Post-request Script:**
```javascript
var jsonData = pm.response.json();
pm.environment.set("official_token", jsonData.token);
pm.environment.set("official_user_id", jsonData.user.id);
```

---

#### 3c. Create Another Volunteer (Future Admin)

**Request 4: Signup - Volunteer (To be promoted to Admin)**

```
Method: POST
URL: {{base_url}}/auth/signup
```

**Body (JSON):**
```json
{
  "name": "Mike Johnson",
  "email": "admin2@test.com",
  "password": "password123",
  "role": "volunteer"
}
```

**Post-request Script:**
```javascript
var jsonData = pm.response.json();
pm.environment.set("admin_user_id", jsonData.token);
pm.environment.set("admin_user_id", jsonData.user.id);
```

---

### ✅ STEP 4: Promote Users to Official & Admin

#### 4a. Promote to Official

**Request 5: Assign Role - Make Official**

```
Method: PUT
URL: {{base_url}}/admin/users/{{official_user_id}}/role
Headers: Authorization: Bearer {{admin_token}}
```

**Body (JSON):**
```json
{
  "role": "official"
}
```

**Expected Response:**
```json
{
  "message": "Role assigned successfully",
  "user": {
    "id": "67...",
    "name": "Sarah Smith",
    "email": "official1@test.com",
    "role": "official"
  }
}
```

---

#### 4b. Promote to Admin

**Request 6: Assign Role - Make Admin**

```
Method: PUT
URL: {{base_url}}/admin/users/{{admin_user_id}}/role
Headers: Authorization: Bearer {{admin_token}}
```

**Body (JSON):**
```json
{
  "role": "admin"
}
```

---

### ✅ STEP 5: Re-login as Official

**Request 7: Login - Official**

```
Method: POST
URL: {{base_url}}/auth/login
```

**Body (JSON):**
```json
{
  "email": "official1@test.com",
  "password": "password123"
}
```

**Post-request Script:**
```javascript
var jsonData = pm.response.json();
pm.environment.set("official_token", jsonData.token);
```

---

## 📁 All API Routes with Sample Data

### 🔐 AUTHENTICATION ROUTES

#### 1. **Signup**
```
POST {{base_url}}/auth/signup
```
| Field | Type | Required | Example |
|-------|------|----------|---------|
| name | String | Yes | "John Doe" |
| email | String | Yes | "john@test.com" |
| password | String (min 6) | Yes | "password123" |
| role | String | No | "citizen" or "volunteer" |

---

#### 2. **Login**
```
POST {{base_url}}/auth/login
```
| Field | Type | Required | Example |
|-------|------|----------|---------|
| email | String | Yes | "john@test.com" |
| password | String | Yes | "password123" |

**Response includes JWT token - save it in Authorization header**

---

#### 3. **Logout**
```
POST {{base_url}}/auth/logout
Headers: Authorization: Bearer {{admin_token}}
```

---

### 👤 USER ROUTES

#### 4. **Get Profile**
```
GET {{base_url}}/users/profile
Headers: Authorization: Bearer {{citizen_token}}
```
No body required. Returns current user's profile.

---

#### 5. **Update Profile**
```
PUT {{base_url}}/users/profile
Headers: Authorization: Bearer {{citizen_token}}
```

**Body (JSON):**
```json
{
  "name": "Jane Doe",
  "email": "newemail@test.com"
}
```

---

#### 6. **Deactivate Account**
```
PUT {{base_url}}/users/deactivate
Headers: Authorization: Bearer {{citizen_token}}
```
No body required. Marks account as inactive.

---

### 🏗️ PROJECT ROUTES

#### 7. **Get All Projects** (Public)
```
GET {{base_url}}/projects
```
Optional query parameters:
- `department=Public Works`
- `status=ongoing`
- `location=City Center`

**Sample Response:**
```json
[
  {
    "_id": "67...",
    "title": "City Bridge Renovation",
    "description": "Renovation of main city bridge",
    "department": "Public Works",
    "location": "Downtown",
    "status": "ongoing",
    "progress": 45,
    "createdBy": {
      "id": "67...",
      "name": "Sarah Smith",
      "role": "official"
    },
    "createdAt": "2026-04-12T10:00:00Z"
  }
]
```

---

#### 8. **Get Single Project** (Public)
```
GET {{base_url}}/projects/{{project_id}}
```

---

#### 9. **Create Project** (Official/Admin Only)
```
POST {{base_url}}/projects
Headers: Authorization: Bearer {{official_token}}
```

**Body (JSON):**
```json
{
  "title": "City Bridge Renovation",
  "description": "Complete renovation and structural reinforcement of the main city bridge to ensure public safety and improve infrastructure. Phase includes engineering assessment, planning, and construction.",
  "department": "Public Works",
  "location": "Downtown - Main Street Bridge"
}
```

**Post-request Script:**
```javascript
var jsonData = pm.response.json();
pm.environment.set("project_id", jsonData.project._id);
```

---

#### 10. **Update Project** (Official/Admin Only)
```
PUT {{base_url}}/projects/{{project_id}}
Headers: Authorization: Bearer {{official_token}}
```

**Body (JSON):**
```json
{
  "title": "City Bridge Renovation - Phase 2",
  "description": "Updated description with phase 2 details",
  "department": "Public Works",
  "location": "Downtown"
}
```

---

#### 11. **Update Project Status** (Official/Admin Only)
```
PUT {{base_url}}/projects/{{project_id}}/status
Headers: Authorization: Bearer {{official_token}}
```

**Body (JSON):**
```json
{
  "status": "ongoing"
}
```

**Valid statuses:** `planned`, `ongoing`, `completed`, `stalled`

---

#### 12. **Update Project Progress** (Official/Admin Only)
```
PUT {{base_url}}/projects/{{project_id}}/progress
Headers: Authorization: Bearer {{official_token}}
```

**Body (JSON):**
```json
{
  "progress": 45,
  "notes": "Foundation work 45% complete, starting structural work next week"
}
```

**Note:** Progress must be 0-100

---

#### 13. **Archive Project** (Official/Admin Only)
```
PUT {{base_url}}/projects/{{project_id}}/archive
Headers: Authorization: Bearer {{official_token}}
```

No body required. Hides project from public listing.

---

### 💰 BUDGET ROUTES

#### 14. **Get Budget by Project** (Public)
```
GET {{base_url}}/budgets/project/{{project_id}}
```

**Sample Response:**
```json
{
  "_id": "67...",
  "project": {
    "_id": "67...",
    "title": "City Bridge Renovation"
  },
  "allocatedAmount": 5000000,
  "spentAmount": 2250000,
  "history": [
    {
      "_id": "67...",
      "allocatedAmount": 5000000,
      "spentAmount": 0,
      "updatedBy": {"name": "Sarah Smith"},
      "notes": "Initial budget allocation",
      "createdAt": "2026-04-12T10:00:00Z"
    }
  ]
}
```

---

#### 15. **Get Budget History** (Public)
```
GET {{base_url}}/budgets/project/{{project_id}}/history
```

Returns array of all budget modifications with timestamps.

---

#### 16. **Add Budget to Project** (Official/Admin Only)
```
POST {{base_url}}/budgets/project/{{project_id}}
Headers: Authorization: Bearer {{official_token}}
```

**Body (JSON):**
```json
{
  "allocatedAmount": 5000000
}
```

**Note:** Only one budget per project. Use update to modify.

---

#### 17. **Update Budget** (Official/Admin Only)
```
PUT {{base_url}}/budgets/project/{{project_id}}
Headers: Authorization: Bearer {{official_token}}
```

**Body (JSON):**
```json
{
  "allocatedAmount": 5500000,
  "spentAmount": 2500000,
  "notes": "Increased allocation for material costs, spent amount updated"
}
```

---

### 💬 COMMENT ROUTES

#### 18. **Get Comments on Project** (Public)
```
GET {{base_url}}/comments/project/{{project_id}}
```

**Sample Response:**
```json
[
  {
    "_id": "67...",
    "project": {"_id": "67...", "title": "City Bridge Renovation"},
    "user": {
      "_id": "67...",
      "name": "John Doe",
      "role": "citizen"
    },
    "content": "Great initiative! Looking forward to see the bridge completed.",
    "isDeleted": false,
    "createdAt": "2026-04-12T11:30:00Z"
  }
]
```

---

#### 19. **Add Comment** (Authenticated Users)
```
POST {{base_url}}/comments/project/{{project_id}}
Headers: Authorization: Bearer {{citizen_token}}
```

**Body (JSON):**
```json
{
  "content": "This project is important for community safety. Please prioritize it!"
}
```

**Post-request Script:**
```javascript
var jsonData = pm.response.json();
pm.environment.set("comment_id", jsonData.comment._id);
```

---

#### 20. **Update Comment** (Own Comments Only)
```
PUT {{base_url}}/comments/{{comment_id}}
Headers: Authorization: Bearer {{citizen_token}}
```

**Body (JSON):**
```json
{
  "content": "Updated comment - This project is critical for community safety and infrastructure improvement!"
}
```

---

#### 21. **Delete Comment** (Own Comments or Admin)
```
DELETE {{base_url}}/comments/{{comment_id}}
Headers: Authorization: Bearer {{citizen_token}}
```

No body required.

---

### 📋 ISSUE ROUTES

#### 22. **Get All Issues** (Public)
```
GET {{base_url}}/issues
```

Optional query parameters:
- `project={{project_id}}`
- `status=open` (open, in_progress, resolved)
- `category=Delay`

---

#### 23. **Get Single Issue** (Public)
```
GET {{base_url}}/issues/{{issue_id}}
```

---

#### 24. **Create Issue** (Authenticated Users)
```
POST {{base_url}}/issues/project/{{project_id}}
Headers: Authorization: Bearer {{citizen_token}}
```

**Body (JSON):**
```json
{
  "title": "Delayed Schedule - Week 3 Behind",
  "description": "The project timeline shows the current phase is running 1 week behind schedule. Weather delays and material procurement issues are the main causes. Concerned about overall project deadline impact.",
  "category": "Timeline Delay",
  "priority": "high"
}
```

**Post-request Script:**
```javascript
var jsonData = pm.response.json();
pm.environment.set("issue_id", jsonData.issue._id);
```

**Valid priorities:** `low`, `medium`, `high` (default: medium)

---

#### 25. **Update Issue Status** (Official/Admin Only)
```
PUT {{base_url}}/issues/{{issue_id}}/status
Headers: Authorization: Bearer {{official_token}}
```

**Body (JSON):**
```json
{
  "status": "in_progress"
}
```

**Valid statuses:** `open`, `in_progress`, `resolved`

---

#### 26. **Respond to Issue** (Official/Admin Only)
```
POST {{base_url}}/issues/{{issue_id}}/respond
Headers: Authorization: Bearer {{official_token}}
```

**Body (JSON):**
```json
{
  "response": "We acknowledge the delay. The weather impact has been factored in. We've brought additional workers and materials. Expected to recover 3 days by next week. Will provide weekly updates."
}
```

---

#### 27. **Escalate Issue** (Authenticated Users)
```
POST {{base_url}}/issues/{{issue_id}}/escalate
Headers: Authorization: Bearer {{citizen_token}}
```

No body required. Marks issue as escalated.

---

### 👥 ADMIN ROUTES

### ⚠️ All admin routes require:
```
Headers: Authorization: Bearer {{admin_token}}
```

---

#### 28. **Get All Users**
```
GET {{base_url}}/admin/users
```

**Query parameters:**
- No parameters currently

**Sample Response:**
```json
[
  {
    "_id": "67...",
    "name": "System Admin",
    "email": "admin@civicfusion.com",
    "role": "admin",
    "isActive": true,
    "isBlocked": false,
    "createdAt": "2026-04-12T09:00:00Z"
  },
  {
    "_id": "67...",
    "name": "John Doe",
    "email": "citizen1@test.com",
    "role": "citizen",
    "isActive": true,
    "isBlocked": false
  }
]
```

---

#### 29. **Get Single User Details**
```
GET {{base_url}}/admin/users/{{user_id}}
```

---

#### 30. **Assign Role to User**
```
PUT {{base_url}}/admin/users/{{official_user_id}}/role
```

**Body (JSON):**
```json
{
  "role": "official"
}
```

**Valid roles:** `citizen`, `volunteer`, `official`, `admin`

---

#### 31. **Block User**
```
PUT {{base_url}}/admin/users/{{user_id}}/block
```

No body required. Prevents user login.

---

#### 32. **Unblock User**
```
PUT {{base_url}}/admin/users/{{user_id}}/unblock
```

No body required.

---

#### 33. **Deactivate User**
```
PUT {{base_url}}/admin/users/{{user_id}}/deactivate
```

No body required. Prevents login.

---

#### 34. **Reactivate User**
```
PUT {{base_url}}/admin/users/{{user_id}}/reactivate
```

No body required.

---

#### 35. **Delete Comment (Admin)**
```
DELETE {{base_url}}/admin/comments/{{comment_id}}
```

No body required. Only admins can delete any comment.

---

#### 36. **Get Platform Statistics**
```
GET {{base_url}}/admin/stats
```

**Sample Response:**
```json
{
  "totalUsers": 4,
  "usersByRole": {
    "citizen": 1,
    "volunteer": 1,
    "official": 1,
    "admin": 2
  },
  "totalProjects": 1,
  "projectsByStatus": {
    "planned": 0,
    "ongoing": 1,
    "completed": 0,
    "stalled": 0
  },
  "totalIssues": 2,
  "issuesByStatus": {
    "open": 1,
    "in_progress": 1,
    "resolved": 0
  },
  "totalComments": 3,
  "totalBudgets": 1
}
```

---

## 📝 Complete Testing Checklist

### Phase 1: Authentication & User Management
- [ ] Create admin (seed script)
- [ ] Login as admin
- [ ] Create citizen user
- [ ] Create volunteer #1 (future official)
- [ ] Create volunteer #2 (future admin - optional)
- [ ] Promote volunteer #1 to official
- [ ] Re-login as official
- [ ] View all users (admin)
- [ ] Update user profile (citizen)
- [ ] Get user profile

### Phase 2: Project Management
- [ ] Create project (official)
- [ ] Get all projects (public)
- [ ] Get single project (public)
- [ ] Update project details (official)
- [ ] Update project status: planned → ongoing (official)
- [ ] Update project progress to 25% (official)
- [ ] Archive project (official)

### Phase 3: Budget Management
- [ ] Add budget to project (official)
- [ ] Get budget by project (public)
- [ ] Get budget history (public)
- [ ] Update budget: increase amount & spent (official)

### Phase 4: Comments
- [ ] Add comment to project (citizen)
- [ ] Get all comments on project (public)
- [ ] Update own comment (citizen)
- [ ] Delete own comment (citizen)

### Phase 5: Issues
- [ ] Create issue (citizen)
- [ ] Get all issues (public)
- [ ] Get single issue (public)
- [ ] Update issue status: open → in_progress (official)
- [ ] Respond to issue (official)
- [ ] Escalate issue (citizen)

### Phase 6: Admin Management
- [ ] Get all users (admin)
- [ ] Block user (admin)
- [ ] Unblock user (admin)
- [ ] Delete comment (admin)
- [ ] Get platform statistics (admin)

---

## 🔑 Authorization Header Format

For all protected endpoints, include:

```
Authorization: Bearer {{token}}
```

In Postman:
1. Go to **Headers** tab
2. Add header: `Authorization`
3. Value: `Bearer {{token_variable}}`
4. Postman will auto-replace with actual token

---

## 🐛 Common Issues & Solutions

### "Invalid token" error
- ❌ Token expired or not valid
- ✅ Re-login and get fresh token
- ✅ Check token variable has value in environment

### "User already exists" error
- ❌ Email already registered
- ✅ Use different email for each test user
- ✅ Add timestamp: `test+{{$timestamp}}@example.com`

### "Role not authorized" error
- ❌ User role doesn't have permission
- ✅ Verify user role (citizen, official, admin)
- ✅ Promote user to correct role via admin endpoint

### "Project not found" error
- ❌ Project ID not set or invalid
- ✅ Create project first
- ✅ Check `{{project_id}}` has value

### "Validation failed" error
- ❌ Missing required fields or invalid format
- ✅ Check all required fields are present
- ✅ Verify data types (string, number, etc.)

---

## 💡 Pro Tips

1. **Use Environment Variables**: Saves time copying IDs around
2. **Post-request Scripts**: Automatically extract IDs from responses
3. **Collection Runner**: Run multiple requests in sequence
4. **Tests Tab**: Add assertions to validate responses
5. **Save Responses**: Click response → save as example
6. **Timestamp IDs**: Use `{{$timestamp}}` for unique values

---

## Sample Complete Flow in Sequence

```
1. POST /auth/signup → Create citizen → Get citizen_token
2. POST /auth/signup → Create volunteer → Get user_id for promotion
3. PUT /admin/users/{id}/role → Promote to official
4. POST /auth/login → Login as official → Get official_token
5. POST /projects → Create project → Get project_id
6. POST /budgets/project/{id} → Add budget
7. POST /comments/project/{id} → Add comment as citizen
8. POST /issues/project/{id} → Create issue as citizen → Get issue_id
9. PUT /issues/{id}/status → Update status as official
10. POST /issues/{id}/respond → Official response
11. GET /admin/stats → View platform stats as admin
```

---

## 📚 Additional Resources

- API Documentation: See [ARCHITECTURE.md](ARCHITECTURE.md)
- Database Schema: See models in `backend/models/`
- Error Handling: See controllers in `backend/controllers/`

---

**Last Updated:** April 12, 2026  
**Version:** 1.0 - Complete Guide

