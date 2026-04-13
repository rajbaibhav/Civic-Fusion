# Civic Fusion - Step-by-Step Data for Postman Testing

Follow this guide step-by-step to test the entire system. Copy-paste the JSON data into Postman and execute in order.

---

## 🔒 STEP 0: Admin User (Already Seeded)

Before starting, run:
```bash
npm run seed:admin
```

**Default Admin:**
- Email: `admin@civicfusion.com`
- Password: `admin123`
- Role: `admin`

---

## 👥 STEP 1: Create User #1 - CITIZEN

### 1a. Signup as Citizen

**Request:**
```
POST {{base_url}}/auth/signup
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "name": "Rajesh Kumar",
  "email": "rajesh.kumar@civic.com",
  "password": "password123",
  "role": "citizen"
}
```

**Expected Response:**
```json
{
  "message": "User registered successfully",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "507f1f77bcf86cd799439011",
    "name": "Rajesh Kumar",
    "email": "rajesh.kumar@civic.com",
    "role": "citizen"
  }
}
```

**What to Save:**
- Copy the `token` → Save as: `citizen_token`
- Copy the `id` → Save as: `citizen_id`

**In Postman Environment:**
```
citizen_token = [paste token here]
citizen_id = [paste id here]
```

---

### 1b. Login as Citizen (Verify)

**Optional Verification:** Login again to confirm credentials work

**Request:**
```
POST {{base_url}}/auth/login
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "email": "rajesh.kumar@civic.com",
  "password": "password123"
}
```

**Expected Response:** Same token as signup

---

## 👥 STEP 2: Create User #2 - VOLUNTEER (To Promote to OFFICIAL)

### 2a. Signup as Volunteer

**Request:**
```
POST {{base_url}}/auth/signup
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "name": "Priya Sharma",
  "email": "priya.sharma@civic.com",
  "password": "password123",
  "role": "volunteer"
}
```

**Expected Response:**
```json
{
  "message": "User registered successfully",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "507f1f77bcf86cd799439012",
    "name": "Priya Sharma",
    "email": "priya.sharma@civic.com",
    "role": "volunteer"
  }
}
```

**What to Save:**
- Copy the `id` → Save as: `official_id` (will promote to official)

**In Postman Environment:**
```
official_id = [paste id here]
```

---

### 2b. Promote Volunteer to OFFICIAL

**Request:**
```
PUT {{base_url}}/admin/users/{{official_id}}/role
Authorization: Bearer {{admin_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
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
    "id": "507f1f77bcf86cd799439012",
    "name": "Priya Sharma",
    "email": "priya.sharma@civic.com",
    "role": "official"
  }
}
```

---

### 2c. Login as Official

**Request:**
```
POST {{base_url}}/auth/login
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "email": "priya.sharma@civic.com",
  "password": "password123"
}
```

**Expected Response:**
```json
{
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "507f1f77bcf86cd799439012",
    "name": "Priya Sharma",
    "email": "priya.sharma@civic.com",
    "role": "official"
  }
}
```

**What to Save:**
- Copy the `token` → Save as: `official_token`

**In Postman Environment:**
```
official_token = [paste token here]
```

---

## 👥 STEP 3: Create User #3 - VOLUNTEER (Optional Extra)

### 3a. Signup as Volunteer #2

**Request:**
```
POST {{base_url}}/auth/signup
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "name": "Amit Patel",
  "email": "amit.patel@civic.com",
  "password": "password123",
  "role": "volunteer"
}
```

**Expected Response:**
```json
{
  "message": "User registered successfully",
  "token": "...",
  "user": {
    "id": "507f1f77bcf86cd799439013",
    "name": "Amit Patel",
    "email": "amit.patel@civic.com",
    "role": "volunteer"
  }
}
```

**What to Save:**
- Copy the `id` → Save as: `volunteer_id`

---

## 📊 USER SUMMARY SO FAR

