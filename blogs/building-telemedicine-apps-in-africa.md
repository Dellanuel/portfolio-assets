## A Different Kind of Engineering Challenge

Building a telemedicine app in San Francisco and building one for Lagos are fundamentally different engineering problems. When I started working on DOC!, I quickly discovered that the playbook from Western tech does not apply here.

## The Bandwidth Problem

The average mobile connection in Nigeria fluctuates between 3G and spotty 4G. A consultation app that buffers for 30 seconds is an app patients will abandon — and in healthcare, abandonment can have serious consequences.

### Our Approach: Offline-First Architecture

Every critical interaction in DOC! works offline and syncs when connectivity returns.

- **Consultation notes** are saved locally in SQLite, then pushed to Firebase when a connection is detected
- **Medical images** are compressed client-side before upload, with a queue that retries failed uploads
- **Prescription data** uses a delta-sync approach — only changes are transmitted, not entire records

We reduced our average payload size by 73% through aggressive response compression and field selection on our API calls.

## Trust and Verification

In a market where counterfeit drugs are a genuine threat, patients need to trust the platform. This is not just a UX problem — it is an engineering one.

We implemented a three-layer verification system:

1. **Doctor verification** — License numbers cross-referenced against the Medical and Dental Council database
2. **Pharmacy verification** — GPS coordinates matched against registered pharmacy locations from NAFDAC data
3. **Prescription tracking** — Every prescription generates a unique code that patients can verify independently

## Designing for Real Devices

Our analytics showed that 68% of users access DOC! on devices with 2-3GB of RAM running Android 10 or older. This drove several technical decisions:

- **Image caching** with size limits to prevent storage bloat
- **Lazy loading** for consultation history — only the last 10 sessions load initially
- **Minimal background services** to avoid being killed by aggressive Android battery managers

## What I Learned

Healthcare software in emerging markets is not about building with fewer features. It is about building with deeper empathy for the constraints your users face. Every millisecond of load time, every megabyte of storage, every failed network request has a human consequence.

The technical problems are fascinating. The human impact makes solving them meaningful.
