# Civic Fusion - Quick Start Checklist

## 🎯 Your Goals
1. Get access to entire Civic Fusion system
2. Create Postman routes & test each endpoint
3. Login & become an admin
4. Test complete workflow with sample data

---

## ⚡ Quick Start (5 Minutes)

### Step 1: Create Admin User
```bash
cd backend
npm run seed:admin
```

**Default Credentials:**
- Email: `admin@civicfusion.com`
- Password: `admin123`

### Step 2: Import Postman Collection
1. Download: `CivicFusion_API.postman_collection.json`
2. Open Postman → Click **Import** button
3. Select the JSON file
4. Collection imported!

### Step 3: Set Environment Variables
1. Postman → **Environments** (left sidebar)
2. Click **+** → Create new
3. Name: `Civic Fusion Local`
4. Add these variables:

```
base_url = http://localhost:5000/api
admin_token = (leave empty, will auto-fill)
citizen_token = (leave empty, will auto-fill)
official_token = (leave empty, will auto-fill)
project_id = (leave empty, will auto-fill)
issue_id = (leave empty, will auto-fill)
comment_id = (leave empty, will auto-fill)
```

### Step 4: Select Environment
Top-right dropdown → Select "Civic Fusion Local"

---

## 🚀 Complete Testing Flow (25 Minutes)

### Phase 1: Authentication (5 min)
Run these in order:

| # | Request | Expected |
|---|---------|----------|
| 1 | **Admin Login** | ✅ Get admin_token |
| 2 | **Signup - Citizen** | ✅ Get citizen_token |
| 3 | **Signup - Volunteer (For Official)** | ✅ Save official_id |
| 4 | **Assign Role - Make Official** | ✅ User promoted to official |
| 5 | **Login - Official** | ✅ Get official_token |

**In Postman:**
- Go to 🔐 AUTHENTICATION section
- Click each request and hit Send
- Watch tokens auto-populate in environment

---

### Phase 2: Project Management (5 min)

**As Official, Create & Update Project:**

| # | Request | Data |
|---|---------|------|
| 1 | Create Project | Title: "City Bridge Renovation" |
| 2 | Get All Projects | View created project |
| 3 | Update Status | Status: "ongoing" |
| 4 | Update Progress | Progress: 45%, Notes: "Foundation started" |

**In Postman:**
- Go to 🏗️ PROJECTS section
- Use official_token (auto in header)
- project_id auto-saves from creation response

---

### Phase 3: Budget Management (3 min)

**As Official, Add & Update Budget:**

| # | Request | Data |
|---|---------|------|
| 1 | Add Budget | Amount: 5,000,000 |
| 2 | Get Budget | View budget details |
| 3 | Update Budget | Spent: 2,500,000 |

**In Postman:**
- Go to 💰 BUDGETS section
- Uses project_id automatically

---

### Phase 4: Comments (3 min)

**As Citizen, Add Comments:**

| # | Request | Data |
|---|---------|------|
| 1 | Add Comment | "Great project! Excited about it." |
| 2 | Get Comments | View all comments |
| 3 | Update Comment | Modify your comment |

**In Postman:**
- Go to 💬 COMMENTS section
- Uses citizen_token (auto in header)
- comment_id auto-saves

---

### Phase 5: Issues (5 min)

**As Citizen, Create Issue → Official Responds:**

| # | Request | Data |
|---|---------|------|
| 1 | Create Issue | Title: "Schedule Delay" |
| 2 | Get Issue | View issue details |
| 3 | Update Status (Official) | Status: "in_progress" |
| 4 | Respond to Issue (Official) | Provide response |

**In Postman:**
- Go to 📋 ISSUES section
- Citizen creates → Official updates
- issue_id auto-saves

---

### Phase 6: Admin Dashboard (4 min)

**As Admin, Manage Platform:**

| # | Request | What It Does |
|---|---------|--------------|
| 1 | Get All Users | See citizen, official, admin users |
| 2 | Get Platform Stats | View total projects, issues, budgets |
| 3 | Block/Unblock User | Test user moderation |

**In Postman:**
- Go to 👥 ADMIN section
- Use admin_token (auto in header)

---

## 📊 Sample Data Reference

### Users
```
Admin (Seed):
- Email: admin@civicfusion.com
- Password: admin123
- Role: admin

Citizen:
- Email: citizen@test.com
- Password: password123
- Role: citizen

Official:
- Email: official@test.com
- Password: password123
- Role: official (after promotion)
```

### Projects
```
Title: City Bridge Renovation
Department: Public Works
Location: Downtown - Main Street Bridge
Description: Complete renovation and structural reinforcement...
Status: planned → ongoing → completed
Progress: 0 → 25 → 50 → 75 → 100
```

