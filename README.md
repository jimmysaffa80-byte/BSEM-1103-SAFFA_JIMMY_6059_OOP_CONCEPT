# Student Academic Performance Monitoring System

A console-based Python OOP learning project for managing secondary-school
students, subject offerings, teacher assignments, examinations, results, and
academic reports. The project is an educational prototype, not production
school software.

## Before modifications

The provided workspace was empty: it had no existing source, tests,
documentation, dependency manifest, or Git repository. The workspace did not
have a `python` command available during inspection; use the Windows `py`
launcher if a Python runtime is installed.

## Design and implementation plan

1. Model users, students, teachers, administrators, subjects, examinations,
   and individual examination results.
2. Store records behind a repository abstraction with an in-memory first
   implementation.
3. Put validation, authentication, permissions, academic analysis, and report
   generation in services separate from the console interface.
4. Connect role-specific console menus to those services.
5. Verify model, service, access-control, and analysis behavior with
   `unittest`.

The implementation follows this plan. The primary relationships are:
students have many examination results; an examination identifies one subject,
class, and academic period; a teacher has one or more subject/class
assignments; and each registered user is both their domain record and login
identity.

## Project structure

```text
main.py
student_academic_performance/
    academics.py
    analysis.py
    authentication.py
    console.py
    errors.py
    models.py
    reports.py
    repositories.py
tests/
    test_system.py
requirements.txt
```

## OOP concepts in the implementation

- **Encapsulation:** `ExamResult.update_score` validates a score before
  changing it; `Student.update_profile` validates every requested change before
  saving any of them.
- **Inheritance and polymorphism:** `Student`, `Teacher`, and `Administrator`
  inherit the abstract `User` type and override `can_access_student`. Services
  call that shared operation to enforce each role's different access rules.
- **Abstraction:** services use the `Repository` interface. The current
  `InMemoryRepository` can later be replaced with a persistent implementation.
- **Composition:** services receive repositories through `InMemoryStore`
  rather than maintaining unrelated copies of student and account data.

## Setup and usage

Install Python 3.10 or newer. No third-party packages are required. On Windows,
check that a runtime is available with `py --version`, then start the app:

```powershell
py main.py
```

On first launch, create the initial administrator username, name, and
password. Passwords must be at least eight characters and are stored as
PBKDF2-HMAC-SHA256 hashes. No demonstration or default credentials are
included. An administrator can register, update, search, and remove eligible
students; register and update teachers; add, update, or deactivate subjects;
assign teachers; create examinations; record or correct results; and view
reports. Teachers are limited to their assigned subjects and classes; students
can view only their own profile and report.

The administrator should add subjects and assign teachers before creating
examinations. Scores use a maximum of 100 by default, configurable per
examination. The default grade scale is configurable in `GradeCalculator`.

## Example terminal screenshot

The following illustrative screenshot shows a sample administrator opening an
academic report. It is rendered from a run of the application's console and
report code using temporary, fictional in-memory records. The sample account
and student are **not** preloaded or included in the application; the screenshot
is an example, not a live capture from a user's terminal.

![Illustrative terminal output showing a student academic report](docs/terminal-report-example.svg)

## Tests

Run all automated tests from the project root:

```powershell
py -m unittest discover -v
```

Tests cover registration and validation, repositories, subject offerings,
examination/result management, score updates, grade boundaries, normalized
analysis, academic support indicators, reports, password hashing, and
role-based permissions.

## Validation and limitations

- Supported school classes are JSS 1–3 and SSS 1–3; student ages are limited
  to 10–25.
- Grades use the example scale in the project brief; it is not asserted to be
  any official grading policy.
- Support is recommended only when the average is below the configured 50%
  threshold across at least two recorded assessments. One low mark is reported
  as a reason to monitor, not as a consistent pattern.
- Subject comparisons and period changes normalize each mark against its
  examination's maximum. Missing results are excluded, never treated as zero.
- Deactivated subjects and students with academic history are protected to
  preserve historical records.
- All data is held in memory and disappears when the program exits. Do not use
  this prototype to store real student data or treat it as production-ready.
- The first version has no persistent database, account recovery, password
  change flow, or audit trail.
- Passwords are read without echo in an interactive terminal; automated
  non-interactive setup is not provided.

## Recommended next phase

Add persistence behind the repository interface, beginning with a carefully
reviewed schema and migration plan. Before handling real student information,
add a threat model, privacy controls, audit logging, backup/recovery procedures,
and a school-approved grading policy.
# BSEM-1103-SAFFA_JIMMY_6059_OOP_CONCEPT
