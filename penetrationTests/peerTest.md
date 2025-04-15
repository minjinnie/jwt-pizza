# Penetration Testing Report

## Name
- **Peer 1**: MJ Sung
- **Peer 2**: Blake Mcghie

### Self attack
**Peer 1**: MJ Sung

#### Test 1 - Brute Force Login
| Item           | Result                                                                         |
| -------------- | ------------------------------------------------------------------------------ |
| **Date**       | April 9, 2025                                                                |
| **Target**     | pizza.minjin.click                                                           |
| **Classification** | Authentication                                                              |
| **Severity**   | 2                                                                              |
| **Description** | The admin@jwt.com account was accessed using a simple password (admin) via Burp Intruder. |
| **Corrections** | Strengthened password, added recommendations for rate limiting and CAPTCHA   |

#### Test 2 - Error Message Exposure
| Item           | Result                                                                         |
| -------------- | ------------------------------------------------------------------------------ |
| **Date**       | April 9, 2025                                                                |
| **Target**     | pizza.minjin.click                                                           |
| **Classification** | Information Disclosure                                                      |
| **Severity**   | 2                                                                              |
| **Description** | Stack trace and file paths were exposed in login failure responses.         |
| **Corrections** | Error messages were simplified and stack traces are now hidden from responses. |

#### Test 3 - JWT Token Replay
| Item           | Result                                                                         |
| -------------- | ------------------------------------------------------------------------------ |
| **Date**       | April 9, 2025                                                                |
| **Target**     | pizza.minjin.click                                                           |
| **Classification** | Session Management                                                          |
| **Severity**   | 2                                                                              |
| **Description** | JWT tokens did not expire and could be reused indefinitely.                  |
| **Corrections** | Token expiration (expiresIn) was added and expiration is now verified on each request. |

---

### Peer 2: Blake Mcghie

#### Test 1 - SQL Injection
| Item           | Result                                                                         |
| -------------- | ------------------------------------------------------------------------------ |
| **Date**       | April 14, 2025                                                                |
| **Target**     | pizza.mcghie-blake.com                                                        |
| **Classification** | Injection                                                                   |
| **Severity**   | Low                                                                            |
| **Description** | The endpoint to update a user is possibly vulnerable to SQL injection due to non-parameterized queries. Upon investigation, the furthest exploitation I could illicit was a stack dump from a SQL error. |
| **Corrections** | Updated the ‘updateUser’ function to be more strict. Parameterized the query to avoid any future issues. |

---

### Peer attack

#### Peer 1 attack on peer 2: Unauthenticated API Access
| Item           | Result                                                                         |
| -------------- | ------------------------------------------------------------------------------ |
| **Date**       | April 9, 2025                                                                |
| **Target**     | pizza.mcghie-blake.click                                                       |
| **Classification** | Access Control                                                              |
| **Severity**   | 0                                                                              |
| **Description** | All protected endpoints returned 401 Unauthorized when accessed without a token. |
| **Corrections** | No changes required. Authentication is correctly enforced. |

#### Peer 2 attack on peer 1: Sensitive Data Exposure
| Item           | Result                                                                         |
| -------------- | ------------------------------------------------------------------------------ |
| **Date**       | April 14, 2025                                                                |
| **Target**     | pizza.minjin.click                                                            |
| **Classification** | Security Misconfiguration                                                    |
| **Severity**   | Critical                                                                       |
| **Description** | Sensitive administrator login information found through the pizza-service.minjin.com/api/docs endpoint. Using this information, I can destroy a lot of data on the website, including closing all franchises in the database. |
| **Corrections** | Updated admin login, removed actual user login information from the public facing documentation. Added authentication requirements (requires admin token) to retrieve documentation. |

---

## Combined Summary of Learnings:

#### **MJ:**
- Never expose internal documentation or admin credentials to the public. Always protect sensitive routes with proper authentication and review default configurations in deployment.

#### **Blake:**
- It is always worth limiting access to as much information as possible about an application. Also, don’t write SQL queries that are not parameterized if they contain user input.
