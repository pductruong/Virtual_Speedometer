# Virtual Speedometer - Project Breakdown Summary

## Quick Reference Guide

This document provides a high-level overview of the complete project breakdown. For detailed task descriptions, see:
- **PROJECT_BREAKDOWN.md** - Epics 1-3 (Infrastructure, BSW, RTE)
- **PROJECT_BREAKDOWN_PART2.md** - Epics 4-8 (Application Layer, UI)
- **PROJECT_BREAKDOWN_PART3.md** - Epics 9-11 (Configuration, Testing, Integration)

---

## Project Overview

**Project Name:** Virtual Speedometer - AUTOSAR Application Layer Simulation
**Total Estimated Effort:** ~74 person-days (3.5-4 months for single developer)
**Number of Epics:** 11
**Number of Stories:** ~35
**Number of Tasks:** ~120

---

## Epic Summary

| # | Epic | Stories | Effort | Key Deliverables |
|---|------|---------|--------|------------------|
| 1 | Core Infrastructure & Build System | 3 | 5 days | CMake build, directory structure, docs |
| 2 | Basic Software Layer (BSW) | 3 | 8 days | Scheduler, COM layer, config loader |
| 3 | Runtime Environment (RTE) | 3 | 10 days | S/R communication, C/S communication |
| 4 | Application Layer - Sensor & Mode | 2 | 6 days | WheelSpeedSensor, VehicleModeManager |
| 5 | Application Layer - Display & Safety | 3 | 6 days | SpeedDisplay, SpeedLimiter, WarningLight |
| 6 | Application Layer - Diagnostics | 1 | 5 days | DiagManager with DTC storage |
| 7 | User Interface - Terminal Mode | 1 | 5 days | ncurses-based dashboard |
| 8 | User Interface - Qt GUI Mode | 1 | 8 days | Qt graphical dashboard |
| 9 | Configuration Management | 3 | 6 days | XML/JSON config system |
| 10 | Testing & Quality Assurance | 4 | 10 days | Unit tests, integration tests, coverage |
| 11 | Integration & System Verification | 4 | 5 days | System integration, verification, release |

---

## Requirements Coverage Matrix

| Requirement Category | SRS IDs | Covered By |
|---------------------|---------|------------|
| Functional - WheelSpeedSensor | SRS-FUNC-001 to 007 | EPIC-4 Story 4.1 |
| Functional - VehicleModeManager | SRS-FUNC-010 to 016 | EPIC-4 Story 4.2 |
| Functional - SpeedDisplay | SRS-FUNC-020 to 025 | EPIC-5 Story 5.1 |
| Functional - SpeedLimiter | SRS-FUNC-030 to 037 | EPIC-5 Story 5.2 |
| Functional - WarningLight | SRS-FUNC-040 to 046 | EPIC-5 Story 5.3 |
| Functional - DiagManager | SRS-FUNC-050 to 058 | EPIC-6 Story 6.1 |
| Architecture Requirements | SRS-ARCH-001 to 015 | EPIC-1, 2, 3 |
| Interface Requirements | SRS-IF-001 to 035 | EPIC-2, EPIC-3 |
| UI Requirements - Terminal | SRS-UI-001 to 014 | EPIC-7 |
| UI Requirements - Qt GUI | SRS-UI-020 to 030 | EPIC-8 |
| UI Architecture | SRS-UI-040 to 045 | EPIC-7, EPIC-8 |
| Performance Requirements | SRS-PERF-001 to 012 | EPIC-2, 3, 7, 8 |
| Testing Requirements | SRS-TEST-001 to 014 | EPIC-10 |
| Build System Requirements | SRS-BUILD-001 to 015 | EPIC-1 |
| Configuration Requirements | SRS-CFG-001 to 014 | EPIC-9 |

**Total Requirements:** 134
**Coverage:** 100%

---

## Development Timeline

### Recommended 15-Week Schedule

**Weeks 1-3: Foundation Phase**
- EPIC-1: Core Infrastructure (1 week)
- EPIC-2: BSW Layer (2 weeks)
- **Checkpoint:** Scheduler operational

**Weeks 4-5: Communication Phase**
- EPIC-3: RTE Implementation (2 weeks)
- **Checkpoint:** S/R and C/S routing working

**Weeks 6-8: Application Phase**
- EPIC-4: Sensor & Mode Components (1.5 weeks)
- EPIC-5: Display & Safety Components (1 week)
- EPIC-6: Diagnostics (0.5 weeks)
- **Checkpoint:** All SW-Cs functional with unit tests

