# GitHub Actions Test Workflow Documentation

## Overzicht
Dit document beschrijft de CI/CD test workflow (`test.yml`) van het Zachte Heelmeesters Portaal project.

## Workflow Trigger
De workflow wordt geactiveerd op:
- **Push**: Alle pushes naar elke branch
- **Pull Request**: Pull requests naar de `master` en `staging` branches

## Jobs

### 1. Frontend Tests (Vue.js)
**Naam**: `frontend-tests`  
**Platform**: Ubuntu Latest

#### Stappen:
1. **Checkout code** - Klont de repository
2. **Setup Node.js** - Installeert Node.js versie 22
   - Gebruikt npm caching voor snellere installs
3. **Install dependencies** - Installeert npm packages via `npm ci`
4. **Run ESLint** - Voert linting uit (niet-kritisch, continue on error)
5. **Run frontend tests** - Voert Vitest tests uit met JSON reporting
   - Voert tests uit met `npm run test -- --run`
   - Genereert test-results.json
   - Maakt coverage reports
6. **Upload test results** - Slaat test-results.json op als artifact

---

### 2. Backend Tests (.NET)
**Naam**: `backend-tests`  
**Platform**: Ubuntu Latest

#### Stappen:
1. **Checkout code** - Klont de repository
2. **Start Docker containers** - Start SQL Server via docker-compose
3. **Wait for SQL Server** - Wacht tot SQL Server bereikbaar is (max 30 pogingen, 5 sec intervals)
4. **Setup .NET** - Installeert .NET 8.0.x
   - Gebruikt dependency caching
5. **Restore dependencies** - Voert `dotnet restore` uit
6. **Build backend** - Bouwt de applicatie in Debug modus
7. **Run backend tests** - Voert xUnit tests uit
   - Genereert TRX (Test Results XML) bestanden
   - Verzamelt XPlat Code Coverage
   - Environment: Testing
8. **Upload test results** - Slaat TRX bestanden op als artifact
9. **Stop Docker containers** - Schakelt Docker containers uit (altijd uitgevoerd)
10. **Upload coverage reports** - Stuurt coverage data naar Codecov

---

### 3. Test Summary
**Naam**: `test-summary`  
**Platform**: Ubuntu Latest  
**Afhankelijkheden**: Wacht op `frontend-tests` en `backend-tests`

#### Stappen:
1. **Checkout code** - Klont de repository
2. **Download backend test results** - Haalt TRX bestanden op
3. **Download frontend test results** - Haalt test-results.json op
4. **Parse and display test results** - Genereert een samenvatting in GitHub Actions summary
   - Toont frontend test status en statistieken
   - Toont backend test status en statistieken
   - Telt test files per project
   - Geeft overall summary
5. **Fail if tests failed** - Mislukt workflow als frontend of backend tests faalden

---

## Test Results Output

### Frontend Test Summary
- **Status**: ✅ Passed / ❌ Failed
- **Metrics**:
  - 📁 Test Files (aantal .spec.ts/.spec.js/.test.ts/.test.js files)
  - 🧪 Total Tests
  - ✅ Passed
  - ❌ Failed

### Backend Test Summary
- **Status**: ✅ Passed / ❌ Failed
- **Metrics**:
  - 📁 Test Files (aantal *Tests.cs files)
  - 🧪 Total Tests
  - ✅ Passed
  - ❌ Failed
  - ⏭️ Skipped

### Overall Summary
- Total aantal test files (frontend + backend)
- Eindresultaat: ✅ All tests passed / ❌ Some tests failed

---

## Environment Variabelen

| Variabele | Waarde | Gebruikt in |
|-----------|--------|------------|
| `VITEST_COVERAGE` | true | Frontend tests |
| `ASPNETCORE_ENVIRONMENT` | Testing | Backend tests |

---

## Artifacts

| Artifact | Locatie | Beschrijving |
|----------|---------|-------------|
| `frontend-test-results` | zachteheelmeestersportaal.client/test-results.json | Frontend test resultaten in JSON formaat |
| `backend-test-results` | ZachteHeelmeestersPortaal.Tests/TestResults/ | Backend test resultaten in TRX formaat |

---

## Codecov Integration

Backend coverage reports worden automatisch geüpload naar Codecov:
- **File**: coverage.cobertura.xml
- **Flag**: backend
- **Name**: backend-coverage

---

## Best Practices in deze Workflow

1. ✅ **Caching**: Node.js packages en .NET dependencies worden gecached voor snellere builds
2. ✅ **Parallelisatie**: Frontend en backend tests draaien parallel
3. ✅ **Error Handling**: ESLint en Docker shutdown zijn non-blocking (continue-on-error/if: always)
4. ✅ **Logging**: Uitgebreide logging voor debugging
5. ✅ **Code Coverage**: Automatische coverage rapportage naar Codecov
6. ✅ **Health Checks**: Wacht op SQL Server voordat tests starten
7. ✅ **Artifact Management**: Test resultaten worden opgeslagen voor analyse

---

## Workflow Status Badge

Om een status badge aan te kunnen maken, voeg je dit toe aan je README.md:

```markdown
![Tests](https://github.com/<owner>/<repo>/actions/workflows/test.yml/badge.svg)
```

---

*Gegenereerd op basis van `.github/workflows/test.yml`*

