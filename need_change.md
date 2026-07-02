Dưới đây là các phần nên chỉnh và hướng chỉnh, **chưa viết lại nội dung**, chỉ xác định vị trí và cách chỉnh theo đúng các ý bạn đưa ra. Mình dựa trên cấu trúc hiện tại của PDF M3: benchmark nằm ở Introduction/Table 0.1, VSS theory ở Chapter 2, VSS ingest/query/persistence ở Chapter 3, và limitation/evaluation ở Chapter 4 + Conclusion. 

---

# 1. Benchmark — nên chỉnh ở đâu và chỉnh như thế nào?

## Phần nên chỉnh

Nên chỉnh chủ yếu ở:

**Introduction → 1. Motivation → Table 0.1 Existing technical solutions benchmark by requirement group**

Ngoài ra có thể chỉnh nhẹ thêm ở:

**Introduction → 2. Contribution of the thesis**

**Chapter 1 → 1.4 Related literature and architectural positioning**

## Nhận xét hiện tại

Benchmark hiện tại đã có cấu trúc khá ổn vì nó chia theo các **requirement groups** như centralized monitoring, AI-assisted access control, event detection, investigation/playback, reporting, scalable deployment, extensibility. Cách này phù hợp hơn so với benchmark theo từng sản phẩm đơn lẻ.

Tuy nhiên, điểm yếu hiện tại là bảng mới chủ yếu nêu:

**Project operational need → Reference products/benchmark insight → Remaining gap**

Tức là bảng cho thấy thị trường còn thiếu gì, nhưng chưa làm nổi bật rõ **project của nhóm giải quyết gap đó như thế nào**. Người đọc có thể hiểu là “các sản phẩm hiện tại có gap”, nhưng chưa thấy rõ “vậy nhóm mình mạnh ở đâu, đóng góp cụ thể là gì”.

## Cách chỉnh

Nên thêm một cột mới vào Table 0.1, ví dụ:

**Project response / Proposed advantage**

hoặc:

**How this project addresses the gap**

Cột này nên nêu rõ điểm mạnh của project theo từng requirement group, nhưng phải giữ mức claim đúng với prototype, không viết quá production-ready.

Ví dụ hướng nội dung cho từng nhóm:

| Requirement group                       | Nên làm rõ điểm mạnh project                                                                                                                  |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Centralized monitoring and management   | Project không chỉ quản lý camera/playback, mà tạo pipeline Edge → VMS → VSS để clip có thể được capture, review, analyze trong cùng workflow. |
| AI-assisted access control and identity | VSS có identity-aware memory bằng DeepFace/face_id, nhưng có cơ chế unresolved/missing_faceid để tránh overclaim khi không đủ bằng chứng.     |
| Event detection and alerting            | Edge Agent giúp chuyển continuous RTSP stream thành event-based clips, giảm tải review và bandwidth.                                          |
| Investigation/playback/object tracing   | VMS playback gắn với Deep Analyze và clip-specific query, giúp người dùng không cần rời khỏi playback context để hỏi AI.                      |
| Reporting/visibility                    | Event summaries, metadata, identities, relations có thể trở thành nền cho report và dashboard sau này.                                        |
| Scalable deployment                     | Kiến trúc local-first/cloud-connected: edge xử lý gần camera, VMS lưu/playback cloud, VSS xử lý tri thức chọn lọc.                            |
| Extensibility                           | Hybrid database MongoDB + Neo4j giúp mở rộng semantic search, graph reasoning, identity tracking và customer-specific rules.                  |

Sau bảng, nên thêm một đoạn ngắn để kết luận benchmark. Đoạn này nên nói rõ:

Project của nhóm không thắng ở việc có đầy đủ mọi tính năng như commercial VMS, mà mạnh ở **integration logic**: biến camera stream thành event clip, biến event clip thành playback evidence, rồi biến playback evidence thành searchable/answerable surveillance knowledge.

Đây là điểm cần nhấn mạnh vì nó bảo vệ project tốt hơn: nhóm không cạnh tranh với Verkada/Eagle Eye/Avigilon ở mức sản phẩm hoàn chỉnh, mà đóng góp ở **workflow tích hợp Edge–VMS–VSS**.

---

# 2. Re-ID, tracking person/object across camera — nên chỉnh ở đâu và chỉnh như thế nào?

## Phần nên chỉnh

Nên chỉnh ở các vị trí sau:

**Chapter 2 → 2.2.3 Hybrid retrieval and identity-aware surveillance knowledge**

**Chapter 3 → 3.2.1 Ingest flow and identity lifecycle**

**Chapter 3 → 3.2.3 Persistence and hybrid database design**

**Chapter 4 → 4.4.1 Metadata Extraction**

**Chapter 4 → 4.4.3 VSS Agent Latency**

**Conclusion → limitation and future work**

## Nhận xét hiện tại

Báo cáo hiện tại đã nêu khá tốt rằng `face_id` là identity object quan trọng và có trạng thái `missing_faceid` khi không đủ bằng chứng. Tuy nhiên, phần tracking person/object hiện vẫn hơi nghiêng về **face_id-centric identity**.

Điểm cần chỉnh là: trong surveillance thực tế, tracking một người không thể chỉ dựa vào mặt. Có nhiều trường hợp:

Người quay lưng.

Mặt bị che.

Camera góc cao hoặc xa.

Ánh sáng kém.

Người chỉ xuất hiện một phần cơ thể.

Người xuất hiện qua nhiều camera nhưng không có frontal face.

Trong các trường hợp này, nếu chỉ dựa vào `face_id`, hệ thống sẽ mất khả năng liên kết person observation. Vì vậy báo cáo nên làm rõ rằng:

`face_id` là **primary identity anchor** khi đủ bằng chứng mặt, nhưng không phải là cue duy nhất để tracking.

## Cách chỉnh trong Chapter 2 → 2.2.3

Nên thêm một đoạn sau phần nói về `missing_faceid` hoặc sau Table 2.1.

Ý cần thêm:

VSS sử dụng `face_id` như bằng chứng mạnh nhất cho identity continuity, nhưng trong surveillance, identity continuity còn cần contextual cues như clothing color, carried objects, body appearance, movement direction, camera, timestamp, location, co-occurrence, và event context. Khi không có mặt rõ, hệ thống không nên gán thành verified identity, mà nên lưu thành `PersonObservation` hoặc `unresolved person` với contextual descriptors.

Nên phân biệt ba mức:

| Mức tracking           | Evidence                                                                             | Cách hệ thống nên xử lý                                           |
| ---------------------- | ------------------------------------------------------------------------------------ | ----------------------------------------------------------------- |
| Verified identity      | Face embedding match đạt threshold                                                   | Gán/reuse `face_id`, merge vào Person identity                    |
| Contextual candidate   | Không rõ mặt nhưng có quần áo, đồ vật, thời gian, vị trí, hướng di chuyển tương đồng | Lưu dạng possible/candidate match, không khẳng định là cùng người |
| Unresolved observation | Không đủ mặt và context yếu                                                          | Gán `missing_faceid`, giữ event-local observation                 |

Cách viết nên nhấn mạnh: đây là **evidence integrity**, không phải lỗi hệ thống. Tốt hơn là nói “unresolved” thay vì gán sai identity.

## Cách chỉnh trong Chapter 3 → 3.2.1

Ở ingest flow hiện tại, nên thêm rõ một bước logic sau identity detection/reconciliation:

**contextual observation enrichment**

hoặc:

**non-face person observation handling**

