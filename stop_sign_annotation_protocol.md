# 🚦 Stop Sign Behavior Annotation Protocol  
**Assignment:** Nexar Home Assignment — Stop Sign Behavior  
**Candidate:** Omer Ben Simon (Machine Learning Model Evaluation Engineer, Computer Vision)  

---

## 1. Purpose  
We are training a machine learning model to analyze driver behavior at stop signs.  
This protocol defines clear annotation instructions for remote annotators to ensure **high-quality, consistent labels** that support downstream model training and evaluation.  

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
- **Simplicity vs Richness:**  
  - A simple 4-label scheme maximizes inter-annotator agreement and reduces noise.  
  - A more detailed protocol (timestamps, directions, inch-up stops) could capture richer features but would slow annotation and increase inconsistency.  
- **Decision:** For this task, simplicity was chosen to ensure data quality and minimize ambiguity.  

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

**Conclusion:** While the detailed protocol offers flexibility, for production pipelines we prioritize the **simple 4-label scheme** to maximize consistency, data quality, and annotation efficiency.  
