# DevOps-For-Beginners
This repository documents the daily operations of DevOps engineers.

Day 1: Create a linux user who cannot log into shell (Non-Interactive Shell).

We can achieve this by /sbin/nologin or /bin/false

Why organisations doing this

Prevents login, but still allows the user to own files and run background services.
The user cannot log in via SSH or TTY, but you can still use this account for running daemons, service accounts, or restricting access.

