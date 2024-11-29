# JMeter Performance and Load Testing for JSONPlaceholder API

## Table of Contents
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Test Plan Setup](#test-plan-setup)
   - 3.1 [Load Testing](#load-testing)
   - 3.2 [Performance Testing](#performance-testing)
4. [User Simulation Scenarios](#user-simulation-scenarios)
5. [Stress and Load Testing](#stress-and-load-testing)
6. [Running the Test](#running-the-test)
7. [Result Analysis](#result-analysis)
8. [Conclusion](#conclusion)

---

## 1. Introduction

This repository contains a **JMeter test plan** designed to evaluate the performance and load capacity of the **JSONPlaceholder API**, specifically focusing on the **Posts** and **Users** sections of the API.

The objective of this test is to measure response times, throughput, and error rates under varying conditions, as well as to simulate different user scenarios, including high read and write traffic.

---

## 2. Prerequisites

To run these JMeter tests, you need the following:

- **JMeter** installed locally or running in a Docker container.
- **Git** installed to clone the repository and pull the JMeter test plan (`Typicode.jmx`).
- A stable internet connection to access the JSONPlaceholder API: [JSONPlaceholder API](https://jsonplaceholder.typicode.com/).

---

## 3. Test Plan Setup

### 3.1 Load Testing

The load testing scenario simulates multiple concurrent users interacting with the API to measure how the API behaves under high load.

#### Thread Groups:
1. **Thread Group 1: GET /posts (Read Posts)**:
   - Simulate 200 users (requests to fetch all posts).
   - Ramp-up Time: 30 seconds.
   - Loop Count: Infinite (requests sent continuously).

2. **Thread Group 2: POST /posts (Create Post)**:
   - Simulate 100 users creating new posts.
   - Ramp-up Time: 20 seconds.
   - Loop Count: 10 requests per user.

3. **Thread Group 3: PUT /posts/{id} (Update Post)**:
   - Simulate 50 users updating an existing post.
   - Ramp-up Time: 15 seconds.
   - Loop Count: 5 requests per user.

4. **Thread Group 4: DELETE /posts/{id} (Delete Post)**:
   - Simulate 30 users deleting posts.
   - Ramp-up Time: 10 seconds.
   - Loop Count: 3 requests per user.

#### Configuration:
- **CSV Data Set Config**: Randomizes user data such as user IDs and post IDs.
- **HTTP Request Samplers**: For each endpoint (`GET /posts`, `POST /posts`, `PUT /posts/{id}`, and `DELETE /posts/{id}`), configure the appropriate request type (GET, POST, PUT, DELETE) and the request body if needed (for POST and PUT requests).

---

### 3.2 Performance Testing

Performance testing measures the response time, throughput, and error rate under different load conditions.

#### Timers:
- **Constant Timer**: Introduces a fixed delay between requests to simulate realistic user behavior.
- **Gaussian Random Timer**: Introduces varying delays between requests for even more realistic simulation.

#### Listeners:
- **Summary Report**: Track throughput, average response time, and error rates.
- **View Results Tree**: Inspect individual responses during the test.
- **Graph Results**: Visualize performance over time (e.g., response times and throughput).
- **Response Time Graph**: Show how response time changes over the test execution.

---

## 4. User Simulation Scenarios

### Scenario 1: High Read Traffic

- Simulate 500 concurrent users accessing the `GET /posts` endpoint to retrieve all posts.
- Monitor response time and throughput over a sustained period (e.g., 30 minutes).

### Scenario 2: High Write Traffic

- Simulate 200 users making `POST /posts` requests to create new posts.
- Monitor response time for successful POST requests and track error rates.

### Scenario 3: Mixed Traffic

- Simulate a combination of `GET /posts` and `POST /posts` requests:
  - 100 users making `GET` requests.
  - 50 users making `POST` requests.
- Simulate mixed traffic over a period to assess system stability.

---

## 5. Stress and Load Testing

### 5.1 Stress Testing

- Gradually increase the number of virtual users to simulate stress conditions.
- Measure how the response time degrades as the load increases, and determine the breaking point of the API.

### 5.2 Load Testing (Concurrency Testing)

- Simulate high load with 1000 concurrent users across all four thread groups.
- Gradually increase the number of users to see how the system performs under increasing load.
- Monitor for server failures or timeouts under high load.

---

## 6. Running the Test

To run the JMeter test locally, follow these steps:

1. **Clone the repository**:
   ```bash
   git clone https://github.com/silumi95/Typicode-Load-Performence--JMeter-Testing.git
   cd Typicode-Load-Performence--JMeter-Testing
   ```

2. **Run JMeter**:
   If you have **JMeter** installed locally, open the JMeter GUI and load the `Typicode.jmx` file. You can then run the test from the GUI.

   Alternatively, you can run the tests in **non-GUI mode** using the following command:
   ```bash
   jmeter -n -t Typicode.jmx -l result_file.jtl -e -o output_directory
   ```

3. **JMeter in Docker**:
   If you are using Docker, the following command can run the tests:
   ```bash
   docker run --rm \
     -v $(pwd):/test \
     -v $(pwd)/results:/results \
     -v $(pwd)/output:/output \
     justb4/jmeter:latest -n -t /test/Typicode.jmx -l /results/result_file.jtl -e -o /output
   ```

---

## 7. Result Analysis

Once the test is complete, you can analyze the results:

- **.jtl File**: Contains detailed results, including response times and error rates.
- **HTML Report**: Generated with the `-e -o` flags, you can view the test's performance in an easily digestible format. The report includes graphs for response time, throughput, and error rates.
  - Open the report in your browser: `output_directory/index.html`.

---

## 8. Conclusion

This repository provides the JMeter test plan for performance and load testing of the **JSONPlaceholder API**, focusing on **Posts** and **Users**. You can use the provided test plan to simulate different user scenarios, perform stress and load testing, and measure key performance metrics like response time, throughput, and error rates.

By running these tests, you can assess how the API behaves under various conditions and identify potential bottlenecks or performance issues that could affect users in a production environment.