Bước này không nhất thiết phải claim đã hoàn chỉnh nếu code chưa làm mạnh. Có thể mô tả là current/future design tùy tình trạng thực tế.

Nội dung cần có:

Khi DeepFace không trả về verified face nhưng VLM vẫn thấy person, hệ thống vẫn tạo `PersonObservation`.

`PersonObservation` nên chứa: clothing color, accessories, carried objects, body direction, visible posture, camera_id, date, time_range, location, nearby objects/persons.

Các observation này có thể dùng để hỗ trợ tracking mềm, nhưng không được merge vào `Person` global nếu không có evidence đủ mạnh.

Nếu muốn diễn giải cho defense, logic nên là:

**Face ID gives strong identity truth. Context gives supporting evidence. Context alone can suggest candidate continuity, but it should not become verified identity.**

## Cách chỉnh trong Chapter 3 → 3.2.3

Trong persistence design, nên làm rõ hơn sự khác biệt giữa:

`Person`: verified identity, có `face_id`.

`PersonObservation`: người quan sát được trong một event cụ thể, có thể chưa có face_id.

`candidate_identity_link`: liên kết nghi vấn/candidate giữa nhiều observation nếu context giống nhau.

Nếu hiện tại chưa có `candidate_identity_link` trong implementation, nên ghi là **future extension**, không nên claim là đã fully implemented.

Có thể thêm một bảng nhỏ:

| Object            | Khi nào tạo                                   | Có được merge không?                  | Ghi chú                           |
| ----------------- | --------------------------------------------- | ------------------------------------- | --------------------------------- |
| Person            | Có verified `face_id`                         | Có, theo `face_id`                    | Identity-level object             |
| PersonObservation | Thấy người trong event nhưng chưa verify face | Không merge trực tiếp vào Person      | Event-local evidence              |
| Candidate link    | Nhiều observation có context giống nhau       | Chỉ là possible link                  | Không dùng như biometric truth    |
| Object            | Đồ vật trong event                            | Event-scoped nếu chưa có object re-id | Tránh overclaim object continuity |

## Cách chỉnh trong Chapter 4 và Conclusion

Ở Chapter 4, đặc biệt phần VSS limitation, nên thêm rõ:

Crowded/outlier không chỉ làm latency tăng, mà còn làm identity quality giảm do face occlusion, rear-view, small face, multi-person ambiguity.

Với người không rõ mặt, hệ thống có thể mô tả và lưu observation, nhưng không thể truy vết chắc chắn như verified face_id.

Future work nên gồm: clothing-based person re-identification, object association, temporal-camera transition constraints, multi-camera calibration, tracklet linking, và confidence scoring.

Trong Conclusion, nên thêm một câu hạn chế kiểu:

The current VSS treats face_id as the strongest identity anchor, but person tracking remains limited when faces are occluded, rear-facing, or absent. In such cases, contextual cues such as clothing, carried objects, camera-time continuity, and co-occurrence should be used as supporting evidence rather than as verified identity proof.

---

# 3. Merge data logic — nên chỉnh ở đâu và chỉnh như thế nào?

## Phần nên chỉnh

Nên chỉnh chủ yếu ở:

**Chapter 3 → 3.2.1 Ingest flow and identity lifecycle**

**Chapter 3 → 3.2.3 Persistence and hybrid database design**

Có thể chỉnh nhẹ ở:

**Chapter 2 → Table 2.1 VSS surveillance memory layers**

**Chapter 4 → VSS evaluation limitation/future work**

## Nhận xét hiện tại

Báo cáo hiện tại đã nói có temporal grouping, identity reconciliation, MongoDB identities, MongoDB vss_event, Neo4j graph, processed_videos. Nhưng “merge logic” vẫn chưa đủ rõ.

Cụ thể, cần trả lời rõ các câu hỏi:

Khi nào hai face observations được xem là cùng một identity?

Khi nào hai event records được grouped/merged?

Khi nào chỉ được liên kết bằng graph chứ không merge?

Khi nào không được merge để tránh sai evidence?

## Cách chỉnh: thêm một subsection mới

Nên thêm một mục nhỏ trong **3.2.3 Persistence and hybrid database design**, ví dụ:

**3.2.4 Data merging and identity-resolution policy**

hoặc nếu không muốn thêm số mục mới thì thêm đoạn ngay cuối 3.2.3 trước/ sau Table 3.3.

Mục này nên chia thành ba loại merge:

---

## A. Identity merge logic

Nên nêu:

Identity merge dựa trên `face_id` hoặc face embedding match vượt threshold.

Nếu face embedding match đạt threshold → reuse existing `face_id`.

Nếu không đạt threshold → tạo new `face_id`.

Nếu không đủ mặt → không tạo verified identity, chỉ tạo `missing_faceid` hoặc `PersonObservation`.

Khi merge identity, chỉ merge các thông tin ổn định như:

`face_id`

embedding vector hoặc embedding history

appearance_refs

seen camera/date/location/time

last_seen_at

identity status

Không nên merge trực tiếp các mô tả event-specific như “red shirt”, “holding bag”, “standing near door” vào identity global, vì các thuộc tính này có thể thay đổi theo từng event.

Nên viết rõ nguyên tắc:

**Stable identity facts are stored at identity level; transient visual descriptions are stored at event-observation level.**

---

## B. Event merge/grouping logic

Nên nêu rõ event data merge không dựa trên face_id, mà dựa trên context của event:

`camera_id`

`date`

`location`

`time_range`

`source_id`

`graph_id`

`event_type`

temporal overlap hoặc time gap gần nhau

Ví dụ:

Nếu nhiều frame observations thuộc cùng clip hoặc cùng time window gần nhau → temporal_grouping merge thành một event summary.

Nếu hai event cùng ngày, cùng camera, cùng location và time_range gần nhau → có thể thuộc cùng `GraphGroup`.

Nếu khác camera nhưng time gần nhau và có candidate person/object continuity → không merge event, chỉ tạo relation/candidate link.

Điểm quan trọng: **merge event** và **link event** là hai khái niệm khác nhau.

Nên giải thích:

Merge dùng khi các observations là một phần của cùng event.

Link dùng khi các events khác nhau nhưng có quan hệ điều tra, ví dụ cùng person, cùng object, cùng camera-date-location, hoặc có candidate tracking.

---

## C. Graph merge/link logic

Trong Neo4j, nên làm rõ:

`Event` node nên được unique bằng `source_id`.

`GraphGroup` nên được tạo theo composite key như `camera_id + date + location`.

`Person` node unique bằng `face_id` nếu verified.

`PersonObservation` unique theo event-local key.

`Object` nên event-scoped nếu chưa có object re-id.

Các relation như `INVOLVES`, `SEEN_WITH`, `RELATED_TO`, `CAPTURED_BY`, `OCCURRED_AT` giúp liên kết evidence mà không cần merge sai node.

Nên nhấn mạnh:

GraphDB không chỉ dùng để “lưu dữ liệu”, mà còn để tránh merge sai bằng cách giữ các node riêng và nối chúng bằng typed relationships.

---

# 4. Gợi ý bảng nên thêm vào báo cáo

Để phần merge logic rõ hơn, nên thêm một bảng trong Chapter 3, sau Table 3.3 hoặc trước Table 3.3.

Tên bảng gợi ý:

**Table 3.x. VSS data merging and linking policy**

Cấu trúc bảng nên như sau:

| Data target             | Merge key / evidence                     | Merge action                                      | Do not merge when                                             |
| ----------------------- | ---------------------------------------- | ------------------------------------------------- | ------------------------------------------------------------- |
| Identity document       | `face_id`, face embedding threshold      | Reuse or update identity history                  | Face unclear, low confidence, rear-view                       |
| Person node             | Verified `face_id`                       | Merge as same person                              | Only clothing/context is available                            |
| PersonObservation       | Event-local observation                  | Keep as event-scoped evidence                     | Do not promote to Person without face verification            |
| Event document          | Same source/clip or close temporal group | Merge frame-level observations into event summary | Different clips/time windows without strong continuity        |
| GraphGroup              | Same camera-date-location                | Group events for traversal                        | Different location/camera unless linked by relation           |
| Object node             | Event-local object key                   | Keep object relation in event                     | Do not assume same object across cameras without object re-id |
| Candidate tracking link | Clothing/object/time/location similarity | Store possible relation with confidence           | Do not present as verified identity                           |

Bảng này sẽ giúp hội đồng hiểu rõ hệ thống “merge” theo luật nào, tránh cảm giác dữ liệu bị gom tùy tiện.

---

# 5. Thứ tự ưu tiên chỉnh

Nếu bạn muốn chỉnh theo mức quan trọng, mình đề xuất thứ tự như sau:

**Ưu tiên 1:** Sau bảng Table 0.1 benchmark — thêm phần nội dung về (nội dung) “Project response / Proposed advantage”. Đây là phần giúp làm bật giá trị project nhóm ngay từ đầu.

**Ưu tiên 2:** Chapter 3.2.3 — thêm data merging/linking policy. Đây là phần quan trọng nhất để bảo vệ kiến trúc MongoDB + Neo4j + identity.

**Ưu tiên 3:** Chapter 2.2.3 và 3.2.1 — chỉnh lại Re-ID để không quá phụ thuộc vào face_id, bổ sung contextual cues và unresolved observation.

**Ưu tiên 4:** Chapter 4 + Conclusion — thêm limitation và future work về rear-view/no-face/crowded scenes/context-based tracking.

Tóm lại, hướng chỉnh chính là: **benchmark phải làm bật điểm mạnh tích hợp của project; Re-ID phải chuyển từ “face_id-only” sang “face_id as strongest anchor + context as supporting evidence”; merge logic phải tách rõ identity merge, event grouping, và graph linking.**

---

# 6. Bổ sung mới theo yêu cầu chỉnh trọng tâm Introduction, Chapter 1 và VSS ở Chapter 2-4

Phần này là mục theo dõi chỉnh sửa mới. Mục tiêu là **không viết dài thêm báo cáo**, mà dùng nội dung đang có để thay thế, cô đọng, tách rõ mức độ hoàn thành, và bổ sung các bảng/evaluation còn thiếu cho VSS. Khi sửa báo cáo gốc, ưu tiên thay đoạn cũ hoặc gom đoạn lặp thay vì thêm nhiều đoạn dài.

## 6.1 Phân tích nhanh hai file hiện tại

### Báo cáo chuẩn hiện tại

Báo cáo đã có khung khá tốt:

- Introduction đã nêu motivation, benchmark theo requirement group, contribution, milestone và work distribution.
- Chapter 1 đã có research questions, current system overview và Table 1.1 về maturity profile.
- Chapter 2 đã giải thích VSS theory: identity-aware retrieval, MongoDB + Neo4j, direct/vector/graph/hybrid modes, `missing_faceid`.
- Chapter 3 đã mô tả VSS ingest, query routing, persistence object mapping.
- Chapter 4 đã có VSS metadata extraction, answer quality, latency và token-cost evaluation.

Điểm cần chỉnh mạnh:

- Chưa định nghĩa đủ rõ “cloud-native” và “scalable” theo đúng phạm vi prototype.
- VSS còn có claim hơi mạnh về graph reasoning/answer grounding trong khi chưa có ground-truth evaluation sâu, ablation test, hoặc hallucination/faithfulness audit đủ chặt.
- Các kết quả VSS đang trộn giữa completed results, limitations, và future work; cần tách rõ hơn.
- Bảng/hình có lỗi đánh số và caption: List of tables ghi VSS bắt đầu Table 4.5, nhưng trong nội dung VSS lại dùng Table 4.6 trở đi; Figure 3.2 bị caption lặp; một số caption còn lỗi tiếng Anh.
- Security/privacy cho video giám sát, khuôn mặt, identity embedding, relationship graph và hạ tầng cá nhân chưa đủ rõ.

### need_change.md hiện tại

File hiện tại đã xác định tốt ba nhóm chỉnh:

- Benchmark cần thêm cột project response/proposed advantage.
- Re-ID cần chuyển từ face_id-centric sang face_id là evidence anchor mạnh nhất, còn context chỉ là supporting evidence.
- Merge logic cần tách identity merge, event grouping, graph linking.

Phần cần bổ sung thêm là:

- Chỉ rõ vị trí sửa cho cloud-native/scalable trong Chapter 1.
- Chỉ rõ bảng Requirement vs. Actual Results và implemented/partial/future work cho riêng VSS.
- Chỉ rõ ground-truth evaluation và ablation test cho VSS metadata, identity, graph facts, answer faithfulness.
- Chỉ rõ security/privacy cho deployment hiện tại dùng GCS cá nhân, MongoDB/Neo4j cá nhân và VM cá nhân.
- Chỉ rõ các ref cần gắn từ `references.md`.

---

# 7. Vị trí sửa cụ thể và nội dung cần chỉnh

## 7.1 Introduction → Motivation và Contribution

### Vị trí trong báo cáo

- **INTRODUCTION → 1. Motivation**
- **Table 0.1 Existing technical solutions benchmark by requirement group**
- **INTRODUCTION → 2. Contribution of the thesis**

### Cách chỉnh

1. Giữ Table 0.1, nhưng thêm hoặc sửa một cột ngắn:

   **Project response / Current prototype evidence**

2. Nội dung cột mới cần nói đúng mức prototype, không claim production-ready. Ví dụ:

| Requirement group | Project response / Current prototype evidence |
|---|---|
| Centralized monitoring and management | Current evidence is strongest in the Edge → VMS → VSS evidence workflow: event clips can be captured, reviewed, and sent to analysis. |
| AI-assisted identity and investigation | VSS supports DeepFace-based `face_id`, metadata extraction, vector retrieval, and graph persistence, but identity and graph reasoning remain prototype-level and require deeper ground-truth validation. |
| Event detection and alerting | Edge converts continuous RTSP input into event-oriented clips; alert management and production monitoring remain future work. |
| Investigation/playback | VMS playback is the strongest completed operator workflow and provides the entry point for clip-aware VSS analysis. |
| Scalable deployment | The current system is cloud-connected and modular, but only partially cloud-native/scalable because it still uses personal GCS, personal MongoDB/Neo4j and personal VM infrastructure. |
| Extensibility | The MongoDB + Neo4j split supports later extension, but customer-specific rules, monitoring, access control and hardened deployment are future work. |

3. Sau Table 0.1, thay đoạn kết benchmark bằng một đoạn ngắn hơn:

   Project strength is not feature parity with commercial VMS products. The defensible contribution is the integrated evidence pipeline: camera stream → event clip → cloud-backed playback → searchable VSS knowledge. The current prototype demonstrates this integration path, while production-grade scalability, security hardening and deeper VSS evaluation remain future work.

### Citation nên dùng

- Requirement/business context: `[1]`, `[2]`
- VMS/cloud playback: `[6]`, `[7]`, `[30]`, `[31]`, `[34]`, `[35]`
- VSS retrieval/database: `[8]`-`[14]`, `[20]`-`[22]`, `[50]`
- Evaluation/evidence grounding: `[51]`, `[53]`, `[54]`