| # | Name | Email | Role | Token Variable |
|---|------|-------|------|-----------------|
| 1 | Rajesh Kumar | rajesh.kumar@civic.com | citizen | `{{citizen_token}}` |
| 2 | Priya Sharma | priya.sharma@civic.com | official | `{{official_token}}` |
| 3 | Amit Patel | amit.patel@civic.com | volunteer | - |
| Admin | System Admin | admin@civicfusion.com | admin | `{{admin_token}}` |

---

## 🏗️ STEP 4: Create PROJECT #1

### Owner: Official (Priya Sharma)

**Request:**
```
POST {{base_url}}/projects
Authorization: Bearer {{official_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "title": "City Bridge Renovation",
  "description": "Complete renovation and structural reinforcement of the main city bridge to ensure public safety and improve infrastructure. This includes engineering assessment, repair of damaged sections, and installation of modern safety features.",
  "department": "Public Works",
  "location": "Downtown - Main Street Bridge"
}
```

**Expected Response:**
```json
{
  "message": "Project created successfully",
  "project": {
    "_id": "507f1f77bcf86cd799439100",
    "title": "City Bridge Renovation",
    "description": "...",
    "department": "Public Works",
    "location": "Downtown - Main Street Bridge",
    "status": "planned",
    "progress": 0,
    "createdBy": {
      "id": "507f1f77bcf86cd799439012",
      "name": "Priya Sharma",
      "role": "official"
    },
    "createdAt": "2026-04-12T10:30:00Z"
  }
}
```

**What to Save:**
- Copy `project._id` → Save as: `project_id_1`

**In Postman Environment:**
```
project_id = 507f1f77bcf86cd799439100
project_id_1 = 507f1f77bcf86cd799439100
```

---

## 🏗️ STEP 5: Create PROJECT #2

### Owner: Official (Priya Sharma)

**Request:**
```
POST {{base_url}}/projects
Authorization: Bearer {{official_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "title": "Community Park Development",
  "description": "Development of a new community park in the residential area. The project includes construction of playground equipment, walking trails, seating areas, and green landscaping. Aimed at improving quality of life for residents.",
  "department": "Parks & Recreation",
  "location": "North District - Smith Park"
}
```

**Expected Response:**
```json
{
  "message": "Project created successfully",
  "project": {
    "_id": "507f1f77bcf86cd799439101",
    "title": "Community Park Development",
    "status": "planned",
    "progress": 0,
    ...
  }
}
```

**What to Save:**
- Copy `project._id` → Save as: `project_id_2`

---

## 🏗️ STEP 6: Create PROJECT #3

### Owner: Official (Priya Sharma)

**Request:**
```
POST {{base_url}}/projects
Authorization: Bearer {{official_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "title": "Water Treatment Plant Upgrade",
  "description": "Upgrade existing water treatment plant with modern filtration systems and desalination technology to improve water quality and reliability. Project includes testing, installation, and staff training.",
  "department": "Water & Sanitation",
  "location": "Industrial Zone - East"
}
```

**What to Save:**
- Copy `project._id` → Save as: `project_id_3`

---

## 📊 PROJECTS SUMMARY

| # | Project | Department | Status | Progress |
|---|---------|-----------|--------|----------|
| 1 | City Bridge Renovation | Public Works | planned | 0% |
| 2 | Community Park Development | Parks & Recreation | planned | 0% |
| 3 | Water Treatment Plant Upgrade | Water & Sanitation | planned | 0% |

---

## 💬 STEP 7: Add COMMENT #1

### By: Citizen (Rajesh Kumar) on Project 1

**Request:**
```
POST {{base_url}}/comments/project/{{project_id_1}}
Authorization: Bearer {{citizen_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "content": "Great initiative! This bridge is critical for city connectivity. Looking forward to seeing the renovation completed. It will greatly improve safety and reduce traffic congestion."
}
```

