# Consolidated archive - initial migration plan

## Context

This document consolidates the earlier notes on the initial migration proposal for moving from a local-only file-sharing model to a remote-friendly model for the office operation between Borrazópolis and Marilândia.

## Original objective

The original goal was to allow users to access shared files from Windows machines both locally and remotely, while keeping the workflow as close as possible to the existing one.

The main priorities were:

- keep the solution low cost;
- avoid dependence on paid cloud services;
- preserve the existing use of Excel, Word, and KML files;
- keep conflict management mostly manual;
- avoid a heavy maintenance burden.

## Options evaluated

### Option 1: Ubuntu + Samba + Tailscale

This was the main option initially considered.

It included:

- a physical server in Borrazópolis;
- Ubuntu Server as the operating system;
- Samba for file sharing;
- Tailscale or VPN for secure remote access;
- mapped network drives on Windows machines.

### Option 2: Windows Server variant

A second version explored using Windows Server instead of Ubuntu, mainly to align more naturally with the Windows environment.

This version kept the same overall objective, but increased the importance of licensing, setup effort, and operational complexity.

### Option 3: WebDAV fallback

WebDAV was considered as a fallback if Samba performance over WAN proved insufficient.

This was seen as a practical backup because it can work well over high-latency links and still be mapped as a network location in Windows.

## Main concerns identified during analysis

### 1. Samba performance over WAN

The main technical concern was whether Samba would perform well enough over the internet for daily use.

The proposed approach was:

- test Samba first;
- measure opening and saving of a typical file such as the Cadastro workbook;
- if performance was poor, switch to WebDAV.

### 2. Excel macro compatibility

Because the Cadastro file depends on Excel macros, compatibility with network drives was a major concern.

The recommendations were:

- add the mapped network drive as a Trusted Location in Excel;
- enable AutoRecover;
- test the workbook carefully before going live;
- consider converting a test copy to .xlsb if performance proved problematic.

### 3. File locking and conflict management

The business already relied on a manual conflict-management process.

The recommendation was to keep that manual process but add a simple communication rule during the pilot, such as notifying the team before opening the shared database remotely.

## Phase 1 action plan

The refined Phase 1 plan included the following steps:

1. Prepare the server operating system.
2. Enable automatic security updates and a minimal firewall.
3. Deploy Tailscale on the server and Windows PCs.
4. Configure Samba sharing for the main folder.
5. Test local and remote access from Borrazópolis and Marilândia.
6. If needed, deploy WebDAV as a fallback.
7. Configure Excel trusted locations and AutoRecover.
8. Run a pilot with a small number of users.
9. Create a short reference guide for mapping the drive and configuring Excel.

## Notes on infrastructure choices

The analysis also discussed whether a static IP was necessary.

The conclusion was that a static IP was optional, but useful as a backup for SSH or WebDAV access. Tailscale itself could still work without it.

## Final takeaway

The initial plan was a self-hosted file-sharing approach designed to preserve the existing office workflow while adding remote access. It was later judged to be too heavy, costly, and operationally demanding for the business context.

This archive preserves the technical reasoning, the critical concerns, and the implementation steps that were considered before choosing the Microsoft 365-based approach.
