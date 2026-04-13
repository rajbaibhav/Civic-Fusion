# Civic Fusion - Sample Data Scenarios & Test Cases

## 🎭 Different User Scenarios

### Scenario 1: Basic Citizen Testing
**Goal:** Test citizen signup → profile → comments

```json
{
  "name": "Rajesh Kumar",
  "email": "rajesh.kumar@test.com",
  "password": "SecurePass123",
  "role": "citizen"
}
```

**Can Do:**
- ✅ View projects (public)
- ✅ View issues (public)
- ✅ View comments (public)
- ✅ Create comments on projects
- ✅ Create issues
- ✅ Escalate issues
- ❌ Cannot create projects (needs official role)
- ❌ Cannot manage budgets (needs official role)

---

### Scenario 2: Official Tester (Government Employee)
**Goal:** Test project creation → budget → status updates

```json
{
  "name": "Priya Sharma",
  "email": "priya.official@test.com",
  "password": "OfficialPass123",
  "role": "volunteer"
}
```

**After Promotion to Official:**
```json
{
  "role": "official"
}
```

**Can Do:**
- ✅ Create projects
- ✅ Update project status & progress
- ✅ Manage budgets
- ✅ Respond to citizen issues
- ✅ View all data
- ✅ All citizen permissions
- ❌ Cannot manage users (needs admin role)
- ❌ Cannot assign roles

---

### Scenario 3: Admin Testing
**Goal:** Test user management → platform statistics → moderation

**Default Admin:**
```
Email: admin@civicfusion.com
Password: admin123
```

**Can Do:**
- ✅ Manage all users (create, block, promote)
- ✅ View platform statistics
- ✅ Delete any comment (moderation)
- ✅ Assign roles to users
- ✅ Deactivate/reactivate accounts
- ✅ All official permissions
- ✅ All citizen permissions

---

## 📋 Project Testing Scenarios

### Scenario 1: Small Infrastructure Project
```json
{
  "title": "Local Park Renovation",
  "description": "Renovation of community park including new playground equipment, benches, and landscaping. Expected to improve neighborhood quality of life.",
  "department": "Parks & Recreation",
  "location": "Smith Park, North District"
}
```

**Status Flow:**
```
Creation → Progress Updates
planned → ongoing (25%) → ongoing (50%) → ongoing (75%) → completed (100%)
```

---

### Scenario 2: Major Infrastructure Project
```json
{
  "title": "Metro Rail Extension - Phase 3",
  "description": "Extension of metro rail to suburban areas covering 15 km distance. Phase 3 includes tunnel boring, station construction, and electrical installations. Critical project for city transportation.",
  "department": "Public Transportation",
  "location": "North-East Corridor, Suburbs"
}
```

**Complex Status Flow:**
```
planned → ongoing (30%) → ongoing (32%) → STALLED (30%)
→ ongoing (35%) → ongoing (50%) → completed (100%)
```

---

### Scenario 3: Health Project
```json
{
  "title": "Primary Health Center Upgrade",
  "description": "Upgrade 50 primary health centers across rural areas with modern equipment, staff training, and digital records system.",
  "department": "Health",
  "location": "Rural Districts"
}
```

---

## 💰 Budget Testing Scenarios

### Scenario 1: Small Budget
```json
{
  "allocatedAmount": 500000,
  "spentAmount": 250000,
  "notes": "First quarter spending completed"
}
```

**Budget History:**
```
Initial: 500,000 (allocated) → 0 (spent)
↓
Update 1: 500,000 (allocated) → 150,000 (spent) - Materials
↓ 
Update 2: 500,000 (allocated) → 250,000 (spent) - Labor + Materials
```

---

### Scenario 2: Large Budget with Overrun
```json
{
  "allocatedAmount": 50000000,
  "spentAmount": 35000000,
  "notes": "Increased budget due to unforeseen subsurface issues"
}
```

**Expected History Entries:**
```
Entry 1: Allocated 50M, Spent 0
Entry 2: Allocated 50M, Spent 10M - First phase
Entry 3: Allocated 50M, Spent 25M - Second phase
Entry 4: Allocated 50M, Spent 35M - Extra work discovered
Final: Allocated 60M, Spent 35M - Budget increased
```

---

## 💬 Comment Testing Scenarios

