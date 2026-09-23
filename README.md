# NG PULSE — Project Website

**Rail-Signature Workload Integrity Monitor**  
Group 12 · EEL 4914 Senior Design I / II · University of Central Florida  
Sponsored by Northrop Grumman Corporation · Fall 2026 – Spring 2027

🌐 **Live site:** http://maverick.eecs.ucf.edu/seniordesign/fa2026sp2027/g12

---

## Team

| Name | Major | Role |
|---|---|---|
| Joseph Esposito | Electrical Engineering | Analog front end, SAADC integration |
| Dylan Ruiz | Electrical Engineering | Clock distribution, power FET, verification |
| Sean Kenney | Computer Engineering | Ping-pong buffer RTL, feature extractor |
| Jacob Daleandro | Computer Engineering | Classifier RTL, policy FSM, nRF firmware, BLE |

**Faculty Advisor:** Chung Yong Chan — UCF ECE  
**DRACO Lab:** Dr. Mike Borowczak — UCF  
**Sponsor:** Northrop Grumman Corporation

---

## Repo Structure

```
ng-pulse-website/
├── index.html                  # Main project website
├── Divide_and_Conquer.pdf      # SD1 deliverable
├── auto-deploy.sh              # Deploy script (runs on maverick via cron)
├── README.md                   # This file
└── .github/
    └── workflows/
        └── deploy.yml          # GitHub Actions workflow (currently unused)
```

As deliverables are completed throughout SD1 and SD2, PDFs and other files are added to this repo and automatically deployed to the live site.

---

## How Deployment Works

This repo uses a **pull-based auto-deploy** system. Rather than pushing files to the UCF server (which is on a private internal network unreachable from GitHub's servers), the UCF server pulls from GitHub automatically.

```
git push to GitHub (main branch)
        ↓
Within 60 seconds, maverick detects the new commit via GitHub API
        ↓
maverick runs git pull origin main
        ↓
Live site is updated automatically
```

A cron job runs every minute on the maverick server, comparing the latest GitHub commit SHA against the last deployed SHA. If they differ it pulls; if not it does nothing.

### Deploy log

To check deployment status, SSH into maverick and run:

```bash
cat /tmp/ng-pulse-deploy.log
```

To watch it in real time after a push:

```bash
watch -n 5 cat /tmp/ng-pulse-deploy.log
```

---

## Making Updates

### Updating the website

1. Edit `index.html` locally
2. Commit and push to main:

```bash
git add .
git commit -m "your message"
git push origin main
```

3. Within 60 seconds the live site reflects your changes. No manual upload needed.

### Adding a new document

1. Add the PDF or file to the repo root
2. Update the relevant `href` link in `index.html` to point to it
3. Commit and push — the file and updated HTML deploy together

### Adding a YouTube video

When a video is uploaded to YouTube, find the embed URL:
```
https://www.youtube.com/embed/VIDEO_ID
```

In `index.html` find the relevant video placeholder comment and replace it:

```html
<!-- Before -->
<!-- <iframe src="https://www.youtube.com/embed/VIDEO_ID" allowfullscreen></iframe> -->

<!-- After -->
<iframe src="https://www.youtube.com/embed/YOUR_ACTUAL_ID" allowfullscreen></iframe>
```

Commit and push.

---

## Server Details

| Field | Value |
|---|---|
| Server | maverick.eecs.ucf.edu |
| IP | 10.173.204.134 (UCF internal only) |
| Web root | /var/www/html/seniordesign/fa2026sp2027/g12 |
| Username | fa26sp27g12 |
| Access | UCF WiFi or VPN required |

### Manual deploy (fallback)

If the cron job ever stops working, deploy manually from any machine on UCF WiFi:

```bash
sftp fa26sp27g12@10.173.204.134
# then inside sftp:
put index.html
put Divide_and_Conquer.pdf
exit
```

### Re-SSHing into maverick to check things

```bash
ssh fa26sp27g12@10.173.204.134
```

---

## Required Website Content

Tracked here for reference. All items must remain accessible per UCF ABET requirements — do not delete any content after graduation.

### SD1 — Fall 2026
- [x] Divide & Conquer Document
- [ ] Midterm Milestone Report
- [ ] SD1 Final Report
- [ ] Mini Demo Video (YouTube embed)

### SD2 — Spring 2027
- [ ] CDR Presentation Video (YouTube embed)
- [ ] CDR Presentation Slides
- [ ] Midterm Demonstration Video (YouTube embed)
- [ ] 8-Page Conference Paper
- [ ] SD2 Final Report
- [ ] Final Presentation Video (YouTube embed)
- [ ] Final Presentation Slides
- [ ] Final Demonstration Video (YouTube embed)

---

## Notes

- The server is on UCF's private internal network (`10.x.x.x`). It cannot be reached from off campus without UCF VPN.
- The PAT token embedded in the git remote URL on maverick is stored only on the server — do not commit it to this repo.
- All website content must remain accessible after graduation per UCF ABET policy.