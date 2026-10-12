# Backup, Restoration and Incident Recovery Runbook

## Purpose

This runbook records the backup, restoration, and service recovery procedures tested during the Cloud Operations, Monitoring & Recovery Lab.

## 1. Create a Test File

```bash
mkdir -p ~/cloudops-recovery-demo
echo "CloudOps recovery test - original file" > ~/cloudops-recovery-demo/status.txt
cat ~/cloudops-recovery-demo/status.txt
```

## 2. Create and Verify a Backup

```bash
cd ~
tar -czvf cloudops-recovery-backup.tar.gz cloudops-recovery-demo/
tar -tzvf ~/cloudops-recovery-backup.tar.gz
```

Confirm that the archive contains `cloudops-recovery-demo/status.txt`.

## 3. Restore and Validate the Backup

```bash
mkdir -p ~/cloudops-restore-test
tar -xzvf ~/cloudops-recovery-backup.tar.gz -C ~/cloudops-restore-test
cat ~/cloudops-restore-test/cloudops-recovery-demo/status.txt
diff ~/cloudops-recovery-demo/status.txt ~/cloudops-restore-test/cloudops-recovery-demo/status.txt
```

No output from `diff` indicates that the original and restored files are identical.

## 4. Verify Nginx Health

```bash
sudo systemctl is-active nginx
curl -I --max-time 5 http://localhost
```

The healthy service returned `active` and HTTP `200 OK`.

## 5. Simulate a Service Outage

```bash
sudo systemctl stop nginx
sudo systemctl is-active nginx
curl -I --max-time 5 http://localhost
```

During the test, Nginx returned `inactive`, and the HTTP request failed to connect. This was the expected result.

## 6. Restore the Service

```bash
sudo systemctl start nginx
sudo systemctl is-active nginx
curl -I http://localhost
```

Recovery was successful: Nginx returned to `active`, and the HTTP response was `200 OK`.

## 7. Troubleshooting Commands

Inspect service status:

```bash
sudo systemctl status nginx --no-pager
```

Inspect recent logs:

```bash
sudo journalctl -u nginx -n 50 --no-pager
```

Validate Nginx configuration:

```bash
sudo nginx -t
```

## 8. Recovery Evidence

The recovery test output was saved on the EC2 instance at:

`~/cloudops-evidence/recovery-test.txt`

The test demonstrated successful file restoration and recovery of a stopped Nginx service.

## Limitations

The backup test used a local archive on the same EC2 instance. It verifies archive creation and file restoration, but it does not protect against loss of the entire instance or its underlying storage. A production backup strategy should keep independent copies outside the instance and test recovery regularly.
