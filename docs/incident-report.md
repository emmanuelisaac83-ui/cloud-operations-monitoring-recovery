# Incident Report: Nginx Service Outage and Recovery

## 1. Incident Summary

A controlled service outage was simulated on the Cloud Operations, Monitoring & Recovery Lab EC2 instance to practise basic incident response and service recovery.

**Affected service:** Nginx web server  
**Environment:** AWS EC2 running Ubuntu Server 24.04 LTS  
**Incident type:** Service availability  
**Test classification:** Planned simulation

## 2. Incident Simulation

The Nginx service was deliberately stopped using:

```bash
sudo systemctl stop nginx
```

The service state was checked and returned `inactive`. A local HTTP request using `curl` failed to connect, confirming that the web service was unavailable during the test.

## 3. Diagnosis

The service status and HTTP response were used to verify the simulated outage.

Commands used:

```bash
sudo systemctl is-active nginx
curl -I --max-time 5 http://localhost
```

The test demonstrated how service status checks and basic HTTP requests can help identify an availability problem.

## 4. Recovery Actions

Nginx was restarted:

```bash
sudo systemctl start nginx
```

The service status and HTTP response were checked again.

## 5. Recovery Validation

The recovery was successful:

- Nginx returned to the `active` state.
- The local HTTP request returned `HTTP/1.1 200 OK`.
- A recovery evidence file was saved at `~/cloudops-evidence/recovery-test.txt`.

## 6. Lessons Learned

- Check service status before deciding on a recovery action.
- Use service logs and configuration tests when the cause of a real incident is unclear.
- Verify recovery through both service status and an application-level request.
- Record incident actions and results to support troubleshooting and future reviews.

## 7. Scope and Limitations

This was a controlled learning exercise, not a production incident. The test confirmed that Nginx could be restarted successfully, but it did not test automatic alerting, automated remediation, or recovery from an EC2 instance failure.

The recovery evidence was generated on the EC2 instance. It should be copied to a separate location if it needs to remain available after the instance is terminated.
