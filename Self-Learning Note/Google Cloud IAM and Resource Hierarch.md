# Google Cloud IAM and Resource Hierarchy

## 1. IAM (Identity and Access Management)

### Users and Roles Management
- Users should be **added to groups**, and groups should be **assigned roles** to simplify permissions management.
- IAM policies define **who** (members) has **what access** (roles) to **which resources**.
- Members can be:
  - Google accounts
  - Service accounts (for applications instead of users)
  - Google groups
  - Google Workspace domains
  - Cloud Identity domains
  - All authenticated users and all users  

### IAM Components
#### **1. Principal**
- The **identity** (person or system) receiving permissions.
- **Types of Principals**:
  1. **Human Users**: Google Accounts, Google Groups, Federated Identities in Workforce Identity Pools.
  2. **Workloads**: Service Accounts, Federated Identities in a Workload Identity Pool.

  **Note:**
  - **Workforce Identity Pools**: For human users accessing Google Cloud with existing credentials.
  - **Workload Identity Pools**: For applications/services requiring authenticated access without managing service account keys.

#### **2. Roles and Permissions**
- A **role** is a collection of **permissions** that define what actions a principal can perform.
- Permissions **cannot** be assigned directly but are granted through roles.

**Types of Roles:**
1. **Predefined Roles**: Managed by Google, tailored for common tasks (e.g., `roles/pubsub.publisher`).
2. **Custom Roles**: User-defined roles with specific permissions, allowing for fine-tuned access control.
3. **Basic Roles**: Broad permissions (`Owner`, `Editor`, `Viewer`), not recommended for production.

#### **3. Resources**
- The **Google Cloud entity** the principal can access (e.g., Compute Engine instances, disks, subnetworks).
- Roles are granted **on a resource** level.
- Google Cloud provides **container resources**:
  - **Projects**
  - **Folders**
  - **Organizations**  
  - Granting access at a container level applies to all **sub-resources**.

For a full list of resources supporting IAM policies, refer to [Resource types that accept allow policies](https://cloud.google.com/iam/docs/resource-types-with-policies).

---

## 2. Google Cloud Resource Hierarchy

### **Purpose of Resource Hierarchy**
1. **Hierarchy of Ownership**:
   - Google Cloud organizes resources **in a parent-child relationship**.
   - When a **parent resource** (e.g., a project) is deleted, its **child resources** (e.g., storage, compute) are also removed.

2. **Access Control & Organization Policies**:
   - **IAM roles** and **policies** can be applied at different levels (**organization, folder, project**).
   - **Inheritance**: Permissions granted at a higher level are **automatically inherited** by lower-level resources.
   - Example: If a role is granted at the **organization level**, all contained **folders, projects, and resources** inherit it.

---

## 3. Resource Hierarchy Levels

### **1. Organization Resources**
- **Highest-level container** (available to Google Workspace & Cloud Identity users).
- Centralized management and access control.

#### **Attributes:**
- **Organization resource ID**: Unique identifier.
- **Display Name**: Derived from Google Workspace or Cloud Identity domain.
- **Creation & Modification Time**: Tracks resource history.
- **Owner**: Set during creation (Google Workspace Customer ID).

---

### **2. Folder Resources**
- **Optional intermediate grouping mechanism** between Organizations and Projects.
- Acts as **sub-organizations** for structuring resources.

#### **Folder Characteristics:**
- **Hierarchy**: Can contain projects and sub-folders.
- **Access Control**: Delegation of admin rights for better resource management.
- **Isolation**: Access control can be restricted based on folder placement.
- **Visibility**: Can be managed/viewed in the Google Cloud console.

---

### **3. Project Resources**
- **Base-level organizing entity**, required for:
  - **APIs**
  - **Billing**
  - **Permissions Management**
  - **Collaboration**  

#### **Key Attributes:**
- **Identifiers:**
  - **Project Resource ID**: Customizable, user-defined.
  - **Project Resource Number**: System-generated, read-only.
- **Lifecycle State**: `ACTIVE`, `DELETE_REQUESTED`, etc.
- **Labels**: Used for filtering and categorization.
- **IAM Policy**: Initially grants **owner rights** to the creator.

Projects can exist **within an Organization or Folder** but may also stand alone.

---

### **4. Service Resources**
- **Fundamental components** of Google Cloud, such as:
  - Virtual Machines (VMs)
  - Pub/Sub topics
  - Cloud Storage buckets
- **Service Resources belong to a Project Resource**, which is the **first level of grouping**.

---

## 4. IAM Policy Inheritance and Hierarchy

### **IAM Policy Levels**
- IAM policies can be assigned at:
  - **Organization level**
  - **Folder level**
  - **Project level**
  - **Resource level**
- **Inheritance**:
  - Policies are **passed down** from parent to child resources.
  - Example: If **Bob** is granted `Project Editor` at the **folder level**, he retains **edit access** on all child projects, even if removed from one project.

### **Policy Changes & Hierarchy Adjustments**
- Moving a **project between folders** or into an **organization** updates its inherited permissions.
- **Effective Policy** = Directly assigned permissions + Inherited permissions.

---

## Conclusion
Google Cloud IAM provides **structured, hierarchical access control** by defining **who can access what** at different levels.  
Using **roles, policies, and inheritance**, organizations can **manage access efficiently** while maintaining **security and governance**.