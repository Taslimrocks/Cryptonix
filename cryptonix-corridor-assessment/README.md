<div align="center">

# CRYPTONIX SYSTEMS

![Cybersecurity banner](cryptonix-corridor-assessment/assets/cybersecurity-banner.jpg)

# Web Security Assessment

### TryHackMe — Corridor

**An Experimental Framework for Object Reference Manipulation and Kernel Exploitation**

[![Status](https://img.shields.io/badge/Status-Completed-brightgreen)](https://tryhackme.com/room/corridor)
[![Platform](https://img.shields.io/badge/Platform-TryHackMe-red)](https://tryhackme.com/p/taslimbano324)
[![Focus](https://img.shields.io/badge/Focus-Web%20Security-blue)](https://github.com/)

**Prepared by:** Taslim Bano
**Assessment Date:** 29 October 2026
**Environment:** TryHackMe AttackBox
**Status:** Challenge Completed

</div>

---

## 1. Overview

This repository documents my investigation of the TryHackMe Corridor room, a web security challenge focused on discovering resources that are not immediately apparent through the application's visual interface.

I examined the application's corridor image, inspected its HTML and URL patterns, extracted the available door references, and investigated how hexadecimal-looking URL components were generated.

By calculating MD5 hashes of candidate numeric values and comparing them against the references present in the application, I identified an endpoint that was not represented by a visible door. Accessing the relevant endpoint revealed the challenge flag, and I successfully completed the room.

This assessment documents my methodology, observations, technical findings, and supporting evidence.

## 2. Assessment Objectives

The objectives of this exercise were to:

* Examine the web application's structure and navigation.
* Identify URL patterns associated with individual corridor doors.
* Extract and analyze the references embedded in the application's HTML.
* Investigate the relationship between numeric inputs and MD5 hash values.
* Identify an endpoint that was not represented by the visible door references.
* Retrieve the challenge flag and verify successful completion.
* Document the investigation with reproducible steps and screenshots.

## 3. Scope and Environment

| Item                | Details                                                     |
| ------------------- | ----------------------------------------------------------- |
| Target              | TryHackMe Corridor                                          |
| Target environment  | Authorized TryHackMe challenge                              |
| Testing environment | TryHackMe AttackBox                                         |
| Application address | Assigned challenge IP address                               |
| Primary focus       | URL reference analysis and endpoint discovery               |
| Tools               | Firefox Developer Tools, Linux terminal, `md5sum`, Python 3 |
| Final result        | Challenge completed; flag retrieved                         |

Testing was limited to the assigned challenge environment. The findings describe the behavior observed in this lab and should not be interpreted as evidence of vulnerabilities in unrelated systems.

## 4. Methodology

### 4.1 Initial Application Inspection

I opened the Corridor application and examined the displayed hallway and its door navigation.

I then inspected the page using Firefox Developer Tools to understand how the application represented its navigation endpoints.

**Evidence:**

* [E01 — Initial Corridor View](cryptonix-corridor-assessment/evidence/E01-initial-corridor-view.png)
* [E02 — Initial Corridor Inspection](cryptonix-corridor-assessment/evidence/E02-initial-corridor-view.png)

### 4.2 URL Reference Extraction

I inspected the application's HTML and extracted the `href` values associated with the available links.

The extracted references included hexadecimal strings that appeared to be hash values. I recorded the references and compared them with candidate values generated from numeric inputs.

**Evidence:**

* [E03 — Extracted Door Hashes](cryptonix-corridor-assessment/evidence/E03-extracted-door-hashes.png)

### 4.3 Hash Calculation and Comparison

I used Linux command-line utilities and Python to calculate MD5 hashes for candidate numeric values.

For example, the following commands calculate the MD5 hashes of the strings `0` and `1`:

```bash
printf '0' | md5sum
printf '1' | md5sum
```

I compared the resulting hashes against the extracted URL references.

This helped me investigate the relationship between numeric values and the application's URL structure.

**Evidence:**

* [E04 — MD5 Calculation for Zero and One](cryptonix-corridor-assessment/evidence/E04-md5-calculation-zero-and-one.png)
* [E05 — Candidate Index Comparison](cryptonix-corridor-assessment/evidence/E05-candidate-index-comparison.png)

### 4.4 Network Inspection

I used Firefox Developer Tools' Network panel to inspect requests made to the application.

I examined request URLs, HTTP response statuses, and relevant response headers. I also observed a `404 Not Found` response for the favicon request.

The favicon response was recorded as supporting evidence of the application's observed network behavior; it was not treated as proof of the primary finding.

**Evidence:**

* [E07 — Network Favicon 404](cryptonix-corridor-assessment/evidence/E07-network-favicon-404.png)
* [E08 — Network Door Request Summary](cryptonix-corridor-assessment/evidence/E08-network-door-request-summary.png)
* [E09 — Network Door Request Details](cryptonix-corridor-assessment/evidence/E09-network-door-request-details.png)
* [E10 — Expanded Network Response](cryptonix-corridor-assessment/evidence/E10-network-response-expanded.png)

### 4.5 Flag Retrieval and Verification

After comparing the candidate hashes with the extracted references, I investigated the endpoint associated with the numeric value `0`.

The MD5 hash of the string `0` is:

`cfcd208495d565ef66e7dff9f98764da`

I accessed the corresponding endpoint in the challenge application and retrieved the flag displayed by the application.

I then submitted the flag to TryHackMe and verified that it was accepted. The room displayed 100% completion.
### Room Completion Overview

The following screenshot shows the Corridor room at 100% completion, with the challenge task marked as completed.

![Corridor Room Completion Overview](cryptonix-corridor-assessment/evidence/E13-room-completion-overview.png)

**Evidence:**

* [E11 — Flag Endpoint](cryptonix-corridor-assessment/evidence/E11-flag-endpoint.png)
* [E12 — Room Completion Proof](cryptonix-corridor-assessment/evidence/E12-room-completion-proof.png)

## 5. Technical Findings

### Finding 1: Hash-Derived URL References

**Observation:** The application used hexadecimal-looking strings as URL path components associated with its door navigation.

**Analysis:** By extracting the references from the HTML and calculating MD5 hashes of candidate numeric strings, I could compare the application's existing references against predictable hash values.

**Security relevance:** When resource identifiers are derived from predictable values, an observer may be able to infer additional candidate endpoints. Hashing an identifier does not, by itself, establish that access to the associated resource is authorized.

**Evidence:** [E03](cryptonix-corridor-assessment/evidence/E03-extracted-door-hashes.png), [E04](cryptonix-corridor-assessment/evidence/E04-md5-calculation-zero-and-one.png), [E05](cryptonix-corridor-assessment/evidence/E05-candidate-index-comparison.png).

### Finding 2: Resource Discovery Beyond Visible Navigation

**Observation:** The application's visible corridor did not expose every endpoint that could be investigated through direct URL access.

**Analysis:** I used the extracted references and candidate hash calculations to investigate endpoints beyond those immediately apparent from the visual interface.

**Security relevance:** An application should not rely on the absence of a visible navigation link to protect a resource. Resource access should be enforced by appropriate server-side authorization checks.

**Evidence:** [E08](cryptonix-corridor-assessment/evidence/E08-network-door-request-summary.png), [E09](cryptonix-corridor-assessment/evidence/E09-network-door-request-details.png), [E11](cryptonix-corridor-assessment/evidence/E11-flag-endpoint.png).

### Finding 3: Successful Challenge Flag Retrieval

**Observation:** Accessing the endpoint associated with the MD5 hash of `0` revealed the challenge flag.

**Analysis:** The endpoint investigation demonstrated that examining URL construction and testing candidate references could reveal a resource not discovered through the initial visual navigation alone.

**Result:** The flag was accepted by TryHackMe, and the room reached 100% completion.

**Evidence:** [E11 — Flag Endpoint](cryptonix-corridor-assessment/evidence/E11-flag-endpoint.png) and [E12 — Room Completion Proof](cryptonix-corridor-assessment/evidence/E12-room-completion-proof.png).

## 6. Security Recommendations

The exercise illustrates several general web application security principles:

1. **Enforce server-side authorization.** Every protected resource should be checked against the requesting user's permissions, regardless of how the user reaches it.

2. **Do not treat hashed identifiers as access controls.** Hashing a predictable identifier does not replace authorization. Even unpredictable identifiers should not be the sole protection for sensitive resources.

3. **Validate resource access consistently.** Direct requests should receive the same authorization checks as requests initiated through the application's normal interface.

4. **Test for unauthorized resource discovery.** Security assessments should examine whether resources can be accessed by manipulating URL paths or changing object references.

5. **Maintain meaningful security logging.** Applications should record relevant unauthorized access attempts and investigate suspicious request patterns.

These recommendations are general lessons derived from the observed challenge behavior. The assessment did not establish that every listed weakness is present in a production system.

## 7. Limitations

This assessment was conducted against an intentionally vulnerable training environment.

The investigation demonstrated hash-based endpoint analysis and successful retrieval of the challenge flag. It did not demonstrate operating-system kernel exploitation, local privilege escalation, or the establishment of a Metasploit session.

Those activities are outside the demonstrated scope of this Corridor assessment and are not claimed as completed work.

The findings should therefore be understood as a documented web security exercise rather than a comprehensive assessment of a production application.

## 8. Conclusion

Completing the Corridor challenge gave me practical experience investigating how a web application's URL structure can expose resources beyond its visible navigation.

I used browser developer tools to inspect the application, extracted its door references, calculated MD5 hashes for candidate numeric inputs, and compared the results to identify an endpoint worth investigating.

The endpoint associated with the hash of `0` revealed the challenge flag, which I successfully submitted to TryHackMe. The room completion screenshot provides independent confirmation that the challenge was completed.

The main lesson I took from this exercise is that **resource identifiers and visual navigation should never substitute for server-side access control**. Predictable identifiers can make resource discovery easier, but authorization must determine whether a requested resource can actually be accessed.

This assessment forms a practical component of my broader cybersecurity major project, Cryptonix Systems. Future development will require separate implementation and testing work before any broader framework or exploitation capabilities can be claimed.

## 9. Repository Contents

| Path                                                                                 | Description                                 |
| ------------------------------------------------------------------------------------ | ------------------------------------------- |
| [`assets/`](cryptonix-corridor-assessment/assets/)                                                                 | Banner image used in this assessment README |
| [`evidence/`](cryptonix-corridor-assessment/evidence/)                                                             | Screenshots supporting the investigation    |
| [`report/corridor-security-assessment.md`](cryptonix-corridor-assessment/report/corridor-security-assessment.md)   | Full Markdown assessment report             |
| [`report/corridor-security-assessment.pdf`](cryptonix-corridor-assessment/report/corridor-security-assessment.pdf) | Formatted PDF assessment report             |

## 10. Acknowledgements

Thanks to TryHackMe for providing the Corridor room as a practical environment for learning and applying web security assessment techniques.

---

<div align="center">

**CRYPTONIX SYSTEMS**
*Learn. Investigate. Build Securely.*

</div>
