# Cyber Security Projects

Welcome to the Cyber Security Projects repository! This repository hosts a collection of utilities, extensions, and engines designed to defend against modern web threats, credential theft, and privacy leaks.

## Project 1: PhishShield - Advanced Phishing & Credential Guard Extension
An intelligent, privacy-first Manifest V3 browser extension built to detect phishing, combosquatting, and typosquatting attacks in real-time, block credential leaks over insecure connections, and provide deep site threat intelligence.

---

### 1. Overview: Why Do We Need This Extension?
Phishing attacks remain the most common entry point for credential compromise and security breaches. Modern attackers utilize sophisticated tactics such as:
* **Typosquatting:** Creating lookalike domains (e.g., `paypa1.com` instead of `paypal.com`) to exploit user visual errors.
* **Combosquatting:** Combining legitimate brand names with security or utility terms (e.g., `paypal-security.com`).
* **Credential Harvesting:** Intercepting logins and passwords on unencrypted HTTP connections or fake forms.

Standard web browsers rely on static, centralized threat lists. By the time a threat domain is categorized and pushed to users' browsers, credentials may already be leaked. **PhishShield** addresses this gap by combining **real-time local heuristics**, **privacy-preserving lookup protocols**, and **active DOM interceptors** to block leaks *before* submissions can take place.

---

### 2. Features of the Extension
* **Real-time Typosquatting & Combosquatting Heuristics:** Uses a localized Levenshtein distance algorithm to inspect active hostnames against a list of the top 100 most targeted brands.
* **Privacy-Preserving k-Anonymity Threat Intelligence Lookup:** Queries reputation records securely. The URL is hashed locally using SHA-256, and only the first 5 hex characters of the hash prefix are sent to the API. This guarantees that the remote lookup server never learns the exact websites the user visits.
* **Active Credential Guard:** Intercepts sensitive form submissions (passwords, credit cards) on insecure (HTTP) or high-risk domains. Displays an isolated, warning dialog inside a secure **Shadow DOM** to prevent the page's scripts from bypassing or reading the warning.
* **Dynamic NetRequest Blocking:** Integrates with Chrome’s `declarativeNetRequest` engine to instantly block main-frame navigation to confirmed malicious domains once detected or reported.
* **Resource-Optimized Tab Analysis Caching:** Includes a local tab analysis cache that prevents redundant calculations (hashing, Levenshtein checks, API requests) on tab switches or popup opens, ensuring minimal CPU/battery consumption.
* **Modern Premium Dashboard Popup:** A stunning glassmorphic, space-themed dark mode popup summarizing trust scores, connection security (SSL/TLS), hosting location, network ASN details, registration age, and an outline pill button to report threats.

---

### 3. Technological Aspects & Tech Stack
To comply with modern browser standards and strict Content Security Policies (CSP), PhishShield is built entirely on native web APIs with **zero external dependencies**:
* **Manifest V3 Core:** Complies with modern extension architecture requirements, using background service workers and declarative rulesets instead of persistent, resource-heavy V2 background pages.
* **Web Crypto API (`crypto.subtle`):** Performs native SHA-256 cryptographic hashing locally on the client's thread.
* **Vanilla CSS (Modern Layouts):** Features visual elements (glassmorphism, staggered CSS transitions, pulsing glows) designed directly in vanilla CSS. No remote CDNs are loaded, ensuring absolute compliance with security constraints.
* **Shadow DOM API:** Inserts the security warning card in an isolated shadow tree, shielding the modal's styles and scripts from the host website's stylesheets and scripts.
* **Chrome declarativeNetRequest & Storage APIs:** Handles blocklists and telemetry parameters asynchronously.

---

### 4. How I Built It: The Process
1. **Architecture & Sandbox Research:** Investigated Manifest V3 rulesets, specifically looking at how to do secure imports and CSP compliance without loading external libraries.
2. **Developing the Heuristic Engine:** Created the typosquatting engine in `lib/typosquat_engine.js` using Levenshtein distance computations with short-brand length constraints to prevent false positives.
3. **Establishing k-Anonymity Lookup:** Implemented local SHA-256 hashing and prefix-masking fetches to enable private URL checks.
4. **Form Interception & Security Shield:** Developed content scripts to hook submit events, verify tab risk status, and show an isolated Shadow DOM warning block when credentials are at risk.
5. **Dashboard Polish:** Engineered a modern, side-by-side dashboard UI (Score Circle + Status details), matching exact button paddings, implementing staggered CSS transitions, and removing double-border window leakage to match the Chrome window container.
6. **Performance Optimization:** Integrated an active cache on the background worker to serve cached results during tab switches, cutting down redundant CPU overhead.

---

### 5. How to Use & Install It
#### Prerequisites
* Any Chromium-based browser (Google Chrome, Brave, Microsoft Edge, Opera).

#### Installation Steps
1. Download or clone this repository to your local machine.
2. Open your browser and navigate to `chrome://extensions/`.
3. Enable **Developer Mode** using the toggle switch in the top-right corner.
4. Click the **Load unpacked** button in the top-left corner.
5. Select the folder: `Cyber-Security-Projects/browser extension for phishing detection`.
