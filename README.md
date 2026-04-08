# Historical STEM Figures Database

A normalised relational database of historical women in STEM, built as part of a university Databases module assessment. The schema is in **Third Normal Form (3NF)** and includes a full booking/events system for lectures and talks about these figures.

---

## Contents

| File | Description |
|------|-------------|
| `stem_database.sql` | Full database creation, population, and query script |
| `assessment2_report.docx` | Supporting documentation and reflection |

---

## Database Schema

13 tables across three groups:

### Reference tables
- **Country** — each country stored once, referenced by FK
- **Field** — normalised fields of study (removes repeating groups from scientist data)
- **Organisation** — employers and institutions

### Core tables
- **Scientist** — biographical data; all attributes depend solely on `scientist_id` (3NF)
- **Scientist_Field** — many-to-many junction: scientist ↔ field
- **Scientist_Organisation** — many-to-many junction: scientist ↔ organisation

### Events / booking system
- **Event** — core event record with `requires_ticket` flag
- **Speaker** — people who deliver talks
- **Room** — venues with type discriminator (`Lecture` or `Event`)
- **Ticket** — separated from Event so free events carry no NULL columns
- **Event_Room** — many-to-many junction: event ↔ room
- **Event_Speaker** — many-to-many junction: event ↔ speaker
- **Event_HistoricalFigure** — many-to-many junction: event ↔ scientist topic

---

## Scientists Included

26 historical figures across 12 countries:

| Country | Scientists |
|---------|-----------|
| Britain | Ada Lovelace, Mary Cartwright, Rosalind Franklin, Hertha Ayrton, Dorothy Hodgkin |
| USA | Grace Hopper, Stephanie Kwolek, Katherine Johnson, Mary Jackson |
| France | Émilie du Châtelet, Marie-Sophie Germain, Irène Joliot-Curie, Yvonne Choquet-Bruhat |
| Germany | Emmy Noether, Ida Noddack, Maria Goeppert Mayer |
| Russia | Julia Lermontova, Sofya Kovalevskaya |
| Sweden | Sophia Brahe, Maria Christina Bruhn |
| Egypt | Hypatia Alexandria |
| Iran | Maryam Mirzakhani |
| Italy | Maria Gaetana Agnesi |
| Japan | Fumiko Yonezawa |
| Poland | Marie Skłodowska Curie |
| Serbia | Mileva Marić |

> Plus **Cecilia Payne-Gaposchkin** added as part of Query 13.

---

## SQL Queries

The script includes 13 queries covering:

1. List all figures born in France
2. List all figures who contributed to Computing
3. List figures contributing to more than one field
4. Correct Mary Jackson's death year to 2005 (`UPDATE`)
5. List figures who died before age 40
6. Countries with the most historical figures
7. Historical figures still alive
8. Figures who worked for NASA
9. Figures who worked with Einstein
10. Countries with the most distinct research fields
11. Add an event fulfilling all booking requirements
12. Amend an event date (`UPDATE`)
13. Add a new scientist (Cecilia Payne-Gaposchkin)

---

## Getting Started

### Requirements

- MySQL 8.0+ or MariaDB 10.5+

### Run the script

```bash
mysql -u your_username -p your_database < stem_database.sql
```

Or paste the contents of `stem_database.sql` directly into a MySQL client such as MySQL Workbench, DBeaver, or phpMyAdmin.

The script is idempotent — it drops and recreates all tables on each run, so it is safe to execute multiple times.

---

## Normalisation

The database was designed to achieve **Third Normal Form (3NF)**:

- **1NF** — the comma-separated `Main Area` field from the source spreadsheet was split into atomic rows in the `Scientist_Field` junction table
- **2NF** — all non-key attributes depend on the whole primary key (composite PKs used on junction tables)
- **3NF** — transitive dependencies removed by extracting `Country`, `Field`, and `Organisation` into their own tables

---

## Data Source

Biographical data sourced from `HistoricalSTEM.xlsx`, provided as part of the assessment brief.
