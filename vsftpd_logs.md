# Enable the VSFTPD logs

_1. add the configuration file as per the below instruction._
```conf
# sudo vim /etc/vsftpd.cnf
xferlog_file=/var/log/vsftpd.log #log file path
xferlog_std_format=NO
log_ftp_protocol=YES
```
_2. Restart the vsftpd service & check the status of the service_
```cmd
sudo systemctl restart vsftpd
sudo systemctl  status vsftpd
```



# Proving Effectiveness:
_1.Comprehensive Tracking: The logs show that VSFTPD is effectively tracking all major FTP operations (login, upload, download) performed by users ._

_2.Detailed Information: Each log entry provides crucial details such as timestamps, user names, client IP addresses, file paths, file sizes, and transfer speeds. This level of detail demonstrates that the logging is thorough and accurate._

_3.Correlate Actions with Logs: You can correlate specific actions (like uploading or downloading a file) with the corresponding log entries to confirm that every action is being recorded._
