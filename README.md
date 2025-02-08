# superapp Application Deployment Guide

This guide explains how to deploy the Superapp application using Ansible from the bastion server.

## Prerequisites

- Access to the bastion server SSH key (`superapp.pem`)
- Basic understanding of Ansible and SSH
- The deployment playbook and inventory files

## Connecting to the Bastion Server

1. Ensure you have the `superapp.pem` key file on your local machine
2. Set proper permissions on the key file:
   ```bash
   chmod 400 superapp.pem
   ```
3. Connect to the bastion server:
   ```bash
   ssh -i "superapp.pem" ubuntu@ec2-xx-xxx-xxx-xx.af-south-1.compute.amazonaws.com
   ```

## Deployment Process

1. Once connected to the bastion server, verify the presence of required files:

   ```bash
   ls -l
   # Should show:
   # - deploy-superapp.yml
   # - inventory
   ```

2. Run the deployment playbook:
   ```bash
   ansible-playbook -i inventory deploy-superapp.yml
   ```

## What the Deployment Does

The deployment process:

1. Creates backups of the existing application
2. Updates code from Git repository
3. Builds the Go application
4. Sets appropriate permissions
5. Restarts the service
6. Performs health checks
7. Automatically rolls back if deployment fails

## Directory Structure

- Application Directory: `/work/superapp`
- Backup Directory: `/work/superapp_backup_[timestamp]`

## Health Checks

The deployment includes automatic health checks that verify:

- Service is running properly
- Application is responding to HTTP requests
- Proper permissions are set

## Rollback Process

The playbook includes automatic rollback capabilities that trigger if:

- The build fails
- The service fails to start
- Health checks fail

## Troubleshooting

Common issues and solutions:

1. Permission denied errors:

   - Verify you're using the correct SSH key
   - Ensure key permissions are set to 400

2. Deployment failures:

   - Check the service logs: `sudo journalctl -u superapp`
   - Verify Git repository access
   - Check disk space availability

3. Health check failures:
   - Verify the application is running: `sudo systemctl status superapp`
   - Check application logs
   - Verify network connectivity

## Support

For deployment issues or questions, contact:

- DevOps Team: Elias Baya - Eliasbaya1223@gmail.com
- Repository Maintainers: Elias Baya - Eliasbaya1223@gmail.com

## Notes

- Always ensure you have the latest version of the deployment files
- The deployment process includes automatic backups for safety
- Failed deployments will automatically rollback to the previous version
- Monitor the deployment output for any error messages or warnings
