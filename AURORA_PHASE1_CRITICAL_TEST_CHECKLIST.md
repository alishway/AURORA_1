# AURORA API Integration - Phase 1 Critical Test Checklist

## 🎯 Test Objective
Validate Phase 1 foundation before Phase 2 development. Focus on CRUD operations, role-based access, and error handling.

**Estimated Time**: 30 minutes  
**Priority**: Critical for Phase 2 success

---

## ✅ Test 1: UPDATE Operation Fix (URGENT)

### **Test Case 1.1: Edit API Source Description**
**Status**: 🚨 **NEEDS IMMEDIATE TESTING**

**Steps**:
1. **Hard refresh browser** (Ctrl+Shift+R or Cmd+Shift+R)
2. **Click edit icon** on "FBI Wanted Persons API"
3. **Change Description** to: `Federal Bureau of Investigation's public API for wanted persons and fugitive cases - Updated`
4. **Click "Update Source"**

**Expected Results**:
- ✅ Success toast: "API source updated successfully"
- ✅ Dialog closes automatically
- ✅ Description updates in the list view
- ✅ Console shows: `🔍 Updating API source with data:` and `🔍 Final update data:`

**Priority**: 🚨 **CRITICAL** - Must pass before other tests

---

## 🔐 Test 2: Role-Based Access Control

### **Test Case 2.1: Create Organization Administrator**
**Objective**: Test limited permissions for org admin

**Steps**:
1. **Login as Aurora Admin** (`admin@aurora.com`)
2. **Navigate to Admin → User Management**
3. **Create new user**:
   ```
   Email: orgadmin@testorg.com
   Full Name: Test Org Admin
   Role: Organization Administrator
   Organization: AURORA (or create new test org)
   ```
4. **Logout and login as org admin**
5. **Navigate to Data Explorer → API Sources**

**Expected Results**:
- ✅ Can access API Sources feature
- ✅ Can see existing FBI API source (if organization matches)
- ✅ Can create new API sources
- ✅ Security tab: No "Top Secret" classification option
- ✅ Limited visibility options (no Public if restricted)

**Priority**: 🔴 **HIGH**

---

### **Test Case 2.2: Create Regular User**
**Objective**: Test most restrictive permissions

**Steps**:
1. **Login as Aurora Admin**
2. **Create regular user**:
   ```
   Email: user@testorg.com
   Full Name: Test Regular User
   Role: User
   Organization: Same as org admin
   ```
3. **Login as regular user**
4. **Test API Sources access**

**Expected Results**:
- ✅ Can access API Sources feature
- ✅ Can create private API sources only
- ✅ Security tab: Only "Unclassified" and "Confidential" classifications
- ✅ Visibility: Only "Private" and "Organization" options
- ✅ Cannot see other users' private sources

**Priority**: 🔴 **HIGH**

---

## 🔄 Test 3: Complete CRUD Operations

### **Test Case 3.1: Create Multiple API Sources**
**Objective**: Test list management and statistics

**As Aurora Admin, create these sources**:

**Source 1: News API**
```
Name: NewsAPI Headlines
Description: Real-time news headlines and articles
API Type: REST
Base URL: https://newsapi.org/v2
Auth Type: API Key
Visibility: Organization
Classification: Unclassified
```

**Source 2: Weather API**
```
Name: OpenWeather Current
Description: Current weather data by location
API Type: REST  
Base URL: https://api.openweathermap.org/data/2.5
Auth Type: API Key
Visibility: Public
Classification: Unclassified
```

**Expected Results**:
- ✅ All 3 sources appear in list (FBI + News + Weather)
- ✅ Statistics show "3 Total Sources, 0 Active, 3 Inactive"
- ✅ Different icons for each source type
- ✅ Correct visibility badges

**Priority**: 🟡 **MEDIUM**

---

### **Test Case 3.2: Edit Different Field Types**
**Objective**: Test various form field updates

**Steps**:
1. **Edit NewsAPI source**:
   - Change **API Type**: REST → GraphQL
   - Change **Visibility**: Organization → Private
   - Change **Classification**: Unclassified → Confidential
   - Update **Rate Limit**: 60 → 100
2. **Save changes**

**Expected Results**:
- ✅ All changes save successfully
- ✅ UI reflects new values immediately
- ✅ Icons update (GraphQL icon appears)
- ✅ Badges update (Private, Confidential)

**Priority**: 🟡 **MEDIUM**

---

### **Test Case 3.3: Delete API Source**
**Objective**: Test deletion functionality

