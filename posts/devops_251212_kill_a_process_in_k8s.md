# Killing Processes in K8s Pods: PID Namespace Mismatch

**Publication Date**: December 12, 2025
**Author**: Ailven LIU 刘翔
**All Rights Reserved*

## Problem

Process visible in `ps -ef` but cannot be killed:
```bash
$ ps -ef | grep <process>
root  3563929  ...  /bin/bash ./script.sh

$ kill -9 3563929
-bash: kill: (3563929) - No such process
```

## Root Cause

The process runs in a different PID namespace. Your shell and the target process are isolated in separate namespaces, preventing direct signal delivery.

## Quick Diagnosis

```bash
# 1. Check if PID namespaces differ
readlink /proc/<PID>/ns/pid
readlink /proc/$$/ns/pid
# Different output = namespace mismatch

# 2. Find the process's PID within its own namespace
cat /proc/<PID>/status | grep NSpid
# Output: NSpid: <host_PID> <container_PID>
```

## Solution

Use `nsenter` to enter the process's namespace and kill it using its container PID:

```bash
# Get container PID from NSpid (second number)
CONTAINER_PID=$(cat /proc/<HOST_PID>/status | grep NSpid | awk '{print $3}')

# Enter namespace and kill
nsenter -t <HOST_PID> -p -m kill -9 $CONTAINER_PID
```

**Example:**
```bash
# Process 3563929 has NSpid: 3563929 5984
nsenter -t 3563929 -p -m kill -9 5984
```

## General Troubleshooting Script

```bash
#!/bin/bash
TARGET_PID=$1

# Check if process exists
[ ! -d "/proc/$TARGET_PID" ] && echo "Process not found" && exit 1

# Check if zombie process
STATE=$(grep "^State:" /proc/$TARGET_PID/status | awk '{print $2}')
if [ "$STATE" = "Z" ]; then
    PPID=$(grep "^PPid:" /proc/$TARGET_PID/status | awk '{print $2}')
    echo "Zombie process - killing parent: $PPID"
    kill -9 $PPID
    exit 0
fi

# Check PID namespace
PROC_NS=$(readlink /proc/$TARGET_PID/ns/pid)
CURR_NS=$(readlink /proc/$$/ns/pid)

if [ "$PROC_NS" != "$CURR_NS" ]; then
    echo "PID namespace mismatch - using nsenter"
    CONTAINER_PID=$(grep "^NSpid:" /proc/$TARGET_PID/status | awk '{print $3}')
    nsenter -t $TARGET_PID -p -m kill -9 $CONTAINER_PID
else
    kill -9 $TARGET_PID
fi
```

## Common Scenarios

| Issue | Diagnosis | Solution |
|-------|-----------|----------|
| PID namespace mismatch | Different namespace IDs | `nsenter -t <PID> -p -m kill <container_PID>` |
| Zombie process | State = `Z` in `ps aux` | Kill parent: `kill -9 <PPid>` |
| Process already terminated | `/proc/<PID>` missing | No action needed |

## Alternative Approaches

**From within the pod:**
```bash
kubectl exec -it <pod-name> -- kill <PID>
```

**Terminate entire pod:**
```bash
kubectl delete pod <pod-name> --force --grace-period=0
```

## Key Takeaway

In containerized environments, always check PID namespaces when `kill` fails. Use `nsenter` to bridge namespace boundaries.
