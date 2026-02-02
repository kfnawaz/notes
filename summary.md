# Problem Statement and Solution Description  
*(Atlan Secure Agent – k3s Read-Only Access Enablement)*

---

## Problem Statement

The organization is currently opening **too many support tickets** with Atlan to obtain **basic operational and diagnostic information** about Atlan services running on the secure agent.

The Atlan Secure Agent runs on **k3s (lightweight Kubernetes)**, which natively exposes a **remote Kubernetes API** that can provide real-time visibility into pods, deployments, logs, and system health. However:

- Remote Kubernetes API access is **not enabled or governed**
- **RBAC (Role-Based Access Control)** is not configured
- SREs and application developers **do not have read-only visibility**
- Access to logs, pod status, and failures requires **manual intervention or vendor support**

### Resulting Impact

- Routine operational checks escalate into **support tickets**
- Increased **mean time to diagnosis (MTTD)**
- Inefficient troubleshooting
- Over-dependency on Atlan support for day-to-day operations

---

## Solution Description

The solution enables **secure, read-only operational visibility** into the Atlan Secure Agent by configuring **RBAC-controlled remote access** to the k3s Kubernetes cluster, without granting administrative privileges.

---

## 1. Enable Secure Remote Access on k3s

- Update the **k3s installation configuration** to:
  - Enable **RBAC authorization mode**
  - Disable **anonymous authentication**
  - Configure **TLS hostnames** for each environment (UAT, Prod, DR)
- Restart the k3s service to apply the changes

This activates Kubernetes-native access control while keeping the cluster secure.

---

## 2. Define Read-Only RBAC Policies

- Create a **read-only role** (e.g., `SRE-read-only`)
- Permissions limited to:
  - Pods
  - Deployments
  - StatefulSets
  - Jobs
  - Logs
  - Describe operations
- Apply access using:
  - Kubernetes **Role**
  - Kubernetes **RoleBinding**
- Each namespace has a corresponding role-binding pair

### Namespace Handling

- A **single multi-document YAML** file includes all environments:
  - Sandbox
  - Experimental
  - UAT
  - Prod
  - DR
- If a namespace does not exist, Kubernetes issues a **warning only**
- Enables **one unified deployment file** across all environments

---

## 3. Certificate-Based Authentication

- Generate **dedicated client certificates** (public/private key pair) for the read-only role
- Avoid use of the default cluster-wide k3s certificate
- Distribute to authorized users:
  - Client certificate
  - Private key
- Ensures:
  - Strong authentication
  - Least-privilege access
  - Scoped credentials per role

---

## 4. User `kubeconfig` Setup

Each user configures a local `~/.kube/config` file containing:

- **Cluster**
  - Secure agent endpoint
  - Server certificate
- **User**
  - Client certificate
  - Private key
- **Context**
  - Binding between cluster and user identity

This guarantees secure, identity-based access with enforced read-only permissions.

---

## 5. Operational Access via VS Code (Recommended)

Use the **VS Code Kubernetes Extension** instead of raw `kubectl` commands.

### Capabilities

- Browse namespaces
- View:
  - Pods
  - Deployments
  - Jobs
  - StatefulSets
- Pod status indicators:
  - 🟢 Running
  - 🔴 Stopped
  - 🟠 Pending
  - ⚫ Error
- View logs (including multi-container selection)
- Describe resources visually

### Benefits

- Faster troubleshooting
- Visual clarity
- Easy screen-sharing and collaboration
- No CLI expertise required

---

## 6. Troubleshooting & Automation Enablement

With read-only access enabled:

- Teams can independently investigate issues (e.g., Snowflake connection failures)
- Logs can be collected directly from failing pods
- Advanced users can automate:
  - Log aggregation
  - Pod health snapshots
  - Diagnostic bundles for vendor escalation

---

## Outcomes & Benefits

- Significant reduction in Atlan support tickets
- Faster root-cause analysis
- Secure, auditable access model
- No administrative exposure
- Consistent configuration across environments

**Result:** Kubernetes shifts from a black box to a controlled observability platform while maintaining strict security boundaries.