**Weeks 9-11: Interface Phase**
- EPIC-7: Terminal UI (1 week)
- EPIC-8: Qt GUI (2 weeks)
- **Checkpoint:** Both UIs operational

**Weeks 12-13: Configuration & Quality Phase**
- EPIC-9: Configuration Management (1 week)
- EPIC-10: Testing & QA (1 week)
- **Checkpoint:** Test coverage > 80%

**Weeks 14-15: Integration & Release Phase**
- EPIC-11: System Integration & Verification (2 weeks)
- **Checkpoint:** v1.0 Release ready

---

## Critical Path

The following epics must be completed in sequence (critical path):

```
EPIC-1 → EPIC-2 → EPIC-3 → EPIC-4 → EPIC-7 → EPIC-11
```

**Parallel Work Opportunities:**
- EPIC-5 and EPIC-6 can be developed in parallel after EPIC-3
- EPIC-8 can be developed in parallel with EPIC-9
- EPIC-10 testing can begin as soon as individual components are ready

---

## Technology Stack

### Languages
- **C11:** BSW layer (Scheduler, COM, RTE) - MISRA-C guidelines
- **C++17:** SW-Cs and UI layers - Modern C++ practices

### Build Tools
- **CMake 3.12+:** Build system
- **GCC 7+ / Clang 6+:** Compilers
- **Make / Ninja:** Build execution

### Libraries
- **ncurses:** Terminal UI (required)
- **Qt 5.12+:** Graphical UI (optional)
- **Google Test:** Unit testing framework (optional)
- **libxml2 / TinyXML:** XML parsing
- **cJSON:** JSON parsing

### Development Tools
- **Git:** Version control
- **cppcheck:** Static analysis
- **clang-tidy:** Code quality
- **gcov/lcov:** Code coverage
- **Doxygen:** API documentation (future)

---

## Key Architectural Decisions

### 1. Layered Architecture (AUTOSAR-Inspired)
- **Application Layer:** SW-Cs in C++ for business logic
- **RTE Layer:** Communication middleware in C
- **BSW Layer:** Scheduler, COM in C
- **UI Layer:** Decoupled presentation in C++

**Rationale:** Demonstrates AUTOSAR concepts, separates concerns

### 2. No Dynamic Memory in BSW/RTE
- All buffers statically allocated
- Fixed-size arrays for signals
- Complies with MISRA-C guidelines

**Rationale:** Automotive safety standards, predictable memory usage

### 3. Dual UI Modes
- Terminal mode (ncurses): Always available
- Qt GUI mode: Optional build flag

**Rationale:** Portability (terminal works everywhere), Qt provides polish

### 4. Configuration-Driven System
- XML for SW-C composition
- JSON for RTE routing

**Rationale:** Flexibility, demonstrates real-world automotive practice

### 5. Test-Driven Development
- Unit tests with mocked RTE
- Integration tests with real RTE
- Target: 80%+ code coverage

**Rationale:** Quality assurance, demonstrates professional practices

---

## Story Point Legend

| Points | Effort | Description |
|--------|--------|-------------|
| 1-2 | 0.5-1 day | Small task, clear requirements, low complexity |
| 3 | 1-2 days | Medium task, some complexity, straightforward implementation |
| 5 | 3-5 days | Large task, significant complexity, multiple sub-components |
| 8 | 6-8 days | Very large task, high complexity, potential unknowns |
| 13 | 9-13 days | Epic-level effort, requires breakdown into smaller stories |

**Note:** Story points are estimates. Actual effort may vary based on developer experience and unforeseen challenges.

---

## Priority Levels

| Priority | Meaning | Action |
|----------|---------|--------|
| **Critical** | Must-have for v1.0 | Implement first, no compromise |
| **High** | Important for complete system | Implement in phase 1 |
| **Medium** | Valuable but not essential | Implement if time permits |
| **Low** | Nice-to-have enhancement | Defer to post-v1.0 |

---

## Task Categories

Each task includes:
- **Effort:** Estimated time in days
- **Assignee:** TBD (to be determined during sprint planning)
- **Description:** What needs to be done
- **Technical Details:** Implementation guidance, code snippets
- **Requirements:** Linked SRS requirement IDs
- **Coding Standard:** Applicable standards (MISRA-C, C++17)
- **Verification:** How to validate completion

---

## Testing Strategy