---

## 7.2 Chapter 1 → định nghĩa “cloud-native” và “scalable”

### Vị trí trong báo cáo

- **Chapter 1 → 1.1 Research questions and project direction**
- **Chapter 1 → 1.2 Current system overview**
- **Table 1.1 Current subsystem maturity profile**

### Vấn đề hiện tại

Chapter 1 có nói cloud-native là deployability, centralized operation và readiness for extension, nhưng vẫn chưa trả lời đủ rõ:

- “Cloud-native” trong đề tài này nghĩa là gì?
- “Scalable” trong đề tài này nghĩa là gì?
- Thành phần nào đã thực sự cloud-native/scalable?
- Thành phần nào mới chỉ là cloud-hosted hoặc prototype?

### Cách chỉnh

Thêm một subsection ngắn sau **1.1 Research questions and project direction**:

**1.x Definition of cloud-native and scalable scope**

Nội dung cần có:

- **Cloud-native trong đề tài**: service boundaries rõ, stateless API where possible, cloud object storage, asynchronous handoff, environment-based configuration, deployability trên VM/cloud.
- **Scalable trong đề tài**: có thể mở rộng số clip/site/query bằng cách tách Edge, VMS, VSS và storage; không đồng nghĩa đã có autoscaling, multi-tenant IAM, observability hay production SLO.
- **Cloud-hosted**: chạy trên VM cá nhân hoặc dùng dịch vụ cloud cá nhân nhưng chưa có IaC, autoscaling, centralized monitoring, secrets management, least-privilege IAM.
- **Prototype**: flow đã chạy hoặc demo được nhưng chưa hardening, chưa stress test rộng, chưa security audit.

Thêm bảng ngay sau định nghĩa:

**Table 1.x. Cloud-native and scalability status by component**

| Component | Current status | Why |
|---|---|---|
| Edge Agent | Prototype / edge-local | Runs near cameras and reduces raw stream volume, but not yet fleet-managed or centrally auto-scaled. |
| VMS playback API | Partially cloud-native | Uses FastAPI, GCS-backed playback, HTTP Range, and service boundary; still deployed on personal VM without production hardening. |
| GCS clip storage | Cloud-native storage component | Object storage is appropriate for centralized clip storage and separation of compute/storage. |
| Pub/Sub handoff | Cloud-native integration pattern | Supports asynchronous handoff, but consumer retry/monitoring remains partial. |
| VSS MongoDB vector store | Cloud-hosted / partial scalable | Supports vector/event retrieval, but deployment and indexes are personal/prototype-level. |
| VSS Neo4j graph store | Cloud-hosted / partial scalable | Supports graph facts and relationships, but graph evaluation and production governance remain incomplete. |
| VSS reasoning agent | Prototype | Demonstrates routing and answering, but requires ground-truth, ablation, faithfulness and hallucination evaluation. |
| Security/privacy controls | Partial / future work | Sensitive surveillance data exists, but deployment uses personal accounts/VM and needs stronger IAM, encryption, retention and audit controls. |

### Citation nên dùng

- GCS/object storage: `[7]`, `[30]`, `[43]`, `[44]`
- Pub/Sub: `[45]`, `[46]`
- MongoDB Vector Search: `[9]`
- Neo4j AuraDB: `[14]`
- OpenAI/Gemini evaluation and model usage: `[49]`-`[54]`
- Security upload boundary: `[47]`

---

## 7.3 Chapter 2 → VSS theory cần siết claim graph reasoning

### Vị trí trong báo cáo

- **Chapter 2 → 2.2.3 Hybrid retrieval and identity-aware surveillance knowledge**
- **Table 2.1 VSS surveillance memory layers**
- **Chapter 2 → 2.2.4 Figure and schema placement notes**

### Cách chỉnh

1. Thay các câu quá mạnh kiểu “graph reasoning” thành “graph-based fact retrieval / relationship traversal at prototype scale” nếu chưa có ground-truth graph QA.

2. Sau đoạn nói về direct/vector/graph/hybrid, thêm một đoạn ngắn:

   In the current prototype, graph retrieval should be interpreted as structured fact lookup and relationship traversal over extracted VSS records, not as fully validated investigative reasoning. Deeper reasoning claims require a ground-truth graph dataset, graph-fact accuracy measurement, and ablation between vector-only, graph-only and hybrid retrieval.

3. Mở rộng Table 2.1 thành 4 memory layers bằng cách thêm dòng:

| Memory layer | Main storage | Main unit | Main question answered |
|---|---|---|---|
| Evaluation memory | Manual ground-truth sheet / test set | Expected metadata, identity labels, graph facts, expected answers | Did VSS extract and answer correctly, and did the answer stay faithful to evidence? |

4. Chuẩn hóa caption:

- `Figure 2.1. VSS baseline architecture based on the ingest/retrieval pattern.`
- `Figure 2.2. VSS LangGraph workflow visualized in LangSmith.`
- `Figure 2.3. VSS data stores: MongoDB Atlas vector/document memory and Neo4j Aura graph memory.`

### Citation nên dùng

- DeepFace: `[8]`
- MongoDB Vector Search: `[9]`
- LangGraph/LangSmith: `[10]`, `[11]`
- Neo4j AuraDB: `[14]`
- RAGAS/evaluation/faithfulness: `[20]`-`[22]`
- OpenAI/Gemini evals: `[51]`, `[53]`, `[54]`

---

## 7.4 Chapter 3 → VSS implementation cần thêm bảng status theo subsystem

### Vị trí trong báo cáo

- **Chapter 3 → 3.2.1 Ingest flow and identity lifecycle**
- **Chapter 3 → 3.2.2 Query routing and four response modes**
- **Chapter 3 → 3.2.3 Persistence and hybrid database design**

### Cách chỉnh

Thêm một bảng ngắn ở cuối **3.2 VSS ingest and query workflows**, sau Table 3.3:

**Table 3.x. VSS subsystem implementation status**

| VSS subsystem | Implemented | Partial | Future work |
|---|---|---|---|
| Video ingest orchestration | LangGraph ingest flow, payload validation, frame sampling, duplicate check | Error handling and observability are still limited | Retry policy, tracing, stage-level metrics |
| Metadata extraction | VLM captioning, structured event summary, people/object/relation extraction | Ground truth is limited and scene diversity is small | Larger labeled dataset with occlusion, low-light, crowd and multi-camera cases |
| Identity handling | DeepFace/ArcFace, `face_id`, `missing_faceid`, identity store | Threshold calibration and cross-camera identity accuracy are not deeply validated | Ground-truth identity evaluation, false merge/false split analysis |
| Vector retrieval | MongoDB event documents and embeddings | Ranking quality not yet compared against alternatives | Vector-only baseline, top-k recall, precision and answer relevance |
| Graph persistence | Neo4j Event/Person/Object/Camera/Date/Location relationships | Graph facts depend on extracted metadata; reasoning quality not deeply audited | Graph-fact precision/recall and graph-only QA test set |
| Hybrid retrieval | Route selection among direct/vector/graph/hybrid | No ablation yet proving hybrid is better than vector-only or graph-only | Ablation test with identical query set |
| Answer synthesis | Grounded answer with retrieved sources | Faithfulness/hallucination scoring is still manual/lightweight | RAGAS-style faithfulness and answer relevance scoring |
| Security/privacy | Some upload validation and cloud services used | Personal GCS, MongoDB/Neo4j and VM are not production governance | IAM, encryption, retention, audit logging, consent/access policy |

### Lưu ý sửa nội dung

