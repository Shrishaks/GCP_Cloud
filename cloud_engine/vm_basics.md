# Google Compute Engine - VM Basics

- **Step-01:** Create a **Linux** Virtual Machine
- **Step-02:** Connect to VM using **SSH**
- **Step-03:** Install a simple webserver
- **Step-04:** Access the sample pages via browser
- **Step-05:** Explore VM Actions
  - Stop / Start
  - Suspend / Resume
  - Reset
  - Delete
- **Step-06:** Explore VM
  - Details Tab
  - Observability Tab
  - Screenshot Tab

---

# VM Instance Start / Stop

## What will we lose when we stop the VM?

- External or **Public IP** will be released and during the next start different public IP will be assigned to VM.

## What will VM retain when we stop the VM?

- Stopped VM retains its **persistent disks, internal IPs and MAC Address**.

## When do you stop the VM instance?

- To **save cost** when we don’t need that VM in running state
- To **change** VM Instance **Machine Type** features
- To add or remove **disks** attached to VM

## Billing

- VMs in the stopped state (TERMINATED) are **not charged**
- Persistent Disks attached to VM, external IP Address (static) are **charged**

---

# VM Instance Suspend / Resume

- Suspended Instances **preserve** the OS Memory, device state and application state
- When you **suspend** your instance, its memory **moves to storage**.
- When you **resume** your instance, its memory **moves from storage to the instance memory**.
- You can only suspend an instance for **up to 60 days** before the VM is **automatically stopped**.
- You cannot suspend instances with more than **120 GB of memory**.
- Most of the OS **support** Suspend and Resume features. Few of the OS, **don’t support** these features.

## Usecases

- Dev and Test environments that are not used during non-business hours whose application state to be **preserved**
- Applications which take **long time for initialization** when they come online

## Pricing

- Google charges for the **storage** necessary for the **instance memory**
- Any **persistent disk** attached the VM Instance
- Any **Static IPs** attached to the instance

---

# VM Instance Reset / Delete

## Reset VM Instance

- Performing a reset on your VM is **similar to doing a hard reset** on your computer.
- Resetting a **VM forcibly wipes the memory contents** of the machine and resets the VM to its initial state.
- The VM **does not perform a clean shutdown** of the guest OS.
- Throughout this process, the VM remains in the **RUNNING** state.

## Delete VM Instance

- Delete an instance to **remove the instance** and the **associated resources** from your project

---

# Web Server Installation Script

```bash
#!/bin/bash
sudo apt install -y telnet
sudo apt install -y nginx
sudo systemctl enable nginx
sudo chmod -R 755 /var/www/html
HOSTNAME=$(hostname)
sudo echo "<!DOCTYPE html> <html> <body style='background-color:rgb(250, 210, 210);'> <h1>Welcome to StackSimplify - WebVM App1 </h1> <p><strong>VM Hostname:</strong> $HOSTNAME</p> <p><strong>VM IP Address:</strong> $(hostname -I)</p> <p><strong>Application Version:</strong> V1</p> <p>Google Cloud Platform - Demos</p> </body></html>" | sudo tee /var/www/html/index.html
```
