# Multi-Site Active Directory Security Design

**Source:** Coursework-derived case study, independently rewritten  
**Status:** Complete design case study  
**Environment:** Fictionalized for safe publication

## Scenario

A fictional healthcare organization operates three locations and needs centralized identity, resilient authentication, consistent endpoint security, and a recovery strategy that accounts for both outages and ransomware.

The original coursework included topology planning, organizational-unit design, multi-site resilience, Group Policy planning, implementation evidence, and backup recommendations. This case study preserves the technical decisions without reproducing the assignment, school template, identities, hostnames, or network values.

## Objectives

- Design a routed, multi-site Windows domain using documentation-safe network ranges
- Organize users and computers around administrative and policy boundaries
- Apply layered security controls through Group Policy
- Reduce authentication and directory-service single points of failure
- Define recovery objectives and immutable backup requirements

## Proposed architecture

Each site receives a domain controller and a dedicated subnet. The design uses example ranges reserved for documentation:

| Site | Example subnet | Directory role |
|---|---|---|
| Headquarters | `192.0.2.0/24` | Primary directory and DNS services |
| Clinic East | `198.51.100.0/24` | Local authentication and replicated DNS |
| Clinic West | `203.0.113.0/24` | Local authentication and replicated DNS |

The design avoids publishing the original lab's internal addresses, names, or screenshots.

## Identity and OU design

The directory is organized by administrative responsibility rather than mirroring an organization chart too literally. Separate OUs support:

- Departmental users
- Managed workstations
- Servers
- Privileged administrators
- Service accounts
- Test objects used to validate policy changes

Security groups represent access roles. Delegation can then be scoped to the smallest useful OU, reducing reliance on highly privileged domain-wide roles.

## Layered Group Policy strategy

The coursework design demonstrated a layered policy model:

1. Domain-level authentication baselines, including password and lockout controls
2. Workstation hardening, firewall configuration, application control, update management, and removable-storage restrictions
3. Department-specific user settings and approved resource mappings
4. Separate administrative policies for privileged workstations and accounts

Exact policy values belong to an organization's approved security baseline and risk requirements. A lab value is not automatically a production recommendation.

## Resilience and recovery

Replication across sites improves availability, but replication alone does not protect against malicious deletion or ransomware because harmful changes can also replicate. The recovery design therefore separates:

- Directory replication for service continuity
- Application-aware backup for recoverability
- Immutable off-site copies for ransomware resistance
- Documented restoration tests for directory objects and critical services

Recovery point and recovery time objectives must be assigned by business impact rather than assumed from the technology.

## Validation matrix

| Requirement | Validation approach | Evidence expected |
|---|---|---|
| Local authentication | Locate a domain controller from a client at each site | Client selects the intended site-local controller |
| DNS health | Resolve AD service records from each subnet | Correct directory-service records and reachable DNS servers |
| Replication | Review `repadmin /replsummary` and directory-service events | No unexplained replication failures |
| Delegation | Use a nonprivileged test operator | Permitted OU tasks succeed; out-of-scope tasks fail |
| Group Policy | Run `gpresult /h` from a managed client | Intended policies apply without unexpected inheritance |
| Recovery | Delete and restore a fictional test object | Object and required attributes are recovered |

## Skills demonstrated

AD DS, DNS, multi-site topology, OU design, security groups, delegated administration, Group Policy, resilience planning, ransomware-aware backup design, and technical documentation.

## Publication safeguards

No original school files or screenshots are included. All names, addresses, domains, accounts, and network values are fictionalized. New screenshots will be captured only from an independently rebuilt lab.