- Không thêm mô tả dài cho từng subsystem; bảng là đủ.
- Nếu báo cáo có câu “satisfies four evaluated dimensions”, sửa thành “satisfies the selected prototype evaluation dimensions”.
- Khi nói “hybrid retrieval best coverage”, thêm điều kiện “expected by design, pending ablation validation”.

### Citation nên dùng

- VSS stack: `[8]`-`[14]`, `[49]`, `[50]`
- Evaluation: `[20]`-`[22]`, `[51]`, `[53]`, `[54]`
- Security/upload: `[47]`, `[52]`

---

## 7.5 Chapter 4 → thêm Requirement vs. Actual Results cho VSS

### Vị trí trong báo cáo

- **Chapter 4 → 4.4 VSS evaluation and retrieval effectiveness discussion**
- Nên đặt ngay đầu 4.4, trước **4.4.1 Metadata Extraction**

### Cách chỉnh

Thêm bảng:

**Table 4.x. VSS requirement versus actual prototype results**

| VSS requirement | Actual prototype result | Status | Evidence / limitation |
|---|---|---|---|
| Extract event metadata from surveillance clips | Extracts time/camera/location, people, objects, relations and summaries on 50 clips | Implemented | Current evaluation uses limited dataset; needs broader scene diversity |
| Maintain identity-aware memory | Supports `face_id`, identity embeddings and `missing_faceid` | Partial | Needs identity ground truth, false merge and false split analysis |
| Support semantic search | MongoDB vector retrieval supports event-summary search | Implemented / partial | Needs retrieval relevance metrics such as top-k recall/precision |
| Support graph facts | Neo4j stores event/entity/relation facts | Partial | Needs graph-fact ground truth and precision/recall |
| Support hybrid question answering | Routes direct/vector/graph/hybrid queries and synthesizes answers | Partial | Needs ablation and faithfulness evaluation |
| Avoid unsupported identity claims | Uses unresolved or `missing_faceid` for weak face evidence | Implemented by design | Needs audit over occlusion, rear-view and low-confidence cases |
| Protect sensitive surveillance data | Uses cloud services and app-level upload validation | Partial / future work | Personal GCS/MongoDB/Neo4j/VM require production security controls |

### Citation nên dùng

- Evaluation method: `[20]`-`[22]`, `[51]`, `[53]`, `[54]`
- Identity/retrieval/database: `[8]`, `[9]`, `[14]`, `[50]`
- Privacy/sensitive use: `[52]`
- Upload validation/security boundary: `[47]`

---

## 7.6 Chapter 4 → thêm ground-truth evaluation cho VSS

### Vị trí trong báo cáo

- **Chapter 4 → 4.4.1 Metadata Extraction: Coverage and Accuracy**
- Có thể thay Table 4.8 hiện tại bằng bảng chi tiết hơn, hoặc thêm ngay sau Table 4.8.

### Vấn đề hiện tại

Table metadata extraction hiện có claim 100% required fields và 100% correctness, nhưng chưa cho biết ground truth được tạo thế nào, có bao nhiêu nhãn, mỗi loại nhãn đo ra sao, có false positive/false negative không.

### Cách chỉnh

Thêm đoạn ngắn trước bảng:

   Ground truth for VSS should be constructed as a manual annotation sheet for each evaluated clip. Each row should include expected metadata fields, expected visible people, identity label if known, objects, relationships, camera/date/location and a short expected event summary. The current report should treat existing results as prototype evidence unless this ground-truth sheet is fully available.

Thêm bảng:

**Table 4.x. VSS ground-truth evaluation targets**

| Evaluation target | Ground-truth item | Metric to report | Current interpretation |
|---|---|---|---|
| Metadata fields | Time, camera, date, location, event type | Field presence and field correctness | Current result is promising but should show per-field scores |
| People/object extraction | Visible persons and objects per clip | Precision, recall, F1 | Needed to support extraction accuracy claim |
| Identity continuity | Known person labels or manually linked observations | False merge rate, false split rate, unknown handling accuracy | Not sufficiently validated yet |
| Graph facts | Expected Event-Person-Object-Location relationships | Graph triple precision/recall | Needed before strong graph reasoning claim |
| Answer faithfulness | Expected answer and allowed evidence sources | Faithfulness, answer relevance, unsupported-claim rate | Current hallucination claim should be softened until scored |

### Citation nên dùng

- RAGAS faithfulness/relevance: `[20]`, `[21]`, `[22]`
- OpenAI evals: `[51]`
- Google evaluation metrics/templates: `[53]`, `[54]`

---

## 7.7 Chapter 4 → thêm ablation test vector-only, graph-only, hybrid

### Vị trí trong báo cáo

- **Chapter 4 → 4.4.2 Semantic Answer Quality**
- Đặt sau query examples và trước Semantic Answer Quality Evaluation.

### Cách chỉnh

Thêm subsection ngắn:

**4.4.x Retrieval ablation: vector-only, graph-only and hybrid**

Nội dung cần có:

- Dùng cùng một query set cho cả 3 route.
- Tách query thành semantic summary, exact fact, relationship, identity-tracking và mixed investigation.
- Chấm bằng relevance, faithfulness, unsupported claim rate, source coverage và latency.
- Kết luận phải thận trọng: nếu chưa chạy thật, ghi đây là required future evaluation, không ghi là completed result.

Thêm bảng:

**Table 4.x. VSS retrieval ablation design**

| Query category | Vector-only expected strength | Graph-only expected strength | Hybrid expected strength | Metric |
|---|---|---|---|---|
| Broad event summary | Strong semantic recall | Weak if facts are sparse | Strong if graph adds entities | Relevance, source coverage |
| Exact count/fact | May hallucinate or approximate | Strong if graph facts are correct | Strong if graph dominates exact facts | Fact accuracy, unsupported claims |
| Relationship query | Limited unless summary contains relation | Strong for typed edges | Strong if vector finds event and graph expands | Graph-fact precision/recall |
| Identity tracking | Useful for candidate recall | Useful for verified `face_id` traversal | Best expected route, pending validation | False merge/split, faithfulness |
| Mixed investigation | Good narrative context | Good exact constraints | Best expected coverage | Answer relevance, faithfulness, latency |

### Citation nên dùng

- RAG/evaluation: `[20]`-`[22]`
- MongoDB vector: `[9]`, `[50]`
- Neo4j graph: `[14]`
- Evaluation guidance: `[51]`, `[53]`, `[54]`

---

## 7.8 Chapter 4 → tách Completed Results, Limitations, Future Work cho VSS

### Vị trí trong báo cáo

- Cuối **Chapter 4 → 4.4.4 Token Cost**
- Đoạn hiện tại bắt đầu bằng “Overall, the revised VSS pipeline satisfies...”

### Cách chỉnh

Thay đoạn tổng kết dài hiện tại bằng ba subsection ngắn:

**Completed Results**

- 50 clips were processed in the prototype evaluation.
- VSS produced structured event metadata, identity-aware records, MongoDB vector documents and Neo4j graph facts.
- Direct/vector/graph/hybrid query routes executed over the selected query set.
- Latency and token scenario bounds were reported.

**Limitations**

- Ground truth is not deep enough for strong accuracy claims.
- Identity evaluation lacks false merge/false split analysis.
- Graph facts and graph reasoning have not been validated with a labeled graph dataset.
- Hybrid retrieval has not yet been proven better than vector-only or graph-only by ablation.
- Crowded, occluded, low-light and multi-camera cases remain under-tested.
- Current deployment uses personal GCS, MongoDB/Neo4j and VM resources, so security/privacy and production scalability are limited.