### Positive Feedback
```json
{
  "content": "Excellent initiative! The city really needs this infrastructure improvement. Great to see progress!"
}
```

### Concern/Question
```json
{
  "content": "I'm concerned about the noise and dust during construction. Will there be measures to minimize impact on nearby residents? Please provide details."
}
```

### Suggestion
```json
{
  "content": "Suggestion: Consider using eco-friendly materials for the project. This would make it sustainable and set example for future projects."
}
```

### Update/Discussion
```json
{
  "content": "UPDATE: According to latest news, the project completion date has been postponed by 2 months due to weather delays."
}
```

### Comment Thread
```
Original: "When will the project start?"
Reply: "Project starts March 15, 2026"
Reply: "Great! Looking forward to improvements"
```

---

## 📋 Issue Testing Scenarios

### Scenario 1: High Priority Delay Issue
```json
{
  "title": "Critical Delay: 3 Weeks Behind Schedule",
  "description": "The current project phase is running 3 weeks behind the planned schedule. This is due to: 1) Unforeseen geological conditions at construction site 2) Delayed material delivery from suppliers 3) Bad weather conditions. This delay may impact the overall project deadline.",
  "category": "Schedule Delay",
  "priority": "high"
}
```

**Expected Flow:**
```
Created by: Citizen
Status: open
↓ (Official reviews)
Status: in_progress
Response: "We've identified the issues and brought extra crew..."
```

---

### Scenario 2: Safety Concern Issue
```json
{
  "title": "Safety Hazard: Inadequate Fencing",
  "description": "The construction site near school lacks proper safety fencing. Children can potentially access the area. This is a serious safety concern that needs immediate attention. Parents in area are worried.",
  "category": "Safety",
  "priority": "high"
}
```

---

### Scenario 3: Maintenance Issue
```json
{
  "title": "Poor Maintenance: Potholes Increasing",
  "description": "The temporary road arrangement has created more potholes. Traffic congestion increased by 40% and driving is now unsafe. Need better maintenance or permanent solution.",
  "category": "Road Maintenance",
  "priority": "medium"
}
```

---

### Scenario 4: Budget Issue
```json
{
  "title": "Budget Overrun Concern",
  "description": "According to available documents, project is already 20% over budget in first quarter. At this rate, final cost will exceed allocated funds by 50%. Need explanation for cost overruns.",
  "category": "Budget",
  "priority": "high"
}
```

---

### Scenario 5: Communication Issue
```json
{
  "title": "Lack of Transparency: No Progress Updates",
  "description": "It's been 2 months since last official update about project. Citizens are unaware of current progress, challenges, and completion timeline. Need regular communication.",
  "category": "Communication",
  "priority": "medium"
}
```

---

## 🔄 Complete Workflow Example

### Step-by-Step: Park Renovation Project

#### 1. Official Creates Project
```
POST /api/projects
{
  "title": "Central Park Renovation 2026",
  "description": "Complete renovation including playground, walking trails, and community area",
  "department": "Parks & Recreation",
  "location": "Central Park, Downtown"
}
```

#### 2. Official Updates Status
```
PUT /api/projects/{id}/status
{
  "status": "ongoing"
}
```

#### 3. Official Adds Budget
```
POST /api/budgets/project/{id}
{
  "allocatedAmount": 2000000
}
```

#### 4. Citizen Views & Comments
```
GET /api/projects/{id}
GET /api/comments/project/{id}
POST /api/comments/project/{id}
{
  "content": "Looking forward to the park improvements!"
}
```

#### 5. Citizen Raises Issue
```
POST /api/issues/project/{id}
{
  "title": "Playground Equipment Safety",
  "description": "Need to ensure all equipment meets current safety standards",
  "category": "Safety",
  "priority": "high"
}
```

#### 6. Official Responds
```
POST /api/issues/{issueId}/respond
{
  "response": "Safety inspection scheduled for next week. All equipment will be certified before opening."
}
```

#### 7. Official Updates Progress
```
PUT /api/projects/{id}/progress
{
  "progress": 35,
  "notes": "Site preparation 100% done, material storage set up, equipment delivery completed"
}
```

#### 8. Official Updates Budget Spending
```
PUT /api/budgets/project/{id}
{
  "allocatedAmount": 2000000,
  "spentAmount": 700000,
  "notes": "First phase infrastructure spending completed"
}
```

