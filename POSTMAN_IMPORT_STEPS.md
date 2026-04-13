# How to Import Postman Collection & Login as Admin

## 📥 Step 1: Download the Collection File

The file `CivicFusion_API.postman_collection.json` is already in your project root:
```
d:\sem 5\civic fusion\CivicFusion_API.postman_collection.json
```

---

## 🔵 Step 2: Open Postman

1. **Launch Postman** (desktop app)
2. You should see the main Postman window

---

## 📤 Step 3: Import the Collection

### Option A: Using Import Button (Easiest)

1. In the top-left corner, click the **Import** button
   ```
   [Import] button is usually visible in top menu
   ```

2. A dialog box appears with import options

3. Click **Upload Files** tab

4. Click **Select Files** button

5. Navigate to: `d:\sem 5\civic fusion\`

6. Select: `CivicFusion_API.postman_collection.json`

7. Click **Open**

8. Click **Import** button in the dialog

✅ **Collection imported successfully!**

---

### Option B: Drag and Drop

1. Open file explorer: `d:\sem 5\civic fusion\`
2. Find `CivicFusion_API.postman_collection.json`
3. Drag it directly into Postman window
4. Click **Import**

---

## 🔧 Step 4: Create Environment

1. In **left sidebar**, click **Environments**

2. Click **+** button (or "Create Environment")

3. Name it: `Civic Fusion Local`

4. Add these **Variables**:

| Variable | Initial Value |
|----------|---------------|
| `base_url` | `http://localhost:5000/api` |
| `admin_token` | `` (leave empty) |
| `admin_id` | `` (leave empty) |
| `citizen_token` | `` (leave empty) |
| `official_token` | `` (leave empty) |
| `project_id` | `` (leave empty) |
| `issue_id` | `` (leave empty) |
| `comment_id` | `` (leave empty) |
| `official_id` | `` (leave empty) |

5. Click **Save** button (top right)

---

## 🎯 Step 5: Select Environment

1. Look at **top-right corner** of Postman

2. You'll see environment dropdown (currently says "No Environment" or similar)

3. Click the dropdown

4. Select: **Civic Fusion Local**

✅ **Your environment is now active**

---

## 👤 Step 6: Login as Admin

### 6a. Find Admin Login Request

1. In **left sidebar**, you should see the collection:
   ```
   Civic Fusion API
   ├── 🔐 AUTHENTICATION
   │   ├── 1. Admin Login
   │   ├── 2. Signup - Citizen
   │   └── ...
   ```

2. Click on **🔐 AUTHENTICATION** to expand it

3. Click on **1. Admin Login**

---

### 6b. Send Admin Login Request

The request should look like this:

```
POST {{base_url}}/auth/login
Content-Type: application/json

Body:
{
  "email": "admin@civicfusion.com",
  "password": "admin123"
}
```

The email and password are **already pre-filled** in the collection!

1. Click the **Send** button (blue button on right side)

2. Wait for response...

---

## ✅ Step 7: Verify Admin Login

### Expected Response:

```json
{
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "507f1f77bcf86cd799439000",
    "name": "System Admin",
    "email": "admin@civicfusion.com",
    "role": "admin"
  }
}
```

### Check Environment Variables:

1. Go to **Environments** (left sidebar)
2. Click **Civic Fusion Local**
3. Look for `admin_token` - should now be filled with a long token string
4. Look for `admin_id` - should have the ID

---

## 🚀 Step 8: Test Admin Access

Now you can test admin endpoints! Try this:

### Get All Users

1. In collection, go to: **👥 ADMIN**

2. Click **Get All Users**

3. Verify header has:
   ```
   Authorization: Bearer {{admin_token}}
   ```

4. Click **Send**

5. You should see list of all users

---

## 📋 Quick Checklist

After importing and logging in:

- [ ] Collection imported (see "Civic Fusion API" in left sidebar)
- [ ] Environment created: "Civic Fusion Local"
- [ ] Environment selected (top-right dropdown)
- [ ] `base_url` set to `http://localhost:5000/api`
- [ ] Admin Login request sent successfully
- [ ] `admin_token` populated in environment
- [ ] `admin_id` populated in environment
- [ ] Can access admin endpoints (Get All Users works)

---

## 🔑 Admin Credentials Reference

```
Email: admin@civicfusion.com
Password: admin123
```

✅ These are pre-filled in the collection!

---