**Future Work**

- Build a labeled VSS ground-truth dataset.
- Add RAGAS-style faithfulness/relevance and unsupported-claim audit.
- Run vector-only vs graph-only vs hybrid retrieval ablation.
- Add identity threshold calibration and false merge/false split reporting.
- Add security/privacy controls: IAM, encryption, retention, audit logging, access policy, key rotation and deployment separation.

### Citation nên dùng

- Evaluation/faithfulness: `[20]`-`[22]`, `[51]`, `[53]`, `[54]`
- Privacy/sensitive AI usage: `[52]`
- Cloud/security baseline: `[7]`, `[30]`, `[43]`, `[44]`, `[47]`

---

## 7.9 Chapter 4 hoặc Chapter 1 → thêm security/privacy cho VSS

### Vị trí trong báo cáo

Ưu tiên đặt ở:

- **Chapter 4 → 4.4 VSS evaluation and retrieval effectiveness discussion**, sau đoạn mở đầu 4.4

Hoặc nếu muốn đặt nền sớm hơn:

- **Chapter 1 → 1.2 Current system overview**, sau Table 1.1

### Nội dung cần thêm

Thêm subsection ngắn:

**Security and privacy scope for the VSS prototype**

Nội dung phải nói rõ:

- Hệ thống xử lý video giám sát, khuôn mặt, identity embedding, metadata và relationship graph; đây là dữ liệu nhạy cảm.
- Current deployment dùng GCS cá nhân, MongoDB/Neo4j cá nhân và VM cá nhân, nên chỉ phù hợp prototype/internal evaluation.
- Không nên claim production security/compliance.
- Cần giới hạn truy cập dữ liệu, không public bucket/database, không hard-code secrets trong code/report, và không đưa khuôn mặt/identity ra demo công khai khi chưa có quyền.
- Future work: IAM theo least privilege, encryption at rest/in transit, secret manager, audit logging, retention policy, deletion workflow, role-based access control, data minimization, consent/authorization, backup/restore và incident response.

Thêm bảng:

**Table 4.x. VSS security and privacy risk treatment**

| Sensitive asset | Main risk | Current prototype treatment | Required production hardening |
|---|---|---|---|
| Surveillance video clips | Unauthorized viewing or leakage | Stored in personal GCS / VM workflow | Private buckets, signed access, retention/deletion policy |
| Face images and embeddings | Biometric misuse or identity leakage | Used for `face_id` prototype | Encryption, access control, consent/authorization, deletion workflow |
| Metadata and summaries | Sensitive behavior inference | Stored in MongoDB documents | RBAC, audit logs, data minimization |
| Relationship graph | Reveals people-object-location relationships | Stored in Neo4j prototype graph | Tenant isolation, query authorization, graph export control |
| API keys and DB credentials | Account compromise | Personal environment risk | Secret manager, key rotation, no secrets in repo/report |
| VM and service deployment | Unauthorized access or weak monitoring | Personal VM prototype | Firewall, patching, logs, monitoring, backup/restore |

### Citation nên dùng

- OpenAI usage/privacy-sensitive policy: `[52]`
- OWASP file upload: `[47]`
- GCS docs/best practices: `[7]`, `[30]`, `[43]`, `[44]`
- NIST contingency/backup-resilience reference if discussing backup/recovery: `[17]`

---

## 7.10 Biên tập caption, đánh số bảng/hình, citation và lỗi tiếng Anh

### Vị trí trong báo cáo

- **List of figures**
- **List of tables**
- Tất cả caption trong Chapter 1, 2, 3, 4
- Đặc biệt các dòng quanh Figure 1.1, Figure 2.1-2.3, Figure 3.1-3.4, Table 4.5-4.22

### Lỗi cần sửa cụ thể

1. **Table numbering ở Chapter 4 bị lệch**

   List of tables ghi:

   - Table 4.5 Video Evaluation Dataset
   - Table 4.6 Metadata Extraction Evaluation
   - ...

   Nhưng nội dung hiện tại lại dùng:

   - Table 4.6 Video Evaluation Dataset
   - Table 4.8 Metadata Extraction Evaluation
   - ...

   Cần đánh số lại liên tục. Nếu giữ các bảng mới đề xuất ở trên, nên cập nhật toàn bộ Table 4.x theo thứ tự xuất hiện.

2. **Table 4.4 bị trùng trong List of tables**

   Cần đổi:

   - Table 4.4 Retained VMS playback test cases
   - Table 4.5 VMS playback result interpretation

3. **Figure 3.2 bị caption lặp**

   Hiện có hai Figure 3.2. Nên đổi thành:

   - Figure 3.2. VSS query workflow.
   - Figure 3.3. Query routing and retrieval logic.

   Sau đó các figure schema phía sau phải tăng số tương ứng, hoặc gộp hai hình thành một figure có caption rõ.

4. **Caption tiếng Anh cần sửa**

   - `First Diagram show...` → `The first diagram shows...`
   - `The second Diagram show...` → `The second diagram shows...`
   - `Graph Database in Neo4j Aura (Upper Part) vector database...` → `Neo4j Aura graph database (upper part) and MongoDB Atlas vector database (lower part).`
   - `Upload clip from edge to VPS` nếu thực tế là VMS thì sửa thành `Upload clip from Edge to VMS.`
   - `The The token-cost section...` → `The token-cost section...`

5. **Citation format**

   - Sửa các citation bị thiếu ngoặc như `9-14` thành `[9]-[14]`.
   - Sửa các đoạn `1, 2` thành `[1], [2]`.
   - Sửa khoảng trắng lỗi như `\[ 34\]` thành `[34]`.
   - Với câu nói về policy/privacy, chỉ dùng ref có trong `references.md`, ưu tiên `[52]`; tránh nhắc “Anthropic policy” nếu không có trong references.

---

# 8. Thứ tự ưu tiên chỉnh mới

1. **Chapter 1 cloud-native/scalable definition + table status**  
   Đây là yêu cầu quan trọng nhất để tránh overclaim title.

2. **Chapter 4 VSS Requirement vs Actual Results + implemented/partial/future work table**  
   Giúp hội đồng thấy rõ VSS đã làm gì, chưa làm gì.

3. **Chapter 4 ground-truth evaluation + ablation design**  
   Là phần trực tiếp xử lý điểm yếu VSS/graph reasoning chưa kiểm chứng sâu.

4. **Security/privacy subsection**  
   Cần có vì hệ thống xử lý video giám sát, khuôn mặt, embedding và relationship graph trên hạ tầng cá nhân.

5. **Biên tập caption, đánh số bảng/hình, citation, lỗi tiếng Anh**  
   Làm sau khi chốt bảng mới để không phải đánh số lại nhiều lần.

6. **Rút gọn đoạn dài ở Chapter 4 và Conclusion**  
   Tách Completed Results, Limitations, Future Work; xóa bớt câu lặp về prototype/maturity nếu đã có bảng.

---

# 9. Nhật ký chỉnh sửa đã thực hiện trên báo cáo chuẩn

File đã chỉnh: `M3CP2026_Group4_LeCongThanhKhoa_TranChauHuy_PhungDoAnhKhoa_CMCTelecom.docx (1).docx.md`

## 9.1 Introduction

- **Vị trí:** `INTRODUCTION → 1. Motivation → Table 0.1`
- **Đã sửa:** Thêm cột `Project response / current prototype evidence` để chỉ rõ project đáp ứng từng requirement group như thế nào.
- **Mục đích:** Làm nổi bật giá trị tích hợp Edge → VMS → VSS nhưng không claim ngang production/commercial VMS.
- **Ref liên quan:** `[1]`, `[2]`, `[3]`-`[14]`, `[20]`-`[22]`, `[30]`, `[31]`.

