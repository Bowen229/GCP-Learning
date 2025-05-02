1. IAM Related:
    Users should be added to groups and groups assigned roles to simplify permissions management.
    You assign members to roles through an IAM policy. Roles are combinations of permissions needed for a role. Members can be a Google account, a service account,a Google group, a Google Workspace domain, a Cloud Identity domain, all authenticated users, and all users. A service account is an account for an application instead of an end user. 
    

    IAM involves the following three components:
    Principal: 
        The identity of the person or system that you want to give permissions to.

        Principals represent one or more identities that have authenticated to Google Cloud. And it can be divided into 2 categories:
            1. Human Users: Google Accounts, Google groups and ffederated identities in workforce identity pools.
            2. Workloads: Service accounts and federated identities in a workload identity pool.
        
            Note:
                Workforce Identity Pools: Tailored for human users to access GCP resources using existing credentials.
                Workload Identity Pools: Tailored for applications or services needing authenticated access to GCP resources without managing service account keys.

    Roles and Permissions: 
        The collection of permissions that you want to give the principal.

        You cannot directly grant permission to a principal. Instead, you give principals permissions by granting them roles.

        Roles are collections of Permissions. When you grant a role a principal, you give that principal all the permissions in that role. 

        There are 3 types of roles: 
            1. Predefined roles: Roles that are managed by Google Cloud services. These roles contain the permissions needed to perform common tasks for each given service. For example, the Pub/Sub Publisher role (roles/pubsub.publisher) provides access to publish messages to a Pub/Sub topic.

            2. Custom roles: Roles that you create that contain only the permissions that you specify. You have complete control over the permissions in these roles. However, they have a higher maintenance burden than predefined roles and there's a limit to the number of custom roles that you can have in your project and in your organization.

            3. Basic roles: Highly permissive roles that provide broad access to Google Cloud services. These roles can be useful for testing purposes, but shouldn't be used in production environments.

        
    Resource: 
        The Google resource that you wanto to let the principal access.
        Most Google Cloud services have their own resources. For example, Compute Engine has resources like instances, disks, and subnetworks.

        In IAM, you grant roles on a resource. Granting a principal a role on a resource means that the principal can use the permissions in that role to access the resource.

        You can grant roles on a subset of Google Cloud resources. For a full list of resources that you can grant roles on, see Resource types that accept allow policies(https://cloud.google.com/iam/docs/resource-types-with-policies).

        Google Cloud also has several container resources, including projects, folders, and organizations. Granting a principal a role on a container resource gives the principal access the container resource and the resources in that container. This feature lets you use a single role grant to give a principal access to multiple resources, including resources that you can't grant roles on directly. For more information, see Policy inheritance on this page.



2. Resource Hierarchy 
    Resources can be managed using Resource Manger.
    There are 2 main purposes of the Google Cloud Resource hierarchy: 
        1. Hierarchy of Ownership:
            This aspect of the resource hierarchy allows GCP resources to be organized in a parent-child relationship, where each resource is linked to its immediate parent.

            The hierarchy defines how resources are managed and provides a context for ownership and lifecycle management. For example, when a project is deleted, all resources within that project are also considered for deletion. This binding helps maintain a clear structure and governance over resources.

        2. Access Control and Organization Policies:

            The hierarchy serves as a framework for applying access control and organizational policies. By structuring resources hierarchically, permissions and policies can be defined at different levels (such as organization, folder, or project).

            Access control can be inherited from parent to child resources. For instance, if a role is granted at the organization level, all projects and resources within that organization automatically inherit those permissions unless explicitly overridden. Similarly, organization policies can also be applied at different levels, allowing for consistent governance across resources.

    Google Cloud organizes resources hierarchically, where each resource (except the top-level resource) has a single parent:

            1. Organization Resources – Available to Google Workspace and Cloud Identity users, serving as the highest level in the hierarchy. They provide centralized visibility and control.
                
                An organization resource exposed by the Cloud Resource Manager API consists of the following:

                    1. An organization resource ID, which is a unique identifier for an organization.
                    2. A display name, which is generated from the primary domain name in Google Workspace or Cloud Identity.
                    3. The creation time of the organization resource.
                    4. The last modified time of the organization resource.
                    5. The owner of the organization resource. The owner is specified when creating the organization resource. It cannot be changed once it is set. It is the Google Workspace customer ID that is specified in the Directory API.


            2. Folder Resources – An optional intermediate grouping mechanism between organizations and projects, useful for structuring resources within an enterprise.
                    Folder resources provide an optional grouping mechanism within an organization resource, acting as sub-organizations for structuring projects. They can represent legal entities, departments, teams, or applications.

                    1.Hierarchy: Folders can contain projects or other folders, allowing multi-level structuring.

                    2.Access Control: Enables delegation of administrative rights, so department heads can manage their own resources.

                    3.Isolation: Restricts access to resources based on folder placement.

                    4.Visibility: Can be viewed in the Google Cloud console with proper permissions.

            3.Project Resources – Can be standalone (for free-tier users) or part of a larger hierarchy. They can later be migrated into an Organization Resource.
                A project resource is the base-level organizing entity in Google Cloud, required to use services, manage APIs, billing, permissions, and collaborators.

                    Key Attributes:
                    Identifiers:

                    Project resource ID (customized, user-defined)

                    Project resource number (system-assigned, read-only)

                    Display Name: Mutable name for UI display

                    Lifecycle State: Indicates status (e.g., ACTIVE, DELETE_REQUESTED)

                    Labels: Used for filtering projects

                    Creation Timestamp: Tracks when the project was created

                    Usage & Access Control:
                    A project resource must be specified in API requests (via project ID or number).

                    IAM policy initially grants owner rights to the project creator.

                    Projects exist within an organization or folder but can also stand alone.
      
            4. Service Resources – Fundamental components such as VMs, Pub/Sub topics, and Cloud Storage buckets. These belong to Project Resources, which serve as the first level of grouping.

        Google Cloud IAM allows for granular access control, defining who (users) has what access (roles) to which resources by setting IAM policies at different levels in the resource hierarchy.

            Key Concepts:
            IAM Policy Levels:

            Policies can be set at the organization, folder, project, or resource level.

            Lower-level resources inherit policies from their parent resources.

            Role Inheritance:

            Permissions cannot be explicitly removed at a lower level if granted at a higher level.

            Example: If Bob is granted Project Editor at the folder level, he retains that role for all child projects, even if explicitly removed from one project.

            Policy Changes & Hierarchy:

            Moving a project between folders or into an organization updates its inherited permissions.

            The effective policy is a combination of direct assignments and inherited policies.

            IAM ensures secure and structured access control across Google Cloud resources by leveraging policy inheritance and hierarchical management.