# **AWS CloudTrail Log Monitoring & Detection Project**

## **Overview**

This project demonstrates how to build a basic **Security Operations Center (SOC) detection pipeline** using AWS services. It focuses on monitoring account activity, detecting suspicious behavior, and generating real-time alerts.

The system captures AWS activity logs, analyzes them for specific security events, and notifies the user via email when those events occur.

---

## **Objectives**

* Enable centralized logging of AWS account activity  
* Detect high-risk security events  
* Create real-time alerts for suspicious behavior

---

## **Services Used**

* AWS CloudTrail (log collection)  
* Amazon CloudWatch (monitoring & alerting)  
* Amazon SNS (notifications)  
* Amazon S3 (log storage)

---

## **Architecture**

CloudTrail → CloudWatch Logs → Metric Filters → CloudWatch Alarms → SNS → Email Alert  
---

## **⚙️ Implementation Steps**

### **Enable CloudTrail**

* Created a trail to log all account activity across all regions  
* Configured S3 bucket for log storage  
* Enabled management events (Read \+ Write)

---

### **Send Logs to CloudWatch**

* Enabled CloudWatch Logs integration  
* Created log group:  
  `/aws/cloudtrail/soc-monitoring`  
* Configured IAM role for log delivery

---

### **Create Detection Rules (Metric Filters)**

#### **🔐 Root Account Login Detection**

**Metric Name:** `RootAccountLoginEvents`

**Filter Pattern:**

{ $.userIdentity.type \= "Root" && $.eventName \= "ConsoleLogin" }  
---

#### **🚨 Failed Console Login Detection**

**Metric Name:** `LoginFailureEvents`

**Filter Pattern:**

{ $.eventName \= "ConsoleLogin" && $.responseElements.ConsoleLogin \= "Failure" }  
---

### **Create CloudWatch Alarms**

#### **Root Account Alert**

* **Alarm Name:** `RootAccountLoginAlert`  
* Metric: `RootAccountLoginEvents`  
* Threshold: ≥ 1 event in 5 minutes  
* Action: SNS notification

---

#### **Failed Login Alert**

* **Alarm Name:** `FailedLoginAlarm`  
* Metric: `LoginFailureEvents`  
* Threshold: ≥ 3 failed attempts in 5 minutes  
* Action: SNS notification

---

### **Configure SNS Notifications**

* Created SNS topic: `soc-alerts`  
* Subscribed email endpoint  
* Confirmed subscription  
* Linked SNS topic to CloudWatch alarms

---

## **Testing**

To validate detections:

* Root Login → Sign in using root account  
* Failed Login → Enter incorrect credentials multiple times

Expected result: Email alert triggered via SNS

---

## **Use Cases**

* Detect unauthorized root account usage  
* Identify potential brute-force login attempts

---

## **Skills Demonstrated**

* Log analysis  
* Detection engineering  
* Cloud security monitoring  
* Alert configuration