- **Vị trí:** Ngay sau Table 0.1
- **Đã sửa:** Rút gọn đoạn benchmark conclusion, nhấn mạnh đóng góp là integrated evidence pipeline: camera stream → event clip → cloud-backed playback → searchable VSS knowledge.
- **Mục đích:** Tránh viết dài, tránh claim feature parity với commercial systems.

## 9.2 Chapter 1

- **Vị trí:** `Chapter 1 → 1.1 Research questions and project direction`
- **Đã thêm:** Subsection `1.1.1 Definition of cloud-native and scalable scope`.
- **Nội dung:** Định nghĩa rõ `cloud-native`, `scalable`, `cloud-hosted`, `prototype`; nêu rằng hiện tại chỉ là prototype-readiness, chưa phải autoscaling/multi-tenant/SLO production.
- **Ref liên quan:** `[6]`, `[7]`, `[30]`, `[43]`-`[46]`.

- **Vị trí:** Sau subsection mới trong Chapter 1
- **Đã thêm:** `Table 1.1. Cloud-native and scalability status by component`.
- **Nội dung:** Phân loại Edge Agent, VMS playback API, GCS, Pub/Sub, MongoDB, Neo4j, VSS reasoning agent, security/privacy controls theo current status.
- **Ref liên quan:** `[7]`, `[9]`, `[14]`, `[30]`, `[40]`, `[43]`-`[47]`, `[50]`-`[54]`.

- **Vị trí:** `Chapter 1 → 1.2 Current system overview`
- **Đã sửa:** Đổi bảng maturity cũ thành `Table 1.2` để không trùng với bảng cloud-native/scalable mới.

## 9.3 Chapter 2 - VSS theory

- **Vị trí:** `Chapter 2 → 2.2.2 Baseline architecture and adaptation rationale`
- **Đã sửa:** Chuẩn hóa citation bị thiếu ngoặc từ `9-14` thành `[9]-[14]`.

- **Vị trí:** `Chapter 2 → 2.2.3 Hybrid retrieval and identity-aware surveillance knowledge`
- **Đã thêm:** Dòng `Evaluation memory` vào `Table 2.1. VSS surveillance memory layers`.
- **Mục đích:** Đưa ground-truth/test set thành một phần bắt buộc của VSS evaluation, không chỉ có event/identity/relational memory.
- **Ref liên quan:** `[20]`-`[22]`, `[51]`, `[53]`, `[54]`.

- **Vị trí:** Cuối `2.2.3`, trước safety policy
- **Đã thêm:** Đoạn giới hạn claim: graph retrieval hiện chỉ nên hiểu là structured fact lookup và relationship traversal ở mức prototype, chưa phải fully validated investigative reasoning.
- **Mục đích:** Xử lý nhận xét “VSS và graph reasoning chưa được kiểm chứng đủ sâu”.
- **Ref liên quan:** `[20]`-`[22]`, `[51]`, `[53]`, `[54]`.

- **Vị trí:** Figure 2.1-2.3 captions
- **Đã sửa:** Chuẩn hóa caption tiếng Anh cho VSS baseline, LangSmith workflow, Neo4j/MongoDB data stores.

## 9.4 Chapter 3 - VSS implementation

- **Vị trí:** `Chapter 3 → 3.2.3 Persistence and hybrid database design`, sau `Table 3.3`
- **Đã thêm:** `Table 3.4. VSS subsystem implementation status`.
- **Nội dung:** Implemented / Partial / Future work cho video ingest, metadata extraction, identity handling, vector retrieval, graph persistence, hybrid retrieval, answer synthesis, security/privacy.
- **Mục đích:** Tách rõ phần đã làm, còn partial, và future work cho riêng VSS.
- **Ref liên quan:** `[8]`-`[14]`, `[20]`-`[22]`, `[47]`, `[49]`-`[54]`.

- **Vị trí:** Chapter 3 table numbering
- **Đã sửa:** VMS playback architecture layers đổi thành `Table 3.5`; VMS endpoint summary đổi thành `Table 3.6`.

- **Vị trí:** Chapter 3 figure captions
- **Đã sửa:** Tách Figure 3.2 và Figure 3.3; cập nhật schema figures và VMS workflow figures đến Figure 3.14 để tránh trùng số.

## 9.5 Chapter 4 - VSS evaluation

- **Vị trí:** `Chapter 4 → 4.4 VSS evaluation and retrieval effectiveness discussion`, đầu section
- **Đã thêm:** Đoạn framing rằng VSS results là prototype evidence, chưa phải production-grade validation.
- **Đã sửa:** Bỏ nhắc `Anthropic policy` vì không có trong `references.md`; chỉ giữ OpenAI policy `[52]`.
- **Ref liên quan:** `[7]`, `[30]`, `[43]`-`[47]`, `[52]`.

- **Vị trí:** Đầu `4.4`
- **Đã thêm:** `Table 4.6. VSS requirement versus actual prototype results`.
- **Mục đích:** Bổ sung Requirement vs Actual Results riêng cho VSS.
- **Ref liên quan:** `[8]`, `[9]`, `[14]`, `[20]`-`[22]`, `[47]`, `[50]`-`[54]`.

- **Vị trí:** Đầu `4.4`
- **Đã thêm:** `Table 4.7. VSS security and privacy risk treatment`.
- **Nội dung:** Video clips, face embeddings, metadata, relationship graph, API keys/credentials, VM/service deployment.
- **Mục đích:** Bổ sung security/privacy vì hệ thống xử lý video giám sát, khuôn mặt, identity embedding và relationship graph trên hạ tầng cá nhân.
- **Ref liên quan:** `[7]`, `[8]`, `[9]`, `[14]`, `[17]`, `[30]`, `[43]`, `[44]`, `[47]`, `[52]`.

- **Vị trí:** `4.4.1 Metadata Extraction`
- **Đã thêm:** Ground-truth protocol paragraph và `Table 4.10. VSS ground-truth evaluation targets`.
- **Nội dung:** Metadata fields, people/object extraction, identity continuity, graph facts, answer faithfulness.
- **Mục đích:** Làm rõ current results chưa đủ để claim accuracy/hallucination sâu nếu chưa có per-field/precision/recall/F1/faithfulness.
- **Ref liên quan:** `[20]`-`[22]`, `[51]`, `[53]`, `[54]`.

- **Vị trí:** `4.4.2 Semantic Answer Quality`
- **Đã thêm:** Đoạn retrieval ablation và `Table 4.14. VSS retrieval ablation design`.
- **Nội dung:** Vector-only vs graph-only vs hybrid theo broad summary, exact fact, relationship query, identity tracking, mixed investigation.
- **Mục đích:** Xử lý yêu cầu ablation test giữa vector-only, graph-only và hybrid retrieval.
- **Ref liên quan:** `[20]`-`[22]`, `[51]`, `[53]`, `[54]`.

- **Vị trí:** Cuối `4.4.4 Token Cost`
- **Đã sửa:** Thay đoạn tổng kết dài bằng 3 subsection `Completed Results`, `Limitations`, `Future Work`.
- **Mục đích:** Tách rõ kết quả đã hoàn thành, hạn chế, và hướng phát triển; hạ claim từ production/scalable sang prototype evidence.
- **Ref liên quan:** `[8]`, `[9]`-`[14]`, `[17]`, `[20]`-`[22]`, `[47]`, `[48]`, `[51]`-`[54]`.