---

## 🧪 Edge Cases & Error Scenarios

### Scenario 1: Duplicate Email Registration
**First Signup:**
```json
{
  "email": "test@example.com",
  "password": "password123"
}
```

**Second Signup (Same Email):**
```
Response: 400
{
  "error": "User already exists with this email"
}
```

---

### Scenario 2: Invalid Data
**Invalid Email:**
```json
{
  "email": "invalid-email",
  "password": "password123"
}
```

**Response:**
```
400 - Valid email is required
```

---

### Scenario 3: Budget Already Exists
**First Budget Creation:**
```
201 - Success
```

**Second Budget (Same Project):**
```
400 - Budget already exists for this project
```

---

### Scenario 4: Unauthorized Access
**Citizen trying to create project:**
```
Method: POST /projects
Token: citizen_token
Response: 403 - Unauthorized
```

---

### Scenario 5: Invalid Status
**Invalid project status:**
```json
{
  "status": "invalid_status"
}
```

**Response:**
```
400 - Invalid status
Valid: planned, ongoing, completed, stalled
```

---

## 📊 Load Testing Scenarios

### Heavy Comment Thread
```
Create 20 comments on single project
Then retrieve and display
Performance: Should be < 1 second
```

### Multiple Projects
```
Create 10 projects
Update 5 of them
Get all projects with filters
Sort by creation date
```

### Budget History
```
Update budget 15 times
Track all history entries
Verify accuracy of spending calculations
```

---

## 🔐 Role-Based Access Testing

### Citizen Permissions
```
✅ Signup, Login, Get Profile, Update Profile
✅ View Projects, Issues, Comments (Public)
✅ Create Comments, Issues
✅ Escalate Issues
❌ Create/Update Projects
❌ Manage Budgets
❌ Respond to Issues
❌ Admin Operations
```

### Official Permissions
```
✅ All Citizen Permissions
✅ Create Projects
✅ Update Project Status & Progress
✅ Create/Update Budgets
✅ Respond to Issues
✅ View Project Details
❌ Admin Operations
❌ Manage Users
```

### Admin Permissions
```
✅ All Official Permissions
✅ All Citizen Permissions
✅ View All Users
✅ Assign Roles
✅ Block/Unblock Users
✅ Delete Comments
✅ View Platform Statistics
```

---

## 📈 Data Volume Testing

### Small Scale
```
Users: 5 (1 admin, 1 official, 3 citizens)
Projects: 2
Issues: 3
Comments: 5
Budgets: 1
Total Records: ~16
```

### Medium Scale
```
Users: 30
Projects: 10
Issues: 50
Comments: 200
Budgets: 10
Total Records: ~300
```

### Large Scale
```
Users: 500
Projects: 100
Issues: 1000
Comments: 5000
Budgets: 100
Total Records: ~6600+
```

---

## 🎭 Real-World Scenarios

### Scenario 1: Government Transparency
**Real Use Case:** Government publishes road improvement project

```
Monday: Official creates project
Tuesday: Citizens view and comment
Wednesday: Citizens raise issues about traffic impacts
Thursday: Official responds to issues
Friday: Official updates progress
Saturday: Citizens see budget allocation
```

---

### Scenario 2: Citizen Engagement
**Real Use Case:** Community requests park improvement

```
Citizens: Create multiple issues and comments
Officials: Respond with timeline
Admin: Monitor sentiment and engagement
Result: Track community interest in transparency
```

---

### Scenario 3: Issue Escalation
**Real Use Case:** Safety concern not addressed

```
Citizen: Raises safety issue
Official: Reviews but no response
Citizen: Escalates issue
Admin: Sees escalation, investigates
Official: Provides response after escalation
```

---

## 💡 Testing Tips

1. **Use Different Users**: Test with citizen, official, admin accounts
2. **Test All Transitions**: Try invalid state changes
3. **Check Timestamps**: Verify createdAt, updatedAt
4. **Validate Relationships**: Comments linked to projects, issues to projects
5. **Test Pagination**: If implemented, test large data sets
6. **Check Authorization**: Verify role-based restrictions work
7. **Validate Data Format**: Ensure consistent response format

---

**Last Updated:** April 12, 2026  
**Version:** 1.0 - Sample Scenarios Complete