### Issues
```
Title: Delayed Schedule - Week 3 Behind
Category: Timeline Delay
Priority: high
Status: open → in_progress → resolved
```

### Comments
```
Content: "This project is important for community safety."
```

### Budgets
```
Allocated Amount: 5,000,000
Spent Amount: 2,500,000
```

---

## 🔑 Authorization Header

For **protected endpoints**, Postman automatically adds:
```
Authorization: Bearer {{token}}
```

The double curly braces {{ }} means Postman will replace with actual token value.

**In Postman:**
- Headers tab → Authorization header already set
- Postman auto-replaces {{admin_token}}, {{citizen_token}}, etc.

---

## ✅ Verification Checklist

After completing flow, verify:

- [ ] Admin logged in (admin_token populated)
- [ ] Citizen account created (citizen_token populated)
- [ ] Official promoted from volunteer (official_token populated)
- [ ] Project created (project_id populated)
- [ ] Budget added to project
- [ ] Comment added to project
- [ ] Issue created for project
- [ ] Issue responded by official
- [ ] Admin can view all users
- [ ] Admin can see platform stats

---

## 🐛 Troubleshooting

### "Invalid credentials" error
```
❌ Email/password wrong
✅ Use exact credentials from quick start
✅ Check admin seed ran successfully
```

### "Authorization failed" error
```
❌ Token missing or expired
✅ Re-login to get fresh token
✅ Check {{token}} variable has value
```

### "Project not found" error
```
❌ project_id not set
✅ Create project first
✅ Check {{project_id}} populated
```

### "Validation failed" error
```
❌ Missing required fields
✅ Check all fields are filled
✅ Follow sample data exactly
```

### Variables not auto-populating
```
❌ Post-request script not running
✅ Check "Tests" tab in Postman
✅ Script should say: pm.environment.set(...)
```

---

## 📝 Environment Variables Guide

| Variable | When Filled | How |
|----------|-----------|-----|
| `base_url` | Initial | Set manually to `http://localhost:5000/api` |
| `admin_token` | After admin login | Auto-filled by post-request script |
| `citizen_token` | After citizen signup/login | Auto-filled by post-request script |
| `official_token` | After official login | Auto-filled by post-request script |
| `project_id` | After project creation | Auto-filled by post-request script |
| `issue_id` | After issue creation | Auto-filled by post-request script |
| `comment_id` | After comment creation | Auto-filled by post-request script |

**View Variables:**
1. Postman → Environments
2. Click "Civic Fusion Local"
3. See all current variable values

---

## 💡 Pro Tips

1. **Use Collection Runner** (Postman)
   - Click collection name → Run
   - Auto-runs requests in sequence
   - Useful for testing entire flow at once

2. **Save Responses as Examples**
   - Right-click response → Save as example
   - See example without running request again

3. **Check Tests Tab**
   - See what auto-save script is doing
   - View assertions for responses

4. **Use Variables Everywhere**
   - Instead of copying IDs: Use {{variable}}
   - Makes requests reusable

5. **Pretty Print JSON**
   - Response body → Click "Pretty" button
   - Easier to read nested data

---

## 📂 Files Created

| File | Purpose |
|------|---------|
| `POSTMAN_COMPLETE_GUIDE.md` | Detailed documentation for all routes |
| `CivicFusion_API.postman_collection.json` | Import this into Postman |
| `POSTMAN_QUICK_START_CHECKLIST.md` | This file - quick reference |

---

## 🎓 Learning Path

**If you're new to the project:**
1. Read `ARCHITECTURE.md` - understand system design
2. Follow `POSTMAN_QUICK_START_CHECKLIST.md` (this file) - hands-on testing
3. Reference `POSTMAN_COMPLETE_GUIDE.md` - detailed endpoint docs
4. Explore Backend README - how server works

**If you want to test specific feature:**
- Each feature has dedicated section in complete guide
- Sample data provided for each request
- Expected responses shown

---

## 🚀 Next Steps

After testing complete workflow:

1. **Customize Data**: Replace sample emails with real test cases
2. **Add More Routes**: Import .json file, duplicate requests, modify
3. **Automate Tests**: Use Postman's test scripts for assertions
4. **Integration Testing**: Test frontend ↔ backend integration
5. **Performance Testing**: Use Postman's performance testing features

---

## 📞 Support

If stuck:
1. Check `POSTMAN_COMPLETE_GUIDE.md` - "Common Issues" section
2. Review `ARCHITECTURE.md` - understand data flow
3. Check backend server logs for errors
4. Verify MongoDB is running: `mongod`
5. Verify backend is running: `npm run dev`

---

**Created:** April 12, 2026  
**Status:** Ready to Test ✅  
**Time to Complete:** 25 minutes

Happy Testing! 🎉