**Expected Response:**
```json
{
  "message": "Comment added successfully",
  "comment": {
    "_id": "507f1f77bcf86cd799439200",
    "project": {
      "_id": "507f1f77bcf86cd799439100",
      "title": "City Bridge Renovation"
    },
    "user": {
      "_id": "507f1f77bcf86cd799439011",
      "name": "Rajesh Kumar",
      "role": "citizen"
    },
    "content": "Great initiative! This bridge is critical...",
    "createdAt": "2026-04-12T10:45:00Z"
  }
}
```

**What to Save:**
- Copy `comment._id` → Save as: `comment_id_1`

---

## 💬 STEP 8: Add COMMENT #2

### By: Citizen (Rajesh Kumar) on Project 2

**Request:**
```
POST {{base_url}}/comments/project/{{project_id_2}}
Authorization: Bearer {{citizen_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "content": "Perfect! Our neighborhood really needs a park. Kids in the area will finally have a safe place to play. Thank you for this wonderful project!"
}
```

**Expected Response:** Similar structure, new comment_id

**What to Save:**
- Copy `comment._id` → Save as: `comment_id_2`

---

## 💬 STEP 9: Add COMMENT #3

### By: Volunteer (Amit Patel) on Project 1

**First, Login as Volunteer:**

**Request:**
```
POST {{base_url}}/auth/login
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "email": "amit.patel@civic.com",
  "password": "password123"
}
```

**Save the token as:** `volunteer_token`

**Now Add Comment:**

**Request:**
```
POST {{base_url}}/comments/project/{{project_id_1}}
Authorization: Bearer {{volunteer_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "content": "As a structural engineer, I recommend conducting thorough foundation assessment before proceeding. The bridge has been heavily used for 30 years and may have unseen structural issues."
}
```

**What to Save:**
- Copy `comment._id` → Save as: `comment_id_3`

---

## 📊 COMMENTS SUMMARY

| # | Author | Project | Content |
|---|--------|---------|---------|
| 1 | Rajesh Kumar (Citizen) | Bridge Renovation | Great initiative! Improving safety... |
| 2 | Rajesh Kumar (Citizen) | Park Development | We need a park for kids to play... |
| 3 | Amit Patel (Volunteer) | Bridge Renovation | Structural engineer recommendation... |

---

## 📋 STEP 10: Create ISSUE #1

### By: Citizen (Rajesh Kumar) on Project 1

**Request:**
```
POST {{base_url}}/issues/project/{{project_id_1}}
Authorization: Bearer {{citizen_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "title": "Safety Hazard: Inadequate Warning Barriers",
  "description": "The construction site of the bridge renovation lacks proper warning barriers and safety signage. This is a serious safety concern as children and pedestrians can access the dangerous construction area. I've seen multiple instances of people wandering into the unsafe zone.",
  "category": "Safety",
  "priority": "high"
}
```

**Expected Response:**
```json
{
  "message": "Issue created successfully",
  "issue": {
    "_id": "507f1f77bcf86cd799439300",
    "project": {
      "_id": "507f1f77bcf86cd799439100",
      "title": "City Bridge Renovation"
    },
    "raisedBy": {
      "_id": "507f1f77bcf86cd799439011",
      "name": "Rajesh Kumar",
      "role": "citizen"
    },
    "title": "Safety Hazard: Inadequate Warning Barriers",
    "description": "...",
    "category": "Safety",
    "priority": "high",
    "status": "open",
    "responses": [],
    "createdAt": "2026-04-12T11:00:00Z"
  }
}
```

**What to Save:**
- Copy `issue._id` → Save as: `issue_id_1`

---

## 📋 STEP 11: Create ISSUE #2

### By: Citizen (Rajesh Kumar) on Project 1

**Request:**
```
POST {{base_url}}/issues/project/{{project_id_1}}
Authorization: Bearer {{citizen_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "title": "Schedule Delay: Project 2 Weeks Behind",
  "description": "According to the project timeline, the renovation should have started 2 weeks ago, but there's been no visible work yet. The delay impacts the entire city's traffic management plan. When will actual construction begin?",
  "category": "Timeline",
  "priority": "high"
}
```

**What to Save:**
- Copy `issue._id` → Save as: `issue_id_2`

---

