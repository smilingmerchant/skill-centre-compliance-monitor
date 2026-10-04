# skill-centre-compliance-monitor
# AI-Based Compliance Monitoring for Skill Training Centres

**Smart India Hackathon 2026 | PS 26245 | MSDE**

Checks that trainees are really present and approved equipment is really available, with or without CCTV, while protecting privacy.

> **Status:** Hackathon prototype. Items marked *(proposed)* are planned. Accuracy numbers are *to be measured*.

---

## Problem

The government pays skill centres for trainee attendance and equipment. Inspections are rare, so two things go unnoticed for months:

- **Proxy attendance:** a trainee is marked present but never came, or a friend marks for them.
- **Missing equipment:** the approved lathe or computers are missing, broken, or shown only on inspection day.

CCTV, where it exists, only records. It is not linked to attendance.

## Our Solution

**Trust, but verify.** Every attendance mark needs proof, and the centre's claims are checked against evidence it cannot easily control.

- Works **with or without CCTV**
- Each record becomes **Verified / Needs review / Suspicious**
- Officers see **only the problems**, not raw video
- **A human officer decides. No automatic penalty.**

## How It Works

```
Phone App + Camera + Beacon/Sensors -> small signed message (~1 KB) -> Verification Engine -> Officer Dashboard
```

| Part | Job |
|---|---|
| Trainee App | Proves the right person, alive, is in the room |
| Room Beacon (ESP32) | Shows a 6-digit code that changes every 30 s |
| Edge Unit | Counts people and matches faces from CCTV, inside the centre |
| Sensors | Show if machines are really running |
| Verification Engine | Compares all evidence with the centre's register |
| Dashboard | Map, alerts and follow-up for officers |

**5 proofs per check-in:** room code, face match, liveness (blink/turn), peer phones nearby, genuine phone. One weak signal never raises a flag.

## Tech Stack

| Area | Technology | Why |
|---|---|---|
| **Frontend** | React, Leaflet, Recharts (Streamlit for prototype) | Fast, clear officer dashboard with map and charts |
| **Mobile app** | Flutter, TFLite, MediaPipe/ML Kit | One codebase, face and liveness work offline on the phone |
| **Backend** | FastAPI, PostgreSQL (PostGIS), Redis + Celery | Fast, reliable, handles many check-ins at once |
| **Messaging** | MQTT (Mosquitto) | Built for weak internet, supports offline |
| **Evidence storage** | MinIO / S3 | Stores small blurred thumbnails only |
| **Face AI** | MobileFaceNet, ArcFace, InsightFace, MiniFASNet | Face match and fake-face detection |
| **Camera AI** | YOLO, ByteTrack | Counts people and equipment, no double counting |
| **Edge hardware** | Mini PC / Jetson, ONNX | Video stays inside the centre |
| **Room proof** | ESP32, BLE, TOTP | Cheap (~₹400). Forwarded codes expire in 30 s |
| **Sensors** | PZEM-004T, vibration sensor | Proof a machine actually ran |
| **Decision engine** | scikit-learn, XGBoost, Isolation Forest, MLflow | Simple, explainable scoring and pattern checks |
| **Security** | TLS, AES-256, JWT, audit logs, Play Integrity | Protects data and blocks fake phones |
| **Deployment** | Docker Compose | Same setup on any server |

## APIs

| API | Used for | Status |
|---|---|---|
| Google Play Integrity API | Checks the phone and app are genuine | In design |
| Our FastAPI REST APIs | Check-in, alerts, reports, dashboard | In design |
| MQTT / HTTPS | Events from phones and Edge Unit | In design |
| Aadhaar eKYC (where permitted) | Identity at enrolment | Needs legal approval, **not integrated** |
| Push notifications | Random check-in prompt | Service not decided *(proposed)* |
| Scheme / government APIs | Link with scheme systems | Planned for National phase, **not validated** |

## Privacy

- **Headcount by default.** Faces are matched only for consenting, enrolled trainees.
- **Only face numbers (embeddings) are stored.** No photos, no video.
- **Raw video never leaves the centre.** Only small packets and blurred thumbnails of flags.
- Designed around India's DPDP Act 2023. Consent wording must be confirmed with legal experts.

## Limitations

- Bluetooth relay is possible in theory. We make it costly and detectable, not impossible.
- Face match can fail in poor light or on old phones. Those cases go to **review**, not accusation.
- Android first. Needs a small one-time hardware cost per centre (beacon about ₹400, indicative).
- It tells inspectors where to look. It does not replace them.




## Team

- **Team name:** *STARK_01*
- 

---


