# Changelog

##### Legend  
- **I:** Improvement  
- **B:** Bugfix 
- **F:** New Feature  
- **C:** Change  

---

##### V1.0.85 – 10.06.2025
- **B:** Fix RAM issue when downloading large meta data from SD-Card (CSV), now it use streams and DB operate in WAL mode.
- **C:** Correct sorting on metrices in metadata dispatcher (UI)
- **I:** Add PTZ movment metric
  
---

##### V1.0.82 – 06.06.202
- **F:** Add image to FTP feature
- **F:** Update goxis lib fixes SD-Card release callback issue
- **I:** Update golang version to latest

---

##### V1.0.73 – 06.06.2025
- **I:** Add loading indicator for csv download on sd-card metadata dispatch

---

##### V1.0.72 – 06.06.2025
- **I:** Add SD-Card sink to metadata dispatcher

---

##### V1.0.71 – 03.06.2025
- **B:** Fix an issue with event countdown that can lead into deadlock when stopping multiple times fast.

---

##### V1.0.7 – 01.06.2025
- **I:** Improve Image loading for SD-Card feature
- **B:** Fix image deletion regarding to database entries for SD-Card feature (File was deleted but not DB entrie)

---


##### V1.0.6 – 25.05.2025
- **I:** Add UI Popup for counting rule occupancy in area

---

##### V1.0.5 – 22.05.2025
- **I:** Add config import / export

---

##### V1.0.4 – 22.05.2025
- **I:** Add seven.io voice ringtime

---

##### V1.0.3 – 22.05.2025
- **I:** Add seven.io balance check before perform api call (sms, voice)

---

##### V1.0.2 – 22.05.2025
- **I:** Add doc link for event trigger component
- **B:** Fix race condition in countdown feature

---

##### V1.0.1 – 22.05.2025
- **C:** 12.4 SDK Adding to build pipeline

---

##### V1.0.0 – 21.05.2025
- **C:** Eula Logo remove
- **I:** Better error message in case of img size could not get via VAPIX (sd-card feature)
- **B:** Prevent save for notifications only when trigger is set
- **C:** Readd toast notificaton for test sms success
  
---

##### V0.9.9 – 15.05.2025
- **C:** Remove zone count reset on Occupancy Counting start
- **C:** UI Improvments on Occupancy Counting

---

##### V0.9.8 – 13.05.2025
- **C:** Remove crossline occupancy because its merged with Occupancy Counting

---

##### V0.9.7 – 12.05.2025
- **F:** Occupancy Counting

---

##### V0.9.6 – 12.05.2025
- **F:** Add Cross Line Occupancy

---

##### V0.9.5 – 9.05.2025
- **I:** Change Teltonika SMS to notification service, including seven.io sms and phone calls

---

##### V0.9.4 – 6.05.2025
- **F:** Add Teltonika SMS

---

##### V0.9.3 – 6.05.2025
- **I:** Additional JSON Payload for HTTP Metadata Dispatch

---

##### V0.9.2 – 6.05.2025
- **I:** Prevent Cert delete when its in use

---

##### V0.9.1 – 6.05.2025
- **I:** OPC UA Support for metadata dispatch

---

##### V0.9.0 – 24.04.2025
- **I:** Individual feature licensing: features can now be purchased à la carte.

---

##### V0.8.0 – 16.04.2025
- **F:** Introduce Metadata Dispatch
- **C:** move metadata to https into the new Metadata Dispatch
- **B:** Font loading issue 