## 📋 STEP 12: Create ISSUE #3

### By: Citizen (Rajesh Kumar) on Project 2

**Request:**
```
POST {{base_url}}/issues/project/{{project_id_2}}
Authorization: Bearer {{citizen_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "title": "Budget Transparency Request",
  "description": "Can the administration please provide a breakdown of how the park development budget will be allocated? Citizens want to know the costs for playground equipment, landscaping, construction, etc.",
  "category": "Transparency",
  "priority": "medium"
}
```

**What to Save:**
- Copy `issue._id` → Save as: `issue_id_3`

---

## 📊 ISSUES SUMMARY

| # | Title | Category | Priority | Status |
|---|-------|----------|----------|--------|
| 1 | Safety Hazard | Safety | high | open |
| 2 | Schedule Delay | Timeline | high | open |
| 3 | Budget Transparency | Transparency | medium | open |

---

## 💰 STEP 13: Add BUDGET #1

### For: Project 1 (Bridge Renovation)
### By: Official (Priya Sharma)

**Request:**
```
POST {{base_url}}/budgets/project/{{project_id_1}}
Authorization: Bearer {{official_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "allocatedAmount": 5000000
}
```

**Expected Response:**
```json
{
  "message": "Budget added successfully",
  "budget": {
    "_id": "507f1f77bcf86cd799439400",
    "project": {
      "_id": "507f1f77bcf86cd799439100",
      "title": "City Bridge Renovation"
    },
    "allocatedAmount": 5000000,
    "spentAmount": 0,
    "history": [
      {
        "allocatedAmount": 5000000,
        "spentAmount": 0,
        "updatedBy": {
          "name": "Priya Sharma"
        },
        "notes": "Initial budget allocation",
        "createdAt": "2026-04-12T11:30:00Z"
      }
    ]
  }
}
```

**What to Save:**
- Copy `budget._id` → Save as: `budget_id_1`

---

## 💰 STEP 14: Add BUDGET #2

### For: Project 2 (Park Development)
### By: Official (Priya Sharma)

**Request:**
```
POST {{base_url}}/budgets/project/{{project_id_2}}
Authorization: Bearer {{official_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "allocatedAmount": 2000000
}
```

---

## 💰 STEP 15: Add BUDGET #3

### For: Project 3 (Water Treatment Plant)
### By: Official (Priya Sharma)

**Request:**
```
POST {{base_url}}/budgets/project/{{project_id_3}}
Authorization: Bearer {{official_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "allocatedAmount": 8000000
}
```

---

## 💰 BUDGETS SUMMARY

| # | Project | Allocated | Spent | Status |
|---|---------|-----------|-------|--------|
| 1 | Bridge Renovation | 5,000,000 | 0 | Created |
| 2 | Park Development | 2,000,000 | 0 | Created |
| 3 | Water Treatment | 8,000,000 | 0 | Created |

---

## 🔄 STEP 16: Update Project - Change Status & Progress

### Project 1: Status → ongoing, Progress → 25%

**Request:**
```
PUT {{base_url}}/projects/{{project_id_1}}/status
Authorization: Bearer {{official_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "status": "ongoing"
}
```

**Then Update Progress:**

**Request:**
```
PUT {{base_url}}/projects/{{project_id_1}}/progress
Authorization: Bearer {{official_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "progress": 25,
  "notes": "Site preparation and safety barriers completed. Foundation assessment underway."
}
```

---

## 🔄 STEP 17: Update Budget - Record Spending

### Project 1: Update spent amount

**Request:**
```
PUT {{base_url}}/budgets/project/{{project_id_1}}
Authorization: Bearer {{official_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "allocatedAmount": 5000000,
  "spentAmount": 1250000,
  "notes": "First quarter spending: Site preparation, equipment rental, labor costs"
}
```

---

## 🔄 STEP 18: Update Issue - Official Responds

### Issue 1: Safety Hazard - Official Response

