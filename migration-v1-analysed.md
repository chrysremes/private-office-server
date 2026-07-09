# Migration Plan v1 – Refined Phase 1 Action Plan
Based on a discussion, this document summarizes the refined critical concerns for Phase 1, incorporating your feedback regarding budget, static IP, deferred items, and the current status of Ubuntu 26.04. All non-urgent items (monitoring, training, advanced security, etc.) have been consciously moved to Phase 2.

# Items Explicitly Deferred to Phase 2 (No Action Required Now)
Deferred Item;	Rationale
Backup Strategy	Not a current worry; we will keep basic manual copies initially.
Monitoring / Alerts	Moved to Phase 2 to reduce initial complexity.
User Training & Local Champion	Moved to Phase 2; basic verbal instructions will suffice for the pilot.
Advanced Access Control (User Permissions)	Moved to Phase 2 to avoid overcomplicating the initial setup.
Security Hardening (Fail2ban, Key-based SSH)	Moved to Phase 2; we will keep minimal UFW firewall for Phase 1.
Comprehensive Maintenance Documentation	Moved to Phase 2; only a quick-reference guide will be created for the pilot.

# Critical Concerns & Phase 1 Recommendations
Below are the three specific concerns we agreed to explore in depth, plus a verification for Tailscale.

## Concern: Samba Performance Over WAN
Context: SMB/Samba is notoriously "chatty" and can feel sluggish over high-latency internet connections (even with decent bandwidth).

Recommendation for Phase 1 (Test-First Approach):

Step 1 (Baseline): Deploy Samba over Tailscale as originally planned. Measure performance by opening a typical file (e.g., the Cadastro.xlsm).

Passing criteria: File opens in < 3 seconds and saves in < 2 seconds over the WAN.

Step 2 (Fallback - WebDAV): If Samba proves too slow, we switch to WebDAV over HTTPS.

Why: WebDAV uses HTTP/2, handles latency better, and can still be mapped as a Windows network drive (using the "Add Network Location" wizard).

Setup: Install Apache/nginx with mod_dav, secure it with Let's Encrypt (SSL).

Cost: Free (open-source).

Compatibility: Windows 10/11 maps WebDAV drives natively; Excel/Word open files directly from it.

Action Item: Test Samba first. If performance fails, we pivot to WebDAV (I can provide a detailed configuration script upon request).

## Concern: Excel Macro (.xlsm) Stability & Compatibility
Context: The "Cadastro" relies on VBA macros. Excel treats network drives differently than local drives.

Recommendations for Phase 1:

Trusted Locations (Critical): Add the mapped network drive (e.g., Z:\) to Excel's Trusted Center → Trusted Locations on every Windows machine. Without this, macros will be blocked or prompt security warnings every time.

File Format Optimization: If possible, convert the .xlsm to .xlsb (Excel Binary Workbook).

Benefit: Smaller file size and faster load/save times over the network.

Caution: Test a copy thoroughly to ensure macros work identically.

AutoRecover: Ensure "Save AutoRecover information every X minutes" is enabled (set to 5 minutes) to mitigate data loss during network hiccups.

Network Drop Mitigation: Warn users to explicitly Save and Close the file before switching networks or turning off their machines.

Action Item: Prepare a batch script or simple guide for adding the Trusted Location on each PC. Test the .xlsm extensively before going live with the pilot.

## Concern: File Locking vs. Manual Conflict Management
Context: You mentioned manual conflict management works today, but with simultaneous remote access, a new risk of version conflicts emerges.

Verification & Recommendation for Phase 1:

Samba defaults (specifically oplocks = yes and kernel oplocks = yes) already provide a basic file-locking mechanism. This should prevent simultaneous writes in most cases.

However, manual conflict management is currently a process, not a technical feature.

Recommendation: Keep the manual process but augment it with an informal communication rule (e.g., ping the team on WhatsApp before opening Cadastro.xlsm remotely). This is a cheap, zero-tech solution that aligns with the business culture.

Monitor during the pilot if any conflicts occur. If they become frequent, we can revisit enabling stricter strict locking or implementing a simple "check-out" text file in Phase 2.

## Verification: Tailscale + Static IP
Context: You asked if Tailscale still works with a Static IP.

Confirmation:
Yes, Tailscale works perfectly with a static IP.

Tailscale is a layer-3 mesh VPN; it doesn't care whether your public IP is static or dynamic.

A static IP gives you the added benefit of directly connecting via SSH (or WebDAV) without relying on Tailscale's relay servers if you ever need a backup access method.

We will still use Tailscale for the file access (Samba/WebDAV) to keep the traffic encrypted and avoid exposing Samba ports directly to the internet.

# Revised Phase 1 Action Plan (Step-by-Step)
Step	Task	Details / Tools
1	Server OS Setup	Install Ubuntu 26.04 LTS. Enable automatic security updates. Configure UFW to allow only SSH (port 22) and Tailscale ports (default).
2	Tailscale Deployment	Install Tailscale on the Server and all Windows PCs. Ensure all devices appear in the Tailscale admin console. Test ping between Borrazópolis and Marilândia using Tailscale IPs.
3	Samba Setup (Primary)	Configure Samba sharing for the central folder. Map as Z:\ on local (Borrazópolis) Windows PCs using the local IP for best performance.
4	Remote Access Test (Samba)	From Marilândia, map Z:\ using the Tailscale IP of the server. Test opening/saving the Cadastro.xlsm. Measure response time.
5	(Conditional) WebDAV Fallback	If Step 4 is sluggish, set up Apache/WebDAV with SSL. Map the same folder remotely using the WebDAV URL. Test again. Choose the faster method for remote users.
6	Excel Configuration	Add the Z:\ drive as a Trusted Location on all PCs. Optionally convert a test copy of the database to .xlsb for comparison.
7	Pilot Execution	Select 1 employee in Borrazópolis and 1 in Marilândia to use the new system for 2 weeks. Keep the old local Windows shares running as a rollback fallback.
8	Quick-Reference Guide	Create a 1-page PDF with screenshots on how to map the drive and add the Trusted Location (documentation deferred, but this is essential for the pilot).

# Final Summary of Phase 1 Decisions
Acceptance Criteria for Samba: We will use Samba unless it feels painfully slow during the pilot. If slow, we deploy WebDAV.

Macros: The main bottleneck is Trusted Locations—we guarantee this is set up correctly.

Locking: Rely on Samba defaults + a verbal communication rule for the Cadastro file.

Infrastructure: Ubuntu 26.04 LTS + Tailscale + Static IP (Static IP is optional but can serve as a reliable fallback for SSH/WebDAV).