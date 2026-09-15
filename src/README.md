# Source Code

#include <iostream>
#include <vector>
#include <string>
#include <iomanip>
using namespace std;

struct Shipment {
    string id;
    string destination;
    string disruption;
    int delayHours;
    double temperature;
    int riskScore;
};

int calculateRisk(int delay, double temperature, string disruption) {
    int score = 0;

    // Delay risk
    if (delay >= 48)
        score += 40;
    else if (delay >= 24)
        score += 30;
    else if (delay >= 12)
        score += 20;
    else
        score += 5;

    // Temperature risk
    if (temperature > 10)
        score += 30;
    else if (temperature > 5)
        score += 15;

    // Disruption risk
    if (disruption == "Port Strike")
        score += 20;
    else if (disruption == "Storm")
        score += 15;
    else if (disruption == "Road Block")
        score += 15;
    else if (disruption == "Carrier Delay")
        score += 10;

    return score;
}

string getRiskLevel(int score) {
    if (score >= 70)
        return "HIGH";
    else if (score >= 40)
        return "MEDIUM";
    else
        return "LOW";
}

void recommendation(int score, string disruption) {

    cout << "Recommendation: ";

    if (score >= 70) {
        cout << "URGENT - Reroute shipment and contact alternate carrier.\n";
    }
    else if (score >= 40) {
        cout << "Monitor shipment and consider an alternate route.\n";
    }
    else {
        cout << "Continue current route and monitor normally.\n";
    }

    if (disruption == "Port Strike") {
        cout << "Action: Check nearby ports for alternative routing.\n";
    }
    else if (disruption == "Storm") {
        cout << "Action: Avoid affected weather zone.\n";
    }
    else if (disruption == "Road Block") {
        cout << "Action: Select an alternate road route.\n";
    }
}

int main() {

    vector<Shipment> shipments;

    int n;

    cout << "=============================================\n";
    cout << "              SHIPGUARD AI\n";
    cout << "   Supply Chain Disruption Assistant\n";
    cout << "=============================================\n\n";

    cout << "How many shipments do you want to analyze? ";
    cin >> n;

    for (int i = 0; i < n; i++) {

        Shipment s;

        cout << "\n========== Shipment " << i + 1 << " ==========\n";

        cout << "Shipment ID: ";
        cin >> s.id;

        cout << "Destination: ";
        cin >> s.destination;

        cout << "Disruption type:\n";
        cout << "1. Port Strike\n";
        cout << "2. Storm\n";
        cout << "3. Road Block\n";
        cout << "4. Carrier Delay\n";
        cout << "5. None\n";

        int choice;
        cout << "Choose: ";
        cin >> choice;

        switch (choice) {
            case 1:
                s.disruption = "Port Strike";
                break;
            case 2:
                s.disruption = "Storm";
                break;
            case 3:
                s.disruption = "Road Block";
                break;
            case 4:
                s.disruption = "Carrier Delay";
                break;
            default:
                s.disruption = "None";
        }

        cout << "Expected delay (hours): ";
        cin >> s.delayHours;

        cout << "Current temperature (C): ";
        cin >> s.temperature;

        s.riskScore = calculateRisk(
            s.delayHours,
            s.temperature,
            s.disruption
        );

        shipments.push_back(s);
    }

    cout << "\n\n=============================================\n";
    cout << "             SHIPMENT ANALYSIS\n";
    cout << "=============================================\n";

    for (const auto &s : shipments) {

        cout << "\nShipment ID   : " << s.id;
        cout << "\nDestination    : " << s.destination;
        cout << "\nDisruption     : " << s.disruption;
        cout << "\nDelay          : " << s.delayHours << " hours";
        cout << "\nTemperature    : " << s.temperature << " C";
        cout << "\nRisk Score     : " << s.riskScore << "/100";
        cout << "\nRisk Level     : " << getRiskLevel(s.riskScore) << "\n";

        recommendation(s.riskScore, s.disruption);

        cout << "---------------------------------------------\n";
    }

    cout << "\n=============================================\n";
    cout << "           AI ANALYSIS COMPLETE\n";
    cout << "=============================================\n";

    return 0;
}

## Structure Guidelines

Organize your code logically. Here are common patterns — use whatever fits
your project:

### Web Application
```
src/
  backend/        ← API server code
  frontend/       ← UI code
  shared/         ← Shared utilities/types
```

 ### Data / AI Project
```
src/
  data/           ← Data ingestion / preprocessing
  models/         ← ML model code
  api/            ← Serving layer
  notebooks/      ← Jupyter notebooks (exploration)
```

### CLI / Script-based Tool
```
src/
  cli/            ← CLI entry points
  lib/            ← Core logic
  utils/          ← Helpers
```

## Important Files to Include

- `requirements.txt` or `package.json` — dependency manifest
- `.env.example` — template for environment variables (NEVER commit `.env`)
- Any database migration files
- Configuration files

## What NOT to Include in src/

- `.env` files with real secrets
- Large binary files (use Git LFS or link externally)
- `node_modules/` or `venv/` (these are in `.gitignore`)
- Build artifacts (`dist/`, `build/`, `__pycache__/`)