## 📂 Collection Structure

Once imported, you'll see this structure:

```
Civic Fusion API (Collection)
│
├── 🔐 AUTHENTICATION (6 requests)
│   ├── Admin Login
│   ├── Signup - Citizen
│   ├── Signup - Volunteer
│   ├── Login - Citizen
│   ├── Login - Official
│   └── Logout
│
├── 👤 USER MANAGEMENT (3 requests)
│   ├── Get Profile
│   ├── Update Profile
│   └── Deactivate Account
│
├── 🏗️ PROJECTS (7 requests)
│   ├── Get All Projects
│   ├── Get Single Project
│   ├── Create Project
│   ├── Update Project
│   ├── Update Project Status
│   ├── Update Project Progress
│   └── Archive Project
│
├── 💰 BUDGETS (4 requests)
│   ├── Get Budget by Project
│   ├── Get Budget History
│   ├── Add Budget to Project
│   └── Update Budget
│
├── 💬 COMMENTS (4 requests)
│   ├── Get Comments on Project
│   ├── Add Comment
│   ├── Update Comment
│   └── Delete Comment
│
├── 📋 ISSUES (6 requests)
│   ├── Get All Issues
│   ├── Get Single Issue
│   ├── Create Issue
│   ├── Update Issue Status
│   ├── Respond to Issue
│   └── Escalate Issue
│
└── 👥 ADMIN (9 requests)
    ├── Get All Users
    ├── Get Single User
    ├── Assign Role - Make Official
    ├── Assign Role - Make Admin
    ├── Block User
    ├── Unblock User
    ├── Deactivate User
    ├── Reactivate User
    ├── Delete Comment (Admin)
    └── Get Platform Statistics
```

---

## 💡 Pro Tips

### Auto-Save Tokens
The collection has **"post-request scripts"** that automatically save tokens to your environment!

- After **Admin Login** → `admin_token` auto-saved
- After **Create Project** → `project_id` auto-saved
- After **Create Issue** → `issue_id` auto-saved

You don't need to manually copy-paste!

### View Variables
To see all saved variables:
```
Environments → Civic Fusion Local → See all variables with values
```

### Test Without Manual Setup
The requests are pre-configured with:
- Correct HTTP methods (GET, POST, PUT, DELETE)
- Correct endpoints ({{base_url}}/...)
- Correct headers (Authorization: Bearer {{token}})
- Sample JSON data

Just click **Send** and it works!

---

## 🐛 Troubleshooting

### "Invalid token" Error
```
❌ Problem: Admin login failed
✅ Solution: 
   1. Check MongoDB is running
   2. Check backend server is running (npm run dev)
   3. Check email/password are correct
   4. Try login again
```

### "Environment not selected"
```
❌ Problem: Variables not working ({{base_url}} shows literally)
✅ Solution:
   1. Top-right of Postman
   2. Select "Civic Fusion Local" environment
   3. Now variables will auto-replace
```

### "Base URL not working"
```
❌ Problem: 404 error when sending request
✅ Solution:
   1. Make sure backend is running: npm run dev
   2. Check port 5000 is correct
   3. Try: http://localhost:5000/api/projects (in browser)
```

### "Collection didn't import"
```
❌ Problem: Can't find CivicFusion_API.postman_collection.json
✅ Solution:
   1. File is in: d:\sem 5\civic fusion\
   2. Make sure file exists
   3. Try import again
   4. Or drag-drop file into Postman
```

---

## Next Steps After Admin Login

Once you're logged in as admin, you can:

1. **View all users** → 👥 ADMIN → Get All Users
2. **View platform stats** → 👥 ADMIN → Get Platform Statistics
3. **Create more test data** → Follow POSTMAN_STEP_BY_STEP_DATA.md
4. **Test all endpoints** → Browse the collection

---

## 📞 Quick Reference

| What | How |
|------|-----|
| Import Collection | Click Import → Upload JSON file |
| Create Environment | Click Environments → + → Add variables |
| Select Environment | Top-right dropdown → Choose "Civic Fusion Local" |
| Login as Admin | 🔐 AUTHENTICATION → 1. Admin Login → Send |
| Save Variables | Post-request scripts auto-save (no manual work!) |
| View Saved Tokens | Environments → Civic Fusion Local |
| Test Admin Access | 👥 ADMIN → Get All Users → Send |

---

Created: April 12, 2026
Status: Ready to Import ✅