**Request:**
```
POST {{base_url}}/issues/{{issue_id_1}}/respond
Authorization: Bearer {{official_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "response": "We acknowledge your safety concerns. Proper barriers and warning signs have been installed around the construction site. Security patrol has been increased. We are committed to maintaining the highest safety standards throughout the project."
}
```

**Then Update Status:**

**Request:**
```
PUT {{base_url}}/issues/{{issue_id_1}}/status
Authorization: Bearer {{official_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "status": "in_progress"
}
```

---

## 🔄 STEP 19: Update Issue #2 - Official Response

### Issue 2: Schedule Delay

**Request:**
```
POST {{base_url}}/issues/{{issue_id_2}}/respond
Authorization: Bearer {{official_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "response": "The project has completed site assessment and we're now proceeding with the structural work. Work started last week with foundation testing. Expected to see visible progress by next week. Regular updates will be posted on the project page."
}
```

**Update Status:**

**Request:**
```
PUT {{base_url}}/issues/{{issue_id_2}}/status
Authorization: Bearer {{official_token}}
Content-Type: application/json
```

**Copy-Paste This Data:**
```json
{
  "status": "in_progress"
}
```

---

## 📊 FINAL SUMMARY - Everything Created

### Users (4)
```
1. Rajesh Kumar (Citizen) - rajesh.kumar@civic.com
2. Priya Sharma (Official) - priya.sharma@civic.com
3. Amit Patel (Volunteer) - amit.patel@civic.com
4. System Admin - admin@civicfusion.com [seeded]
```

### Projects (3)
```
1. City Bridge Renovation
2. Community Park Development
3. Water Treatment Plant Upgrade
```

### Comments (3)
```
1. Rajesh on Bridge - "Great initiative!"
2. Rajesh on Park - "We need a park for kids"
3. Amit on Bridge - "Structural engineering concerns"
```

### Issues (3)
```
1. Safety Hazard - HIGH (Responded, In Progress)
2. Schedule Delay - HIGH (Responded, In Progress)
3. Budget Transparency - MEDIUM (Open)
```

### Budgets (3)
```
1. Bridge: 5,000,000 (Spent: 1,250,000)
2. Park: 2,000,000 (Spent: 0)
3. Water: 8,000,000 (Spent: 0)
```

---

## 🧪 Testing Verification Checklist

After following all steps, verify:

```
✅ Can login as citizen (rajesh.kumar@civic.com)
✅ Can login as official (priya.sharma@civic.com)
✅ Can login as admin (admin@civicfusion.com)
✅ See 3 projects in project list
✅ See 3 comments under Project 1
✅ See 3 issues in issues list
✅ See budget for each project
✅ Issues show official responses
✅ Project 1 shows status "ongoing" and 25% progress
✅ Budget shows spending tracked
✅ Admin can view all users
✅ Admin can view platform stats
```

---

## 📝 Environment Variables - Final Setup

```
base_url = http://localhost:5000/api

// Admin (Seeded)
admin_token = [from login]
admin_id = [from user details]

// Users
citizen_token = [from signup/login]
citizen_id = 507f1f77bcf86cd799439011
official_token = [from login]
official_id = 507f1f77bcf86cd799439012
volunteer_token = [from login]

// Projects
project_id_1 = 507f1f77bcf86cd799439100
project_id_2 = 507f1f77bcf86cd799439101
project_id_3 = 507f1f77bcf86cd799439102

// Comments
comment_id_1 = 507f1f77bcf86cd799439200
comment_id_2 = 507f1f77bcf86cd799439201
comment_id_3 = 507f1f77bcf86cd799439202

// Issues
issue_id_1 = 507f1f77bcf86cd799439300
issue_id_2 = 507f1f77bcf86cd799439301
issue_id_3 = 507f1f77bcf86cd799439302

// Budgets
budget_id_1 = 507f1f77bcf86cd799439400
budget_id_2 = 507f1f77bcf86cd799439401
budget_id_3 = 507f1f77bcf86cd799439402
```

---

**Follow this guide step-by-step and you'll have a complete working system to test!**

Created: April 12, 2026
