  
# **RCMS Report Generation and Email Notification Process**  

---

## **1. Purpose**  
This document outlines the changes made to the report generation and email notification process. It serves as a reference for understanding the previous and current workflows, as well as the technical details involved.  

---

## **2. Previous Process**  
The earlier workflow for report generation and email notification was as follows:  
1. **Report Generation:** The report was generated on the ERP server.  
2. **File Transfer:** The generated report file was transferred via FTP to the test server.  
3. **Email Trigger:** The test server triggered the email and sent it to all respective recipients.  

---

## **3. Current Process**  
The process has been updated to improve efficiency and reduce complexity. The new workflow is as follows:  
1. **Report Generation:** The report is generated on the ERP server.  
2. **Email Trigger:** The report is now triggered via email using the **'rcms-event-notifications-rest-api'**.  

---

## **4. Technical Details**  
Below are the key technical details of the current process:  

### **Report File Path**  
- Location where the report is stored:  
  `/var/www/dbs/rcms/Automation`  

### **Email User Configuration**  
- File used to add or remove email users:  
  `EmailDataConfig.php`  

### **Email Vendor Configuration**  
- Configuration file for email and SMS settings:  
  `email_sms_config.php`  

---

## **5. Steps to Configure or Modify**  
1. **Adding/Removing Email Users:**  
   - Open the `EmailDataConfig.php` file.  
   - Add or remove email addresses as needed.  

2. **Modifying Email Vendor Settings:**  
   - Open the `email_sms_config.php` file.  
   - Update the configuration parameters as required.  

---

## **6. Benefits of the New Process**  
- Eliminates the need for FTP file transfer.  
- Simplifies the workflow by directly triggering emails from the ERP server.  
- Reduces dependency on the test server for email notifications.  

---

## **7. Points to Verify After Implementation**  
- Ensure the `rcms-event-notifications-rest-api` is functioning correctly.  
- Verify that emails are being sent to all intended recipients.  
- Confirm that the report files are being generated and stored in the correct path (`/var/www/dbs/rcms/Automation`).  

---

## **8. Support and Troubleshooting**  
For any issues or queries related to this process, please contact:  
- **Name:** martin 
- **Email:** Martin@gmail.com
- **Phone:** 1234567890  

---

**Prepared By:** Pandi 
**Date:** 19-02-2025  

---
