# 🚦 Stop Sign Behavior Annotation Protocol  
**Assignment:** Nexar Home Assignment — Stop Sign Behavior  
**Candidate:** Omer Ben Simon (Machine Learning Model Evaluation Engineer, Computer Vision)  

---

## 1. Purpose  
- This protocol provides clear and objective labeling rules for stop-sign encounters.
- It ensures consistent, high-quality annotations that directly improve driver-behavior models and, downstream, enhance the accuracy of Nexar’s Live HD Maps.

---

## 2. Input Available  
- Annotators will receive **video footage only**.  
- No metadata (GPS, speed, IMU, external sensors) is available.  
- Clips are pre-flagged as potentially involving a stop sign; annotators must confirm behavior.  

---

## 3. Label Categories  
Annotators must assign **exactly one label** per stop-sign encounter:  

| Label | Definition | Example |
|-------|------------|---------|
| ✅ **Full Stop** | Vehicle comes to a *complete halt* before the stop line/intersection. Even a short halt (<1s) counts. | Car fully stops, wheels stationary, then proceeds. |
| ⚠️ **Rolling Stop** | Vehicle slows noticeably but never fully stops. Wheels continue moving. | Car decelerates but rolls at low speed through the sign. |
| ❌ **No Stop** | Vehicle passes the stop sign without slowing. | Car drives straight through at normal speed. |
| ❓ **Unclear / Occluded** | Behavior cannot be reliably judged due to poor visibility, occlusion, or video quality. | Stop sign blocked, video too dark/shaky. |

---

## 4. Annotation Instructions  
1. Watch the clip fully.  
2. Focus on the **driver’s behavior relative to the stop sign**.  
3. Assign **one label** from the table above.  
4. Replay once if needed. If still uncertain → mark **Unclear**.  

---

## 5. Simplifications & Assumptions  
- Do **not** measure stop duration (binary only: full vs not).  
- Ignore turn direction (left/right/straight).  
- Ignore pedestrians or other cars unless they block visibility.  
- Assume no more than one stop-sign encounter per clip.  
- Approximate judgments are sufficient; consistency is more important than precision.  

---

## 6. Trade-offs  
- While a detailed protocol could capture richer features such as timestamps, turning directions, or multi-vehicle interactions, it would increase annotator burden and introduce higher label noise.
- For production pipelines, the 4-label scheme (Full Stop, Rolling Stop, No Stop, Unclear) was chosen to maximize consistency, reduce ambiguity, and scale efficiently.
---

## 7. Why This Protocol Supports Effective Model Training  
- **Consistency:** Clear definitions lead to high inter-annotator agreement → less label noise.  
- **Strong training signal:** The distinction between *full stop*, *rolling stop*, and *no stop* directly aligns with driver compliance modeling.  
- **Error handling:** “Unclear” option prevents mislabeled data, which harms models more than fewer samples.  
- **Production mindset:** Balances annotation efficiency with the need for accurate, reliable ground truth to evaluate and improve ML models.  

---

## ✅ Summary for Annotators  
- Watch → Decide → Label once.  
- Categories: **Full Stop / Rolling Stop / No Stop / Unclear**.  
- Be consistent and objective.  
- If in doubt → mark **Unclear**.  

---

## 📎 Appendix: Alternative Detailed Protocol (Optional)  
In some projects, richer annotations may be useful. Below is an alternative design capturing additional features.  

**Fields to capture:**  
- **Stop Sign Visibility**: timestamp first visible, timestamp out of sight.  
- **Vehicle Behavior**: Did vehicle slow? (Yes/No), Complete stop? (Yes/No), Number of “inch-up” stops.  
- **Driver Action**: Direction after stop (Left / Right / Straight).  
- **Other Drivers**: Presence and position (Opposite / Left / Right), whether they stopped.  
- **Collisions / Near-Collisions**: Event type, timestamp, collision location (Front / Back / Left / Right), vehicles involved and their movement directions.  

**Pros:** Very rich data, supports multiple downstream tasks.  
**Cons:** Increases annotation time and inconsistency; lowers inter-annotator agreement.  

## **Conclusion:** 
- Selecting the simple 4-label protocol reflects a production-oriented mindset: prioritizing annotation quality, consistency, and scalability over annotation richness.
- This balance ensures reliable ground truth for evaluating ML models and supports Nexar’s broader mission of building accurate, real-time HD maps at scale.
