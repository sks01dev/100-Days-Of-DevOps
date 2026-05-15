
# Day 1: Non-Interactive User Management

### 1. The Problem

The system administration team requires the creation of a service account for a backup agent tool.

* **Target:** App Server 1
* **User Name:** `kirsty`
* **Requirement:** The user must have a **non-interactive shell** to prevent any human from logging into this account for security reasons.

---

### 2. The Concept (Intuition)

To master this task, you need to understand three core Linux realities:

* **The Remote Workplace (The Network):** You don't work on the computer you are sitting at. You are on a **Jump-Host** (a gateway). To do the work, you must "teleport" your terminal to the **App Server** using **SSH**.
* **The Library (The Filesystem):** Linux doesn't have a hidden database for users. It has a text file: `/etc/passwd`. Adding a user is simply telling the system to write a new line of text into that file.
* **The Locked Door (The Shell):** Usually, when a user logs in, Linux starts a "Shell" (like a desk with a computer). By assigning `/sbin/nologin`, we tell Linux: "This user has an ID, but they don't get a desk." This is perfect for robots/tools that only run in the background.

---

### 3. The Implementation

#### Step 1: Connect to the Target Server

First, move from the Jump-Host to the App Server.

```bash
ssh stapp01

```

#### Step 2: Create the User with Constraints

We use `sudo` for authority and `-s` to define the "No Login" shell.

```bash
sudo useradd -s /sbin/nologin kirsty

```

#### Step 3: Verify the System Records

Read the `/etc/passwd` file to confirm the Librarian wrote the entry correctly.

```bash
grep kirsty /etc/passwd

```

---

### 4. Logic Diagram

This diagram represents the flow of your commands across the infrastructure:

```
[ YOUR TERMINAL ] 
      |
      | (ssh) --> [ APP SERVER 1 ]
                       |
                       | (sudo useradd)
                       |
        [ /etc/passwd File ] <--- (New Entry Added)
        ------------------------------------------
        kirsty:x:1001:1001::/home/kirsty:/sbin/nologin
                                         ^--- (The "Lock")


```
---


### 5. Summary of Day 1

* **SSH:** Used to switch the context of your commands to a remote server.
* **SUDO:** Necessary because `/etc/passwd` is a protected system file.
* **NOLOGIN:** A security best practice (Principle of Least Privilege) to ensure service accounts cannot be used by hackers to get a command prompt.