**Steps**:
1. **Click delete icon** on Weather API source
2. **Confirm deletion** in dialog
3. **Verify removal**

**Expected Results**:
- ✅ Confirmation dialog appears
- ✅ Source removed from list after confirmation
- ✅ Statistics update: "2 Total Sources"
- ✅ Success message appears

**Priority**: 🟡 **MEDIUM**

---

## 🛡️ Test 4: Cross-User Visibility

### **Test Case 4.1: Visibility Rules Validation**
**Objective**: Ensure sources appear correctly for different users

**Setup**: Use the 3 user accounts (Aurora Admin, Org Admin, Regular User)

**Test Matrix**:
| Source | Visibility | Aurora Admin | Org Admin | Regular User |
|--------|------------|--------------|-----------|--------------|
| FBI API | Public | ✅ Should see | ✅ Should see | ✅ Should see |
| NewsAPI | Private (Admin) | ✅ Should see | ❌ Should NOT see | ❌ Should NOT see |
| Weather API | Organization | ✅ Should see | ✅ Should see | ✅ Should see |

**Steps**:
1. **Login as each user type**
2. **Check API Sources list**
3. **Verify visibility rules**

**Priority**: 🔴 **HIGH**

---

## ⚠️ Test 5: Form Validation & Error Handling

### **Test Case 5.1: Required Field Validation**
**Objective**: Test form prevents invalid submissions

**Steps**:
1. **Click "Add API Source"**
2. **Leave Name field empty** → Try to submit
3. **Enter invalid URL** (e.g., "not-a-url") → Try to submit
4. **Enter extremely long name** (200+ characters) → Try to submit

**Expected Results**:
- ✅ Form shows clear error messages
- ✅ Submit button remains disabled or shows errors
- ✅ No database calls made for invalid data
- ✅ User-friendly error descriptions

**Priority**: 🟡 **MEDIUM**

---

### **Test Case 5.2: Network Error Handling**
**Objective**: Test behavior during connection issues

**Steps**:
1. **Disconnect internet** (or use browser dev tools to simulate)
2. **Try to create API source**
3. **Reconnect and retry**

**Expected Results**:
- ✅ Appropriate error message for network failure
- ✅ Form doesn't crash or show cryptic errors
- ✅ Retry works after reconnection

**Priority**: 🟡 **MEDIUM**

---

## 📱 Test 6: Responsive Design (Quick Check)

### **Test Case 6.1: Mobile/Tablet View**
**Objective**: Ensure UI works on different screen sizes

**Steps**:
1. **Open browser dev tools** (F12)
2. **Switch to mobile view** (iPhone/iPad simulation)
3. **Test API Sources interface**
4. **Try creating/editing a source**

**Expected Results**:
- ✅ Interface adapts to smaller screens
- ✅ All buttons remain clickable
- ✅ Form dialog fits on screen
- ✅ Text remains readable

**Priority**: 🟢 **LOW** (but good to check)

---

## 🎯 Test Completion Criteria

### **✅ Phase 1 Ready for Phase 2 When**:
- [ ] **UPDATE operation works** (Test 1.1)
- [ ] **Role-based access functional** (Tests 2.1, 2.2)
- [ ] **Basic CRUD operations work** (Tests 3.1, 3.2, 3.3)
- [ ] **Cross-user visibility correct** (Test 4.1)
- [ ] **Form validation robust** (Test 5.1)

### **📊 Success Metrics**:
- **Critical Tests**: 100% pass rate required
- **High Priority**: 90% pass rate required  
- **Medium/Low Priority**: 80% pass rate acceptable

---

## 🚨 Current Status

### **Immediate Action Required**:
1. **🔧 Test UPDATE operation** - Fix just implemented
2. **👥 Create test users** - Role-based access validation
3. **🔄 Test full CRUD cycle** - Ensure stability

### **If Tests Fail**:
- **Document the issue** with screenshots/console logs
- **Priority**: Critical > High > Medium > Low
- **Fix critical issues** before Phase 2

### **If Tests Pass**:
- **🎉 Phase 1 Complete!**
- **Ready for Phase 2** development
- **Solid foundation** for connection testing

---

**⏱️ Estimated Testing Time**:
- **Critical Tests (1-3)**: 15 minutes
- **High Priority Tests (4)**: 10 minutes  
- **Medium/Low Tests (5-6)**: 5 minutes
- **Total**: ~30 minutes

**🚀 Let's start with Test 1.1 - UPDATE operation fix!**