### Unit Testing (EPIC-10)
- **Coverage Target:** 80%+
- **Framework:** Google Test
- **Approach:** Mocked RTE for SW-C isolation
- **Deliverables:** Test suite per component

### Integration Testing (EPIC-10)
- **Focus:** RTE routing, scheduler behavior
- **Approach:** Real RTE with multiple components
- **Deliverables:** End-to-end data flow tests

### System Testing (EPIC-11)
- **Focus:** Complete user scenarios
- **Approach:** Manual testing with both UIs
- **Deliverables:** Demo scenarios, verification report

### Static Analysis (EPIC-10)
- **Tools:** cppcheck, clang-tidy
- **Standards:** MISRA-C:2012 for BSW/RTE
- **Deliverables:** Analysis reports, deviation justifications

---

## Documentation Deliverables

| Document | Epic | Status | Purpose |
|----------|------|--------|---------|
| README.md | EPIC-1, 11 | To be completed | Quick start, build instructions |
| SRS.md | N/A | ✅ Complete | Requirements specification |
| ARCHITECTURE.md | EPIC-11 | To be created | System design documentation |
| API_REFERENCE.md | EPIC-11 | To be created | API documentation |
| AUTOSAR_CONCEPTS.md | EPIC-1 | To be created | Educational content |
| CONTRIBUTING.md | EPIC-1 | To be created | Contribution guidelines |
| PROJECT_BREAKDOWN.md | N/A | ✅ Complete | This work item breakdown |
| Verification Report | EPIC-11 | To be created | Requirements verification |
| Release Notes | EPIC-11 | To be created | v1.0 release documentation |

---

## Quality Gates

### Epic Completion Criteria
- [ ] All stories completed
- [ ] All tasks verified
- [ ] Unit tests passing (if applicable)
- [ ] Code reviewed
- [ ] Documentation updated

### Story Completion Criteria
- [ ] All acceptance criteria met
- [ ] All tasks completed
- [ ] Tests written and passing
- [ ] Code reviewed and merged

### Task Completion Criteria
- [ ] Implementation complete
- [ ] Verification method executed
- [ ] Code committed to version control
- [ ] Documentation updated (if applicable)

---

## Risk Mitigation Strategies

| Risk | Mitigation |
|------|------------|
| **Scope Creep** | Stick to SRS requirements, defer enhancements to v2.0 |
| **RTE Complexity** | Start with simple S/R, iterate to full functionality |
| **Integration Issues** | Regular integration checkpoints, interface contracts |
| **Testing Delays** | Write tests alongside code (TDD), not at end |
| **Qt Availability** | Terminal UI is mandatory, Qt is optional |
| **Schedule Overrun** | Prioritize Critical/High items, defer Medium/Low |

---

## Post-v1.0 Enhancement Ideas

**Future Epics (v2.0):**
- EPIC-12: NvM Persistence Layer (DTC storage to file)
- EPIC-13: Advanced Scheduler (true preemption, multi-core)
- EPIC-14: CAN Frame Visualization
- EPIC-15: UDS Diagnostic Protocol Simulation
- EPIC-16: Multi-ECU Simulation (virtual CAN bus)
- EPIC-17: Configuration GUI Tool
- EPIC-18: Real-time Performance Optimization

---

## How to Use This Breakdown

### For Project Managers:
1. Import epics into project management tool (Notion, Jira, Azure DevOps)
2. Assign stories to sprints based on timeline
3. Track progress using story points
4. Monitor critical path and dependencies

### For Developers:
1. Pick tasks from current sprint backlog
2. Follow technical details and coding standards
3. Write tests as specified in verification section
4. Update task status upon completion

### For QA Engineers:
1. Review acceptance criteria for each story
2. Plan test cases based on verification methods
3. Track test coverage and quality metrics
4. Execute system verification in EPIC-11

### For Technical Leads:
1. Review architectural decisions
2. Conduct code reviews per coding standards
3. Ensure dependencies are respected
4. Guide team through technical challenges

---

## Contact & Support

For questions about this breakdown:
- Refer to **SRS.md** for detailed requirements
- See individual epic documents for task details
- Consult **ARCHITECTURE.md** (when available) for design context

---

**Document Version:** 1.0
**Last Updated:** 2026-02-16
**Maintained By:** Development Team

---

## Change Log

| Version | Date | Changes | Author |
|---------|------|---------|--------|
| 1.0 | 2026-02-16 | Initial breakdown based on SRS v1.0 | Claude Sonnet 4.5 |

---

**Ready to start development? Begin with EPIC-1: Core Infrastructure & Build System!**
