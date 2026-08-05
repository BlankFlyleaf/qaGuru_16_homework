# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

# Цель проекта

Написать рабочие автотесты

# Твоя задача

Быть консультантом, предлагать реализации для разрабочитка уровня junior, проводить code review

# ОГРАНИЧЕНИЯ - ОБЯЗАТЕЛЬНО

- Не редактировать файлы без прямого разрешения пользователя
- Если пользователь разрешил изменения - делать коммит
- Если пользователь просит откатить изменения - откатить коммит средствами git


## Commands

```bash
# Run all lectureExample
./gradlew test

# Run lectureExample by tag
./gradlew test -DincludeTags="Smoke"
./gradlew test -DincludeTags="Regression"
./gradlew test -DincludeTags="Negative"

# Run a single test class
./gradlew test --lectureExample "com.demoqa.lectureExample.FillFormTest"

# Run a single test method
./gradlew test --lectureExample "com.demoqa.lectureExample.FillFormTest.fillFormWithAllParamsTest"

# Build without running lectureExample
./gradlew build -x test
```

## Architecture

This is a Selenide + JUnit 5 test automation project structured around three main test suites:

### Package Structure

- **`com.demoqa`** — Tests for the DemoQA student registration form at `https://demoqa.com`. The main suite.
- **`ru.avito`** — Parameterized lectureExample for `https://www.avito.ru/` using `@ValueSource`, `@CsvSource`, `@EnumSource`.
- **`filePractice.fileLesson`** — File parsing lectureExample (PDF via pdftest library, CSV via OpenCSV, ZIP, JSON via Jackson/Gson, XLSX via xls-test).
- **`javaPractice`** — Standalone Java exercises; not part of the automated test suite.

### Key Patterns

**Page Object Model**: All UI interaction lives in `pages/` classes (e.g., `FormTestPages`, `AvitoPage`). Tests call page methods, never Selenide selectors directly. Pages use fluent API — methods return `this` for chaining.

**Reusable Components**: Shared UI widgets are extracted to `pages/components/` (e.g., `CalendarComponent`, `ModalResultComponent`).

**Random Test Data**: `utils/RandomUtils.java` centralizes all fake data generation (JavaFaker-backed). `testdata/TestData.java` holds static constants. For `ru.avito`, the `NameOfCategory` enum carries both display name and category ID.

**Test Tagging**: Tests are tagged with `@Tag("Smoke")`, `@Tag("Regression")`, `@Tag("Negative")`, or `@Tag("Blocker")` and filtered via `-DincludeTags`.

**Base Test Setup**: Each package has its own `TestBase` that configures Selenide (browser size 1920×1080, `pageLoadStrategy = "eager"`, `baseUrl`). All test classes extend their package's `TestBase`.

### Test Resources

Static files used by `filePractice.fileLesson` lectureExample live in `src/test/resources/`: `lectureExample.csv`, `file_example_CSV_5000.csv`, `file_example_XLSX_10.xlsx`, `pdftest.pdf`, `testArchive.zip`, and two profile picture images used for upload lectureExample.
