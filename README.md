# jmeter-performance-tests

A performance testing suite built with Apache JMeter 5.6.3 against the Swagger Petstore REST API. The goal was to simulate realistic concurrent user load on REST endpoints and measure how response times and throughput hold up under that load.

## Why JMeter

JMeter is the most widely used open-source performance testing tool in professional QA teams. Unlike functional tests which validate correctness one request at a time, performance tests answer a different question — what happens when hundreds of users hit the same endpoint simultaneously? This suite focuses on that.

## What This Tests

Two API operations are executed in sequence by each virtual user, simulating a realistic flow:

- **GET /v2/pet/findByStatus** — fetch all pets with status "available"
- **POST /v2/pet** — create a new pet entry with a JSON request body

## Test Configuration

| Parameter | Value |
|---|---|
| Virtual Users (Threads) | 10 |
| Ramp-up Period | 5 seconds |
| Loop Count | 1 |
| Target API | https://petstore.swagger.io/v2 |

The 5 second ramp-up means JMeter starts 2 users per second rather than hitting the server with all 10 simultaneously — this more closely mirrors how real traffic builds up.

## Results Summary

| Endpoint | Samples | Avg Response | Min | Max | Error % | APDEX |
|---|---|---|---|---|---|---|
| GET - Find Pets by Status | 10 | ~1700ms | ~1480ms | ~2700ms | 0.00% | 0.450 |
| POST - Create Pet | 10 | ~900ms | ~225ms | ~2600ms | 0.00% | 1.000 |

100% pass rate across all 20 requests.

## Reading the APDEX Score

APDEX (Application Performance Index) is an industry standard score between 0 and 1 that measures user satisfaction based on response times. A score of 1.0 means all users were satisfied, 0 means nobody was.

The POST endpoint scored a perfect 1.000. The GET endpoint scored 0.450 — this means response times frequently exceeded the 500ms toleration threshold. This is expected behaviour for a public demo server that is shared globally, not a reflection of a code issue. In a real production scenario, this would be flagged and investigated.

## How to Run

1. Install Apache JMeter 5.6.3 from https://jmeter.apache.org
2. Open `petstore-load-test.jmx` in the JMeter GUI
3. Run the test plan using the green play button
4. View results in the Summary Report or View Results Tree listeners
5. To regenerate the HTML report, run:

jmeter -g results.csv -o reports/

## Tech Stack

- Apache JMeter 5.6.3
- Swagger Petstore public API
- HTML Dashboard Report