- **Vị trí:** Chapter 4 table numbering và List of tables
- **Đã sửa:** Chuẩn hóa số bảng Chapter 4 từ `Table 4.1` đến `Table 4.25`, xóa dòng duplicate `Table 4.4`.

## 9.6 Conclusion

- **Vị trí:** `CONCLUSION`
- **Đã thêm:** Câu giới hạn VSS: cần ground-truth evaluation, retrieval ablation, identity false-merge/false-split analysis, answer-faithfulness scoring, security/privacy hardening cho surveillance video, face embeddings và relationship graphs.
- **Mục đích:** Kết luận không overclaim VSS/graph reasoning.
- **Ref liên quan:** `[20]`-`[22]`, `[47]`, `[51]`-`[54]`.

## 9.7 Biên tập kỹ thuật

- **Đã sửa:** `The The token-cost section` → `The token-cost section`.
- **Đã sửa:** Một số caption tiếng Anh: `First Diagram show...`, `Graph Database in Neo4j...`, `Upload clip from edge to VPS`.
- **Đã sửa:** Một số citation formatting lỗi như `,[ 34]`, `[33-35]`, `[23]\-[25]`.
- **Còn lưu ý:** Trong Chapter 2 vẫn còn một vài cụm mô tả cũ dùng từ `graph reasoning` trong cùng paragraph dài; đã bổ sung caveat ngay sau đó để giới hạn claim thành prototype graph lookup/traversal. Nếu muốn polish thêm, có thể rewrite toàn bộ paragraph `2.2.3` ở lượt sau để thay triệt để wording cũ.

---

# 10. Nhật ký tinh gọn bổ sung

## 10.1 Kiểm tra số thứ tự bảng

- **Vị trí:** `List of tables` và toàn bộ heading bảng trong báo cáo.
- **Đã kiểm tra:** `List of tables` đã khớp với các bảng trong thân bài từ `Table 0.1` đến `Table 4.25`.
- **Kết quả:** Không còn dòng duplicate `Table 4.4`; Chapter 3 đã có `Table 3.4` cho VSS status nên VMS tables được giữ ở `Table 3.5` và `Table 3.6`.

## 10.2 Tinh gọn Chapter 1

- **Vị trí:** `Chapter 1` intro paragraph.
- **Đã sửa:** Rút gọn đoạn giới thiệu chapter, giữ lại mục tiêu định nghĩa context, research questions và maturity framing.

- **Vị trí:** `1.1 Research questions and project direction`.
- **Đã sửa:** Rút gọn research questions từ đoạn dài thành ba câu hỏi trực tiếp.

- **Vị trí:** `1.1.1 Definition of cloud-native and scalable scope`.
- **Đã sửa:** Nén định nghĩa `cloud-native` và `scalable`, giữ ý phân biệt prototype-readiness với production autoscaling/SLO.

- **Vị trí:** `1.2 Current system overview`.
- **Đã sửa:** Rút gọn overview Edge → VMS → VSS; bỏ đoạn giải thích lặp sau Table 1.2.

- **Vị trí:** `1.4 Related literature and architectural positioning`.
- **Đã sửa:** Thay hai đoạn dài bằng một đoạn ngắn, giữ ba domain chính và positioning là system integration/workflow grounding.

## 10.3 Tinh gọn Chapter 2 phần VSS

- **Vị trí:** `2.2.1 Research questions for VSS`.
- **Đã sửa:** Rút gọn thành ba câu hỏi VSS: structured knowledge, vector/graph storage, direct/vector/graph/hybrid routing.

- **Vị trí:** `2.2.2 Baseline architecture and adaptation rationale`.
- **Đã sửa:** Rút gọn baseline/adaptation, giữ ý ingest/retrieval split và persistent identity.

- **Vị trí:** `2.2.3 Hybrid retrieval and identity-aware surveillance knowledge`.
- **Đã sửa:** Rút gọn các đoạn dài về hybrid levels, retrieval controls, answer grounding và safety policy; giữ Table 2.1, graph caveat và `missing_faceid`.
- **Đã sửa thêm:** Xóa các heading placeholder `###` trống trong khu vực VSS theory.

## 10.4 Tinh gọn Chapter 3 phần VSS

- **Vị trí:** `3.2.1 Ingest flow and identity lifecycle`.
- **Đã sửa:** Rút gọn mô tả ingest pipeline và identity lifecycle, bỏ phần liệt kê từng node quá chi tiết.

- **Vị trí:** `3.2.2 Query routing and four response modes`.
- **Đã sửa:** Rút gọn query routing, giữ Figure 3.2/3.3 và Table 3.2.

- **Vị trí:** `3.2.3 Persistence and hybrid database design`.
- **Đã sửa:** Rút gọn persistence design, giữ các quy tắc identifier continuity, conservative persistence, Table 3.3 và Table 3.4.

## 10.5 Tinh gọn Chapter 4 phần VSS

- **Vị trí:** `4.4 VSS evaluation and retrieval effectiveness discussion`.
- **Đã sửa:** Rút gọn opening evaluation framing, giữ bốn evaluation dimensions và prototype/security caveat.

- **Vị trí:** `4.4.1 Metadata Extraction`.
- **Đã sửa:** Rút gọn dataset description, ingest interpretation, ground-truth note và limitation paragraph.

- **Vị trí:** `4.4.2 Semantic Answer Quality`.
- **Đã sửa:** Rút gọn query evaluation description, ablation note, answer-quality interpretation.

- **Vị trí:** `4.4.3 VSS Agent Latency`.
- **Đã sửa:** Rút gọn latency interpretation, giữ Table 4.16-4.19.

- **Vị trí:** `4.4.4 Token Cost`.
- **Đã sửa:** Rút gọn token-cost explanation, xóa dòng `.` thừa, giữ Table 4.20-4.25.

## 10.6 Biên tập nhỏ sau tinh gọn

- **Đã sửa:** `Edge â†’ centralized VMS â†’ VSS analysis` → `Edge -> centralized VMS -> VSS analysis`.
- **Đã sửa:** Query examples trong Table 3.2 để bỏ quote mojibake.
- **Đã sửa:** Một số cụm `graph reasoning` thành `graph retrieval`, `graph traversal`, hoặc `graph fact claims` ở các bảng/đoạn liên quan.
## 10.7 Kiem tra va chinh sua bo sung theo yeu cau moi

- **Vi tri:** `List of tables`.
- **Da sua:** Chuan hoa dinh dang cac dong table tu dau hai cham sang dau cham, de khop voi caption trong than bai: `Table X.Y. ...`.
- **Ket qua:** Danh muc bang hien khop voi cac caption tu `Table 0.1` den `Table 4.25`; khong thay duplicate table number.

- **Vi tri:** Chapter 1 va khu vuc truoc Chapter 2.
- **Da sua:** Xoa heading rong `#` truoc Chapter 2; giu Chapter 1 o dang ngan gon gom project direction, dinh nghia cloud-native/scalable, maturity table, related literature positioning.

- **Vi tri:** Chapter 2, 3, 4 trong cac phan VSS.
- **Da sua:** Xoa cac heading placeholder `###` rong con sot sau khi tinh gon; giu lai cac bang/caption va cac caveat quan trong ve ground truth, ablation, graph facts, answer faithfulness, security/privacy.

- **Vi tri:** Toan bo bao cao.
- **Da sua:** Chuan hoa cac ky tu mojibake con sot nhu quote, dash, `<=`, `>=`; xoa dong dau cham thua truoc `CONCLUSION`.
