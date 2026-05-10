## ADDED Requirements

### Requirement: Repository initialization
The system SHALL provide a dedicated `pearl-infra` repository for Pearl ecosystem infrastructure with clear ownership, scope, and setup instructions.

#### Scenario: Developer opens repository
- **WHEN** a developer reads the repository README
- **THEN** they can identify the repo purpose, upstream Pearl reference, planned modules, local setup path, and current implementation phase

### Requirement: Upstream source manifest
The repository SHALL document every upstream Pearl component it references, vendors, submodules, forks, or wraps.

#### Scenario: Upstream dependency is added
- **WHEN** a Pearl upstream component is added to the repo
- **THEN** the manifest records source URL, commit or version, copied paths if any, license notes, reason for inclusion, and update procedure

### Requirement: Goal-based task metadata
Every implementation task SHALL include success criteria, applicable skill(s), verification command(s), and failure escalation notes.

#### Scenario: Task is marked complete
- **WHEN** an implementation task is marked complete
- **THEN** the task includes evidence that the deliverable exists and the declared verification passed
