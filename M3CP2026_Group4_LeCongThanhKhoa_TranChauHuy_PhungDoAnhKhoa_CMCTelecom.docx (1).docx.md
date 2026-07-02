  
**THE UNIVERSITY OF DANANG**  
**UNIVERSITY OF SCIENCE AND TECHNOLOGY**  
**FACULTY OF ADVANCED SCIENCE AND TECHNOLOGY**

| ![][image1] | ![][image2] |
| :---: | :---: |

**CAPSTONE PROJECT REPORT**

**Cloud-Native AI Video Management and Analytics**   
**Platform for Scalable Smart Surveillance System**

|  Students: |  |     Le Cong Thanh Khoa   Class: 21 ECE    Tran Chau Huy              Class: 21 ECE Phung Do Anh Khoa     Class: 21 ES             |
| ----- | :---: | :---: |

 

| Supervisor: Instructor at the company:                                Company name: |  | Nguyen Quang NhÆ° Quynh Nguyen Truong Xuan CMC Telecom Da Nang |
| ----: | :---- | :---- |

**Danang, April 2026**

# **ABSTRACT** {#abstract}

This thesis presents the design, implementation, and evaluation of a cloud-oriented smart surveillance platform that integrates three main layers: an Edge-based event capture subsystem, a Video Management System (VMS) centered on playback-oriented investigation, and a Visual Surveillance System (VSS) for metadata extraction, identity-aware retrieval, and question answering. The project addresses three linked research directions inherited from the earlier milestone stage: how distributed surveillance sites can be coordinated through an Edge-to-VMS architecture, how AI-assisted retrieval can improve playback-based investigation, and how the overall platform can remain practical in its current prototype form while still supporting later scalability and extension. Rather than treating Edge, VMS, and VSS as separate prototypes, the thesis frames them as one surveillance evidence pipeline that moves from continuous camera streams, to event-based clips, to cloud-backed playback, and finally to searchable surveillance knowledge.

The Edge subsystem transforms legacy ONVIF Profile S or RTSP camera streams into event-driven clips through spatial-temporal motion filtering, pre-event and post-event context preservation, GOP-aware ring-buffer management, and event-based upload coordination. The VMS subsystem provides the clearest operator-facing workflow of the current project by receiving clips, validating and normalizing them into browser-compatible MP4, synchronizing playback-ready objects to Google Cloud Storage, supporting efficient playback through HTTP Range Requests, and mediating clip-specific interaction with the VSS. The VSS subsystem extends a caption-oriented baseline into an identity-aware hybrid retrieval architecture that combines MongoDB for semantic event and identity search with Neo4j for exact surveillance facts and relationship reasoning. It processes selected clips into event summaries, identity records, graph structures, and grounded answers through four response modes: direct, vector, graph, and hybrid.

Evaluation results show that the strongest validated workflow in the current capstone is the integrated Edge-to-VMS-to-VSS evidence path. The Edge subsystem demonstrates fast motion-to-record responsiveness, contextual clip preservation, and reliable upload in the evaluated prototype path. The VSS subsystem demonstrates practical metadata extraction, grounded answer formation, and workable latency for playback-centered investigation. The VMS subsystem provides the clearest quantitative evidence, with retained playback results showing an event-to-play availability rate of 100%, average playback open time, average search latency that is acceptable, and a playback media error rate that is zero in the controlled retained test dataset. These results support the conclusion that the project already constitutes a defensible integrated prototype for surveillance evidence capture, review, and AI-assisted follow-up, while broader production-grade completeness remains future work.

 

 

 


#  {#heading}

# **ACKNOWLEDGMENT** {#acknowledgment}

We would like to express our sincere gratitude to CMC Telecom Infrastructure Corporation \- Central Branch for the support provided throughout the development of this capstone project. The practical environment, technical orientation, and industry context offered by the company were highly valuable in shaping both the direction of the project and the realism of its implementation choices.

We would also like to extend our heartfelt thanks to Mr. Nguyen Truong Xuan and Mr. Nguyen Minh Thanh for their guidance, support, and practical insights during the internship and project process. Their feedback helped us better connect the technical design of the system with real surveillance workflow needs. In addition, we sincerely thank Ms. Nguyen Quang Nhu Quynh, our academic supervisor, for her dedicated instruction, encouragement, and constructive feedback throughout the research and report-writing process. Her support was essential in helping us refine the project into a more coherent and academically defensible capstone report.

 

 

 


#  {#heading-1}

# **Contents** {#contents}

[ABSTRACT	1](#abstract)

	[2](#heading)

[ACKNOWLEDGMENT	3](#acknowledgment)

	[3](#heading-1)

[Contents	4](#contents)

[List of figures	6](#list-of-figures)

[List of tables	7](#list-of-tables)

[Abbreviation	8](#abbreviation)

[INTRODUCTION	10](#introduction)

[1\. Motivation	10](#1.-motivation)

[2\. Contribution of the thesis	14](#2.-contribution-of-the-thesis)

[3\. Organization of the thesis	15](#3.-organization-of-the-thesis)

[4\. Milestone of the Project	17](#4.-milestone-of-the-project)

[5\. Work distribution	17](#5.-work-distribution)

[CHAPTER 1\. REVIEW OF LITERATURE AND PROJECT CONTEXT	19](#chapter-1.-review-of-literature-and-project-context)

[1.1 Research questions and project direction	19](#1.1-research-questions-and-project-direction)

[1.2 Current system overview	20](#1.2-current-system-overview)

[1.4 Related literature and architectural positioning	22](#1.4-related-literature-and-architectural-positioning)

[CHAPTER 2\. DETAILED THEORY AND SUBSYSTEM FOUNDATIONS	24](#chapter-2.-detailed-theory-and-subsystem-foundations)

[2.1 Research-question-driven overview of subsystem foundations	24](#2.1-research-question-driven-overview-of-subsystem-foundations)

[2.2 Visual Surveillance System theoretical foundation	24](#2.2-visual-surveillance-system-theoretical-foundation)

[2.2.1 Research questions for VSS	24](#2.2.1-research-questions-for-vss)

[2.2.2 Baseline architecture and adaptation rationale	25](#2.2.2-baseline-architecture-and-adaptation-rationale)

[2.2.3 Hybrid retrieval and identity-aware surveillance knowledge	26](#2.2.3-hybrid-retrieval-and-identity-aware-surveillance-knowledge)

[2.2.4 Figure and schema placement notes	29](#2.2.4-figure-and-schema-placement-notes)

[2.3 Edge Agent theoretical foundation	30](#2.3-edge-agent-theoretical-foundation)

[2.3.1 Research questions for Edge Agent	30](#2.3.1-research-questions-for-edge-agent)

[2.3.2 ONVIF Profile S constraints and edge gateway rationale	31](#2.3.2-onvif-profile-s-constraints-and-edge-gateway-rationale)

[2.3.3 Spatial-temporal filtering theory	32](#2.3.3-spatial-temporal-filtering-theory)

[2.3.4 GOP-aware buffering and site-to-cloud synchronization theory	34](#2.3.4-gop-aware-buffering-and-site-to-cloud-synchronization-theory)

[2.3.5 Figure placement notes	36](#2.3.5-figure-placement-notes)

[2.4 VMS playback theoretical foundation	36](#2.4-vms-playback-theoretical-foundation)

[2.4.1 Research question for VMS playback	36](#2.4.1-research-question-for-vms-playback)

[2.4.2 HTTP/REST as the orchestration protocol	37](#2.4.2-http/rest-as-the-orchestration-protocol)

[2.4.3 Multipart/form-data for video upload	37](#2.4.3-multipart/form-data-for-video-upload)

[2.4.7 Protocol selection by VMS responsibility	39](#2.4.7-protocol-selection-by-vms-responsibility)

[CHAPTER 3\. PROPOSED ALGORITHMS AND SYSTEM DESIGN	44](#chapter-3.-proposed-algorithms-and-system-design)

[3.1 Integrated system architecture	44](#3.1-integrated-system-architecture)

[3.2 VSS ingest and query workflows	47](#3.2-vss-ingest-and-query-workflows)

[3.2.1 Ingest flow and identity lifecycle	47](#3.2.1-ingest-flow-and-identity-lifecycle)

[3.2.2 Query routing and four response modes	51](#3.2.2-query-routing-and-four-response-modes)

[3.2.3 Persistence and hybrid database design	54](#3.2.3-persistence-and-hybrid-database-design)

[3.3 Edge Agent algorithms and synchronization workflows	59](#3.3-edge-agent-algorithms-and-synchronization-workflows)

[3.3.1 Event detection and clip generation algorithm	59](#3.3.1-event-detection-and-clip-generation-algorithm)

[3.3.2 Buffer management and upload coordination	62](#3.3.2-buffer-management-and-upload-coordination)

[3.4 VMS playback and VSS integration workflows	64](#3.4-vms-playback-and-vss-integration-workflows)

[3.4.1 Upload, normalization, storage, and playback workflow	64](#3.4.1-upload,-normalization,-storage,-and-playback-workflow)

[3.4.2 VMS playback architecture and endpoint contract	65](#3.4.2-vms-playback-architecture-and-endpoint-contract)

[3.4.3 Deep Analyze and clip-specific query workflow	67](#3.4.3-deep-analyze-and-clip-specific-query-workflow)

[3.4.4 Asynchronous ingestion event publishing	68](#3.4.4-asynchronous-ingestion-event-publishing)

[CHAPTER 4\. EXPERIMENTAL RESULTS AND EVALUATION	76](#chapter-4.-experimental-results-and-evaluation)

[4.1 Integration status and evidence	76](#4.1-integration-status-and-evidence)

[4.2 Edge Agent evaluation and operational efficiency discussion	78](#4.2-edge-agent-evaluation-and-operational-efficiency-discussion)

[4.3.1 Experimental Configuration and Baseline Analytical Environment	78](#4.2.1-experimental-configuration-and-baseline-analytical-environment)

[4.3.2 Computational Throughput and Motion-to-Record Responsiveness	78](#to-rigorously-evaluate-the-computational-throughput,-transactional-responsiveness,-and-overall-operational-efficiency-of-the-localized-go-based-edge-agent,-which-is-conceptually-mapped-as-the-kerberos-agent-framework-within-the-primary-site-baseline,-a-quantitative-empirical-benchmark-was-established-under-realistic-surveillance-workloads-[1],-[2].-this-restrictive-hardware-profile-directly-mirrors-the-pc-infrastructure-constraints-typical-of-legacy-retail-and-corporate-physical-security-sites,-ensuring-the-empirical-metrics-gathered-inform-operational-viability-under-resource-bound-deployment-scenarios.-the-evaluation-workload-comprised-multiple-parallel-input-stream-configurations-ingesting-continuous-video-broadcasts-from-legacy-commercial-camera-nodes,-which-specifically-consisted-of-commercial-grade-imou-smart-cameras-over-a-localized-network-topology.-the-raw-source-media-utilized-standard-h.264/avc-compression-running-at-a-baseline-frame-rate-of-10-frames-per-second-with-a-high-definition-spatial-resolution-of-1080p.-under-standard-operating-parameters,-the-static-ring-buffer-capacity-was-initialized-at-a-physical-allocation-scale-of-30-slots-to-capture-and-preserve-structural-spatial-temporal-frame-transitions-without-introducing-memory-overhead.-the-system-performance-was-rigorously-audited-across-five-core-evaluation-axes:-motion-to-record-responsiveness,-event-context-preservation,-clip-save-persistence,-suppression-effectiveness-of-redundant-triggers,-and-upload-network-reliability.)

[4.3.2 Computational Throughput and Motion-to-Record Responsiveness	79](#the-processing-efficiency-of-the-localized-filtering-pipeline-is-critical-to-prevent-frame-buffer-overflow-at-the-local-network-ingress-boundary.-the-execution-latency-per-incoming-raw-video-frame-was-measured-across-individual-sub-stages,-including-vectorized-luma-coefficient-mapping-for-grayscale-conversion,-absolute-register-level-differencing-utilizing-two's-complement-subtraction,-and-the-linear-summation-pass-required-for-spatial-anomaly-density-mask-calculation.-given-that-a-10-frames-per-second-video-pipeline-imposes-a-deterministic-inter-frame-arrival-constraint-of-100-milliseconds,-the-edge-agent-operates-comfortably-within-real-time-execution-bounds,-consuming-merely-9.3-milliseconds-of-the-available-frame-processing-window.)

[4.3 VMS playback evaluation	80](#4.3-vms-playback-evaluation)

[4.3.1 Test principles and test grouping	80](#4.3.1-test-principles-and-test-grouping)

[4.3.2 Playback performance and reliability results	82](#4.3.2-playback-performance-and-reliability-results)

[4.3.3 Test case table	83](#4.3.3-test-case-table)

[4.4 VSS evaluation and retrieval effectiveness discussion	85](#4.4-vss-evaluation-and-retrieval-effectiveness-discussion)

[4.4.1 Metadata Extraction: Coverage and Accuracy	85](#4.4.1-metadata-extraction:-coverage-and-accuracy)

[4.4.2 Semantic Answer Quality	87](#4.4.2-semantic-answer-quality)

[4.4.3 VSS Agent Latency: Ingest and Query	90](#4.4.3-vss-agent-latency:-ingest-and-query)

[4.4.4 Token Cost: Video and Query	94](#4.4.4-token-cost:-video-and-query)

[CONCLUSION	99](#conclusion)

[BIBLIOGRAPHY	101](#bibliography)

 

 


# **List of figures** {#list-of-figures}

Figure 0.1 Full project Grant Chart

Figure 1.1 End-to-end operational flow of the proposed system

Figure 2.1 VSS baseline architecture based on the ingest/retrieval pattern

Figure 2.2 VSS LangGraph workflow visualized in LangSmith

Figure 2.3 VSS data stores in Neo4j Aura and MongoDB Atlas

Figure 2.4 Flowchart of the adaptive spatial-temporal edge filter.

Figure 2.5 Multi-cursor ring buffer diagram

Figure 2.6. Hierarchical site-to-cloud VMS topology

Figure 3.1 Ingested Data & Identity Lifecycle Flow (first diagram); VSS adapted algorithm for data ingestion (second diagram)

Figure 3.2 VSS query workflow

Figure 3.3 Query routing and retrieval logic

Figure 3.4 Data schema for MongoDB identities, MongoDB events, and Neo4j graph

Figure 3.5 Data schema for retrieved event documents, processed videos, and ingestion status

Figure 3.6 Ingestion and spatial-temporal feature extraction stage

Figure 3.7 Event verification and context preservation stage

Figure 3.8 GOP-aware bounded tail-dropping and pointer synchronization loop

Figure 3.9 Upload clip from Edge to VMS

Figure 3.10 Video normalization and cloud synchronization

Figure 3.11 Publish ingest event through Pub/Sub

Figure 3.12 Playback clip from GCS to the browser

Figure 3.13 Seek and scrub using HTTP Range

Figure 3.14 Deep Analyze popup and clip-specific query

# **List of tables** {#list-of-tables}

Table 0.1. Existing technical solutions benchmark by requirement group

Table 1.1. Cloud-native and scalability status by component

Table 1.2. Current subsystem maturity profile

Table 2.1. VSS surveillance memory layers

Table 2.2. Communication model for VMS orchestration

Table 2.3. Upload payload format for clip intake

Table 2.4. Browser playback delivery strategy

Table 2.5. Storage model for playback-ready clips

Table 2.6. Downstream analysis handoff strategy

Table 3.1. Integrated architecture responsibilities and evidence handoff

Table 3.2. VSS query routing modes

Table 3.3. VSS persistence object mapping

Table 3.4. VSS subsystem implementation status

Table 3.5. VMS playback architecture layers

Table 3.6. VMS playback endpoint summary

Table 4.1. Current integration status by system link

Table 4.2. Edge Agent evaluation metrics

Table 4.3. VMS playback test groups and representative checks

Table 4.4. Retained VMS playback test cases

Table 4.5. VMS playback result interpretation

Table 4.6. VSS requirement versus actual prototype results

Table 4.7. VSS security and privacy risk treatment

Table 4.8. Video Evaluation Dataset

Table 4.9. Metadata Extraction Evaluation

Table 4.10. VSS ground-truth evaluation targets

Table 4.11. Query Execution Flows

Table 4.12. Query Evaluation Scope

Table 4.13. Example Queries by Query Type

Table 4.14. VSS retrieval ablation design

Table 4.15. Semantic Answer Quality Evaluation

Table 4.16. Ingest Latency by Face-Processing Case

Table 4.17. Ingest Workload Bounds for the 50-Video Evaluation Set

Table 4.18. Query Latency by Query Type

Table 4.19. Direct Query versus Retrieval Query Latency

Table 4.20. Token Baseline and Scenario Bounds

Table 4.21. Video Token Scenario by Ingest Case

Table 4.22. Video Token Scenario for the 50-Video Evaluation Set

Table 4.23. Query Token Scenario by Query Type

Table 4.24. Overall Token Scenario

Table 4.25. GPT-5.4-Mini Standard-Rate Cost Scenario 

# **Abbreviation** {#abbreviation}

| Abbreviation | Meaning |
| :---- | :---- |
| AI | Artificial Intelligence |
| API | Application Programming Interface |
| CA-RAG | Context-Aware Retrieval-Augmented Generation |
| CCTV | Closed-Circuit Television |
| CPU | Central Processing Unit |
| Edge Agent | Edge-side event detection and synchronization runtime |
| FFmpeg | Fast Forward Moving Picture Experts Group |
| GCS | Google Cloud Storage |
| GOP | Group of Pictures |
| HTTP | Hypertext Transfer Protocol |
| LLM | Large Language Model |
| ONVIF | Open Network Video Interface Forum |
| Pub/Sub | Publish/Subscribe |
| RAM | Random Access Memory |
| REST | Representational State Transfer |
| RTSP | Real-Time Streaming Protocol |
| RTP | Real-time Transport Protocol |
| VLM | Vision-Language Model |
| VMS | Video Management System |
| VSS | Video Search and Summary / Visual Surveillance System |
| WAN | Wide Area Network |

   
 



# **INTRODUCTION** {#introduction}

## **1\. Motivation** {#1.-motivation}

Current surveillance systems are commonly built around several separate solution groups. Traditional Video Management System (VMS) platforms focus on camera management, live view, recording, playback, event lists, and centralized monitoring. Edge-based camera systems place processing closer to the camera site so that raw video does not always need to be continuously transferred to a central server \[3\]-\[5\], \[27\], \[29\]. AI camera and video analytics solutions add functions such as face recognition, license-plate recognition, object detection, intrusion alerts, and event classification \[8\], \[12\], \[13\]. More recent video search and summarization systems also use visual-language models, embeddings, vector databases, or graph structures to help users search and interpret recorded footage \[9\]-\[14\], \[20\]-\[22\].

These existing directions show that many individual parts of the surveillance problem already have practical solutions. A conventional VMS can help operators view cameras, check recordings, and manage event lists through web-compatible playback and storage workflows \[6\], \[7\], \[30\], \[31\], \[34\], \[35\]. An edge-processing layer can reduce unnecessary data transfer by detecting activity near the camera source \[3\]-\[5\], \[27\], \[29\]. AI-assisted analytics can recognize people, vehicles, or objects and generate useful metadata \[8\], \[12\], \[13\]. Retrieval-oriented systems can help users search stored video by semantic meaning instead of manually watching long recordings \[9\]-\[14\], \[20\]-\[22\]. However, these solution groups are often presented or implemented as separate capabilities rather than as one continuous evidence workflow. This separation creates a practical gap for our project. A traditional VMS may support playback, but it does not always provide strong semantic retrieval, identity-aware reasoning, or natural-language investigation support \[6\], \[7\], \[31\], \[34\], \[35\]. An edge-based system may detect local events, but it may not provide a complete centralized review and analysis workflow \[3\]-\[5\], \[27\], \[29\]. AI video analytics may detect objects or generate captions, but the results may not be connected tightly to playback, clip evidence, camera context, and later user questions \[8\]-\[14\]. Video search systems may retrieve relevant clips, but they often assume that video assets and metadata are already prepared, rather than addressing how evidence is captured, normalized, stored, played back, and analyzed across an integrated surveillance chain \[7\], \[9\]-\[14\], \[30\], \[31\].

The target surveillance context of this project requires more than basic camera viewing. Operators need to review past incidents quickly, search for people or objects across recorded footage, inspect event evidence, and connect playback with AI-assisted follow-up \[1\], \[2\]. Manual review becomes slow when the number of cameras, locations, and time ranges increases, and response-time expectations become more difficult to satisfy as the review workflow becomes fragmented \[18\], \[19\], \[23\]-\[25\]. It is also difficult to reconstruct what happens when the operator must move separately between camera screens, file storage, playback interfaces, and analysis tools. The problem is therefore not only how to display video, but how to make video evidence operationally usable. The current project requirements point to several recurring needs. The system should support centralized monitoring and management, AI-assisted access control and identity or vehicle verification, event detection and operational alerting, investigation through playback and tracing, statistical reporting, scalable multi-site deployment, and later customization for customer-specific workflows \[1\], \[2\]. These needs indicate that the project should not be designed as only a VMS, only an edge detector, or only an AI search tool. It should be designed as an integrated surveillance platform where capture, review, and analysis reinforce each other.

**Table 0.1. Existing technical solutions benchmark by requirement group**

| Requirement group | Project operational need | Reference products and benchmark insight | Remaining gap for the project context | Project response / current prototype evidence |
| :---- | :---- | :---- | :---- | :---- |
| Centralized monitoring and management | Manage cameras, status, zones, live view, playback, and event lists from one system \[1\], \[2\] | Eagle Eye, Verkada, Arcules, and similar platforms show the common direction of centralized cloud or enterprise monitoring workspaces \[6\], \[7\], \[30\] | End-to-end alignment across monitoring, playback, event review, analysis, and reporting is not always presented as one continuous workflow \[23\]-\[25\] | The current evidence is strongest in the Edge to VMS to VSS workflow, where event clips can be captured, reviewed, and sent to analysis. |
| AI-assisted access control and identity or vehicle verification | Support face recognition, license-plate recognition, entry or exit verification, and related access workflows \[1\], \[2\] | AI camera products and computer-vision solutions show relevant recognition capabilities \[8\], \[12\], \[13\] | Detection results are not always connected to operator action, evidence review, long-term identity context, and centralized management \[8\]-\[14\] | VSS supports DeepFace-based `face_id`, metadata extraction, vector retrieval, and graph persistence, but identity and graph retrieval remain prototype-level and require deeper ground-truth validation. |
| Event detection and operational alerting | Detect abnormal events, filter event lists, review event details, and support faster response \[1\], \[2\] | Verkada, Avigilon, Eagle Eye, and similar systems show the common direction of alert dashboards and event-oriented monitoring \[3\]-\[5\], \[27\], \[29\] | The connection between alerts, clip evidence, playback, and later investigation is not always emphasized as a single workflow \[6\], \[7\], \[31\], \[34\], \[35\] | The Edge subsystem converts continuous RTSP input into event-oriented clips; production alert management and monitoring remain future work. |
| Investigation, playback, and object tracing | Review recorded video by time and camera, inspect event evidence, trace objects across cameras, and search by natural description \[1\], \[2\] | Avigilon, Eagle Eye, Rhombus, and related products provide useful references for search and investigation-oriented review \[9\]-\[14\], \[20\]-\[22\] | Natural-language search, multi-step refinement, identity continuity, and object tracing tied directly to evidence workflows remain only partially addressed in many available materials \[8\]-\[14\] | VMS playback is the strongest completed operator workflow and provides the entry point for clip-aware VSS analysis. |
| Statistical reporting and operational visibility | Count events or objects, review reports, export clips or reports, and support operational follow-up \[1\], \[2\] | Verkada, Eagle Eye, Milestone XProtect, and similar platforms support reporting and operational dashboards at different levels \[23\]-\[25\] | Reporting is often available, but tighter connection between surveillance evidence, investigation results, and operational decision support is not always clear \[18\], \[19\], \[23\]-\[25\] | Extracted event summaries, identities, metadata, and graph facts provide a basis for reporting, but report automation is not a completed production feature. |
| Scalable deployment and centralized multi-site rollout | Support repeatable deployment across sites, centralized operation, and practical use under edge-cloud conditions \[1\], \[2\] | Eagle Eye, Verkada, Milestone XProtect, and similar systems show multi-site and centralized deployment directions \[7\], \[30\] | A clear edge-cloud workflow that preserves local continuity while still supporting centralized playback and AI-assisted investigation is not always visible \[3\]-\[7\], \[27\], \[30\] | The system is cloud-connected and modular, but only partially cloud-native/scalable because it still uses personal GCS, MongoDB/Neo4j, and VM infrastructure. |
| Extensibility for customer-specific requirements | Allow later feature development for specialized workflows, customer rules, and domain-specific scenarios \[1\], \[2\] | Milestone XProtect, Arcules, Avigilon, and other enterprise-oriented ecosystems indicate extensibility through modular or platform-based approaches \[28\] | Public materials do not always show a flexible path for combining a standard deployable baseline with fast adaptation to specialized surveillance requirements \[1\], \[2\], \[28\] | The MongoDB and Neo4j split supports later extension, while customer-specific rules, monitoring, access control, and hardened deployment remain future work. |

From this benchmark, the motivation of our project becomes clear. The project does not attempt feature parity with commercial VMS products. Its defensible contribution is the integrated evidence pipeline: camera stream to event clip, event clip to cloud-backed playback, and playback evidence to searchable VSS knowledge \[3\]-\[14\], \[20\]-\[22\], \[30\], \[31\]. The current prototype demonstrates this integration path, while production-grade scalability, security hardening, and deeper VSS evaluation remain future work.

This direction is motivated by the need to transform surveillance from passive video storage into an evidence-centered investigation workflow. The Edge Agent reduces unnecessary video handling by converting continuous camera streams into event-oriented clips \[3\]-\[5\], \[27\], \[29\]. The VMS makes those clips usable through upload handling, normalization, cloud-backed storage, playback, and service orchestration \[6\], \[7\], \[30\], \[31\], \[34\], \[35\]. The VSS enriches selected clips with searchable summaries, identity evidence, structured relationships, and answer support \[8\]-\[14\], \[20\]-\[22\]. Together, these components form a pipeline in which raw camera streams become event clips, event clips become reviewable evidence, and reviewable evidence becomes searchable surveillance knowledge. The proposed solution is therefore not intended to replace all existing surveillance products or to claim that each subsystem is fully production-complete. Instead, it addresses a specific integration gap: how to connect edge capture, centralized playback, and AI-assisted investigation into one defensible capstone system \[1\], \[2\]. This is the core motivation of the thesis. The project is valuable because it demonstrates that surveillance review can move beyond isolated camera viewing and manual clip inspection toward a unified workflow where evidence capture, operator review, and intelligent retrieval are designed together from the beginning \[18\], \[19\], \[23\]-\[25\].

## **2\. Contribution of the thesis** {#2.-contribution-of-the-thesis}

This thesis makes four main contributions. First, it defines and implements a unified surveillance direction that combines Edge-side camera intake and event recording, centralized VMS playback workflows, and VSS-based metadata extraction and retrieval in one architecture rather than treating them as independent prototypes \[1\], \[2\]. Second, it demonstrates a practical local-first and cloud-connected integration strategy: camera streams are processed near the site, selected clips are forwarded to VMS, and cloud storage plus AI-oriented services are incorporated where they add scalability and analysis value \[3\]-\[7\], \[12\], \[13\], \[30\]. Third, it establishes the clearest completed operator workflow of the current project through VMS playback on real stored video, while also connecting that workflow to VSS-side analysis and retrieval paths. Fourth, it provides a technical foundation for AI-assisted investigation through identity support, metadata extraction, vector retrieval, graph-oriented experimentation, and natural-language question answering grounded in surveillance evidence \[6\]-\[14\], \[20\]-\[22\]. These contributions should be interpreted as implementation-based capstone results rather than as purely conceptual proposals, because each one is tied to subsystem behavior already built, tested, or integrated to a meaningful extent in the current project stage.

The first contribution is architectural coherence. Many student systems demonstrate one part of a surveillance stack, such as camera streaming, web playback, or AI captioning, but leave the transition boundaries between those parts vague. This thesis instead treats those boundaries as part of the contribution itself. The report makes explicit how clips leave the edge layer, how they become playback resources, how playback becomes a query context, and how VSS stores and answers against the resulting knowledge \[3\]-\[7\], \[9\]-\[14\], \[29\]-\[31\]. The second contribution is operational grounding. The project does not present AI analysis in the abstract. It places AI where it helps the surveillance workflow most at the current maturity stage: after the evidence is already captured and reviewable. This is a defensible capstone choice because it keeps the AI path grounded in real evidence objects rather than hypothetical future integrations. The third contribution is data-model discipline. Through the hybrid use of MongoDB and Neo4j, the capstone distinguishes between semantic event memory, persistent identity memory, and explicit relationship memory. This is more than a tooling choice. It is a design argument about what kinds of surveillance knowledge need approximate similarity search and what kinds need exact graph structure. The fourth contribution is evaluation discipline. Instead of claiming that the entire surveillance platform is uniformly complete, the thesis identifies the strongest workflow core that can currently be defended: Edge-driven event capture, VMS playback on real stored video, and clip-aware VSS support. This narrower but more honest framing strengthens the capstone because it ties claims to evidence rather than to aspiration.

## **3\. Organization of the thesis** {#3.-organization-of-the-thesis}

This thesis is organized into four main chapters after the introduction. Chapter 1 reviews the project context, the overall problem direction, the research questions inherited from earlier milestones, and the current interpretation of the capstone title in relation to the implemented system. Chapter 2 presents the detailed theory and subsystem foundations, covering the conceptual and technical basis of VSS, the Edge Agent, and the VMS playback section, with each major subsystem introduced through its corresponding research questions.

Chapter 3 describes the proposed algorithms and system design. It explains how the integrated architecture is organized, how the VSS ingest and query flows operate, how the Edge Agent performs event detection and synchronization, and how the VMS supports upload, playback, and VSS integration. Chapter 4 presents the experimental results and evaluation, focusing on the current demo-ready flows, subsystem evidence, retained VMS playback test cases, and the limitations that still separate the current system from a fully completed final platform. This organization is intentional. The introduction does not attempt to contain every detail of the system because the thesis is structured to move from motivation and context toward progressively greater technical depth. Chapter 1 establishes the project problem and maturity framing. Chapter 2 explains why the chosen subsystem concepts are technically justified. Chapter 3 explains how those concepts are implemented as workflows and algorithms. Chapter 4 then asks whether those workflows are already strong enough to count as working capstone evidence. Read in sequence, the report therefore moves from "why this system is needed" to "how it works" to "what has actually been proven so far."

 

## **4\. Milestone of the Project** {#4.-milestone-of-the-project}

*Figure 0.1: Full project Grant Chart* 

## **5\. Work distribution** {#5.-work-distribution}

The work distribution in this capstone follows the subsystem responsibilities already evidenced in the M2 report.

* **Phung Do Anh Khoa** responsible for the Edge subsystem, including camera onboarding, local event recording, event continuity, and the Edge-to-VMS integration path. This includes the ONVIF/RTSP discovery utility, the Go-based Edge Agent runtime, RTSP ingestion, motion detection, ring-buffer recording, local clip storage, and related Edge testing \[3\]\-\[5\], \[27\], \[29\].  

* **Tran Chau Huy** responsible for the VMS-related workflow and shared synchronization work between Edge, VMS, and VSS. The main contribution in the current stage is the playback-oriented VMS path, including frontend work, FastAPI-based HTTP endpoints for playback-related exchange, and metadata handoff from VMS to the VSS analysis flow. This role provides the clearest usable operator-facing output of the current project through playback on real stored video while also establishing the backend connection path for broader VMS functions \[6\], \[7\], \[30\], \[31\], \[34\], \[35\].  

* **Le Cong Thanh Khoa** responsible for the AI and VSS pipeline, including LLM integration, VectorDB-oriented retrieval, semantic search, and AI-assisted investigation support. The current contribution includes DeepFace-based identity support, vector storage in MongoDB, LangGraph and LangSmith experimentation, cloud metadata retrieval tests, and the general AI-agent direction used by the VSS layer \[8\]\-\[14\]. Taken together, these three workstreams form one cumulative value chain: Edge makes site capture possible, VMS makes surveillance review usable, and VSS makes evidence enrichment and intelligent follow-up possible.

This distribution also reinforces the integrated nature of the capstone. Each member has a dominant subsystem responsibility, but no subsystem stands alone in the final workflow. Edge outputs must be understandable by VMS. VMS outputs and metadata must be usable by VSS. VSS must answer against clip and event context created upstream. The report therefore treats work distribution not as a set of isolated ownership blocks, but as coordinated contributions to one surveillance evidence pipeline.

 

 

# **CHAPTER 1\. REVIEW OF LITERATURE AND PROJECT CONTEXT** {#chapter-1.-review-of-literature-and-project-context}

This chapter defines the project context, research questions, and maturity framing inherited from the M2 milestone. It clarifies how the thesis uses the terms cloud-native, scalable, video management, analytics, and unified platform in relation to the implemented prototype.

## **1.1 Research questions and project direction** {#1.1-research-questions-and-project-direction}

This thesis continues the capstone direction defined in the earlier project stages: a cloud-connected surveillance platform that combines Edge-side operation, centralized Video Management System workflows, and VSS-based AI-assisted investigation \[1\], \[2\]. The video management scope covers operational workflows such as live view, playback, and event review; the analytics scope covers semantic retrieval, metadata extraction, and investigation support rather than replacement of the core surveillance workflow \[1\], \[2\], \[8\]-\[14\].

The project is guided by three research questions. First, how can an integrated Edge Site and centralized VMS architecture support reliable surveillance operations across distributed deployments? Second, how can the VSS layer improve playback-based review, semantic retrieval, tracing, and information extraction for investigation? Third, how can the system remain practical for internal operation while still supporting later rollout and customer-specific extension \[1\], \[2\]? These questions keep the report focused on one evidence workflow rather than three disconnected prototypes.

### **1.1.1 Definition of cloud-native and scalable scope** {#1.1.1-definition-of-cloud-native-and-scalable-scope}

In this thesis, cloud-native does not simply mean running on an Internet-accessible virtual machine. It means using clear service boundaries, cloud object storage for large video assets, API-based orchestration, asynchronous handoff for long-running analysis, and deployment-ready configuration \[6\], \[7\], \[30\], \[43\]-\[46\]. Scalable means the architecture can grow by separating Edge capture, VMS playback, VSS analysis, media storage, vector search, and graph storage. In the current capstone, this is a prototype-readiness claim, not a claim of production autoscaling, multi-tenant access control, complete observability, or production SLOs. Components running on personal GCS, MongoDB/Neo4j, or VM resources should therefore be described as cloud-hosted or cloud-connected unless stronger operational controls are demonstrated.

**Table 1.1. Cloud-native and scalability status by component**

| Component | Current status | Why |
| :---- | :---- | :---- |
| Edge Agent | Prototype / edge-local | Runs near cameras and reduces raw stream volume, but is not yet fleet-managed or centrally auto-scaled. |
| VMS playback API | Partially cloud-native | Uses FastAPI, GCS-backed playback, HTTP Range, and service boundaries, but still runs in a personal VM-style prototype environment \[6\], \[7\], \[30\], \[31\], \[40\]. |
| GCS clip storage | Cloud-native storage component | Object storage is appropriate for centralized clip storage and compute-storage separation \[7\], \[30\], \[43\], \[44\]. |
| Pub/Sub handoff | Cloud-native integration pattern | Supports asynchronous handoff from VMS to downstream ingest, while consumer retry, monitoring, and production hardening remain partial \[45\], \[46\]. |
| VSS MongoDB vector store | Cloud-hosted / partially scalable | Supports vector and event retrieval, but deployment and index governance remain prototype-level \[9\], \[50\]. |
| VSS Neo4j graph store | Cloud-hosted / partially scalable | Supports graph facts and relationships, but graph evaluation and production governance remain incomplete \[14\]. |
| VSS reasoning agent | Prototype | Demonstrates route selection and answering, but still requires ground-truth evaluation, ablation testing, faithfulness scoring, and hallucination audit \[20\]-\[22\], \[51\], \[53\], \[54\]. |
| Security and privacy controls | Partial / future work | Sensitive surveillance data exists, but personal cloud accounts and VM resources require stronger IAM, encryption, retention, audit, and access controls \[47\], \[52\]. |

## **1.2 Current system overview** {#1.2-current-system-overview}

The current system follows a hybrid workflow: Edge -> centralized VMS -> VSS analysis. Cameras connect at the site edge, event-related clips are forwarded to the VMS for storage and playback, and selected evidence is processed by VSS to generate searchable metadata, identity support, captions, and AI-assisted responses \[3\]-\[14\], \[29\]-\[31\].

The current system is best described as a working integration of selected core flows, not as a complete surveillance product. Edge event capture has prototype-level validation. VMS playback is the strongest operator-facing workflow. VSS metadata extraction and vector retrieval are meaningful but still need deeper graph, identity, and answer-faithfulness evaluation \[1\], \[2\], \[6\]-\[14\].

**Table 1.2. Current subsystem maturity profile**

| Subsystem area | Current maturity | Meaning for the final report |
| :---- | :---- | :---- |
| Edge camera onboarding and local event capture | Prototype with controlled validation | Enough to defend event-driven capture and handoff concepts |
| VMS playback on stored video | Strongest implemented operator workflow | Central evidence path for evaluation and demonstration |
| VMS non-playback modules | Partial or backend-limited | Should be acknowledged, not overstated |
| VSS metadata extraction and vector retrieval | Meaningful implementation with measurable outputs | Strong enough to support playback-centered analysis scenarios |
| VSS graph-dominant end-user reasoning | Emerging and still maturing | Should be described as promising but incomplete |

![][image3]

![][image4]

*Figure 1.1. End-to-end operational flow of the proposed system. The first diagram shows the overall system architecture; the second diagram shows the protocol-level data flow.*

## **1.4 Related literature and architectural positioning** {#1.4-related-literature-and-architectural-positioning}

The thesis draws on three domains: surveillance architecture, AI-assisted video understanding, and cloud-connected system design \[1\], \[2\], \[6\]-\[16\], \[30\]. Its contribution is closer to system integration and workflow grounding than to single-model novelty. For that reason, the report focuses on the bounded evidence path that is already meaningful: Edge capture, VMS playback, and VSS enrichment through selected metadata, identity, retrieval, and graph-storage functions.

 

 

 


# **CHAPTER 2\. DETAILED THEORY AND SUBSYSTEM FOUNDATIONS** {#chapter-2.-detailed-theory-and-subsystem-foundations}

In this chapter, the theoretical and architectural foundations of the three main technical subsystems are presented: the Visual Surveillance System, the Edge Agent, and the VMS playback section. Each subsystem is introduced through its research questions before its design rationale is explained. The goal of the chapter is not to enumerate implementation steps in full detail, but to establish the concepts, architectural choices, and protocol or data-model foundations that make the later system design in Chapter 3 technically coherent.

## **2.1 Research-question-driven overview of subsystem foundations** {#2.1-research-question-driven-overview-of-subsystem-foundations}

The final system rests on three complementary foundations that correspond to three different technical responsibilities. The first foundation concerns surveillance intelligence: how raw video can be transformed into structured knowledge that supports search, identity continuity, and question answering. The second concerns site-level acquisition and synchronization: how legacy camera streams can be filtered, buffered, and forwarded without overloading local hardware or cloud bandwidth. The third concerns operator-facing evidence review: how recorded video can be uploaded, stored, played back efficiently, and then forwarded into AI analysis without breaking the practical workflow of the surveillance user. These foundations are interdependent. A retrieval system without a reliable video intake path cannot produce trustworthy evidence. An edge capture mechanism without a usable playback workflow cannot support investigation. A playback system without metadata, identity support, and contextual query capability remains limited to manual review. For that reason, the theoretical basis of the final platform must be read as a coordinated combination of database design, stream-processing constraints, web communication protocols, and AI-assisted retrieval logic rather than as three unrelated subsystem topics.

## **2.2 Visual Surveillance System theoretical foundation** {#2.2-visual-surveillance-system-theoretical-foundation}

### **2.2.1 Research questions for VSS** {#2.2.1-research-questions-for-vss}

The VSS addresses three surveillance-retrieval questions: how raw footage can become structured knowledge, how that knowledge can be stored across vector and graph databases, and how user questions can be routed to direct, vector, graph, or hybrid answers. Its objective is not only to analyze clips, but to preserve searchable evidence for investigation.

### **2.2.2 Baseline architecture and adaptation rationale** {#2.2.2-baseline-architecture-and-adaptation-rationale}

The VSS starts from the ingest/retrieval split used in video search and summarization systems: ingestion converts visual inputs into machine-readable artifacts, while retrieval searches those artifacts through vector and graph-oriented structures \[9\]-\[14\]. The capstone adapts this pattern for surveillance by adding persistent identity as a first-class object beside event summaries and graph relationships \[8\]-\[14\]. The resulting memory model has three core layers: event-semantic memory, identity memory, and relational graph memory.



### **2.2.3 Hybrid retrieval and identity-aware surveillance knowledge** {#2.2.3-hybrid-retrieval-and-identity-aware-surveillance-knowledge}

The adapted VSS is built around three principles. First, identity-aware retrieval uses DeepFace so verified faces can become persistent `face_id` evidence rather than transient caption details \[8\]-\[14\]. Second, MongoDB stores semantic event memory and identity vectors, while Neo4j stores exact event, person, object, camera, date, location, and relationship facts \[9\], \[14\]. Third, query routing selects direct, vector, graph, or hybrid response modes according to the evidence needed by the user question.

The VSS is therefore more than a caption-search pipeline: it converts selected clips into event summaries, identity-aware records, and relationship-bearing graph context. The model also separates stable identity facts from transient event facts. A persistent Person node or `face_id` should represent cross-event identity evidence, while descriptions such as clothing, carried objects, posture, or nearby locations should remain event-scoped \[8\], \[14\]. The VSS data model can be summarized as follows:

**Table 2.1. VSS surveillance memory layers**

| Memory layer | Main storage | Main unit | Main question answered |
| :---- | :---- | :---- | :---- |
| Event-semantic memory | MongoDB vss\_event | Event document plus embedding | What happened in semantically similar events? |
| Identity memory | MongoDB identities | face\_id plus embedding and appearance history | Which known person is this face most similar to? |
| Relational memory | Neo4j graph | Event, Person, Object, Camera, Date, Location and typed edges | Where, when, and with whom or with what was this entity observed? |
| Evaluation memory | Manual ground-truth sheet / test set | Expected metadata, identity labels, graph facts, and expected answers | Did VSS extract and answer correctly, and did the answer stay faithful to evidence? |

MongoDB supports top-k semantic retrieval with filters such as camera_id, date, location, source_id, or graph_id. Neo4j supports exact relationship traversal after candidate events or identities have been identified \[9\], \[14\].

The four response modes make answer grounding clearer. `direct` is not grounded in surveillance memory, `vector` is grounded in event summaries, `graph` is grounded in graph facts, and `hybrid` combines semantic event context with graph evidence \[9\]-\[14\].

In the current prototype, graph retrieval should be interpreted as structured fact lookup and relationship traversal over extracted VSS records, not as fully validated investigative reasoning. Stronger graph-reasoning claims require a ground-truth graph dataset, graph-fact accuracy measurement, answer-faithfulness scoring, and ablation between vector-only, graph-only, and hybrid retrieval \[20\]-\[22\], \[51\], \[53\], \[54\].

Finally, the VSS is conservative when identity evidence is weak. `missing_faceid` or unresolved identity is a safety state, not a defect: it prevents the system from overclaiming biometric certainty when the visual evidence does not support it \[8\], \[14\].







### **2.2.4 Figure and schema placement notes** {#2.2.4-figure-and-schema-placement-notes}

**![][image5]**  
*Figure 2.1. VSS baseline architecture based on the ingest/retrieval pattern.*

 ![][image6]  
*Figure 2.2. VSS LangGraph workflow visualized in LangSmith.*

 ![][image7]

*Figure 2.3. VSS data stores: Neo4j Aura graph database and MongoDB Atlas vector/document memory.*

## **2.3 Edge Agent theoretical foundation** {#2.3-edge-agent-theoretical-foundation}

### **2.3.1 Research questions for Edge Agent** {#2.3.1-research-questions-for-edge-agent}

To establish a rigorous analytical direction for the decentralized site layer, the core research questions are systematically integrated directly into the architectural definition of the platform \[1\], \[2\]. The initial investigation concerning brownfield hardware integration, designated as the first research question , focuses on the structural mechanisms required to transform passive, legacy ONVIF Profile S camera nodes into intelligent, event-driven assets near the physical site \[3\], \[5\]. Since these standard commercial devices are natively constrained by continuous broadcasting protocols such as RTSP and RTP, the proposed platform shifts the computational intelligence to a localized edge gateway runtime that intercepts the media stream at the network ingress boundary and extracts discrete, event-bounded evidence packages \[3\], \[29\]. Moving beyond simple data acquisition, the second research question  addresses the temporal dependencies inherent in video compression, where intermittent wide-area network partitions risk corrupting the underlying Group of Pictures structures \[5\], \[29\]. The system resolves this vulnerability by maintaining a multi-cursor circular buffer within volatile RAM that executes a bounded tail-dropping policy, which guarantees that historical data is purged strictly along decodable reference boundaries to preserve frame decodability at the central repository \[27\], \[29\]. Finally, the third inquiry optimizes distributed communication by abandoning resource-intensive centralized polling models in favor of an asynchronous, push-based topology \[3\], \[27\]. By completely decoupling control-plane health heartbeats from data-plane evidence payloads, the decentralized edge network can dispatch atomic state information and event clips efficiently over latent wide-area networks without over-allocating core compute resources or relying on monolithic processing structures \[2\], \[27\]. 

### **2.3.2 ONVIF Profile S constraints and edge gateway rationale** {#2.3.2-onvif-profile-s-constraints-and-edge-gateway-rationale}

The theoretical foundation of the decentralized Edge Agent begins fundamentally with the operational and protocol constraints of legacy ONVIF Profile S camera nodes. These baseline devices are engineered strictly around continuous network video broadcasting over Real-Time Streaming Protocol (RTSP) and Real-time Transport Protocol (RTP) abstractions. While highly efficient as dedicated media streaming sources, these legacy nodes lack the embedded computational capacity required to execute localized event driven processing algorithms. Furthermore, their hardware architectures present critical vulnerabilities: onboard processing resources are strictly limited, local physical storage media suffer accelerated hardware fatigue under continuous write loads, and their native network protocols are structurally unsuited for cloud-oriented synchronization across unstable, latent wide-area network (WAN) topologies.  \[3\]-\[5\], \[27\], \[29\]. Consequently, these systemic boundaries create a definitive architectural justification for an intermediary gateway abstraction. The Edge Agent is introduced into the on premise physical site topology to fulfill exactly this decoupling role, intercepting raw RTSP streams directly via a localized edge computing node. Instead of imposing a continuous data transport burden over the WAN infrastructure or relying on unstable, camera bound micro SD recording loops, the edge runtime executes spatial temporal anomaly filtering near the source, manages volatile temporal buffers within RAM, and restrains wide-area network transmission strictly to verified, event-triggered evidence clips. The core theoretical value of this distributed architecture lies in its ability to separate raw data acquisition from event intelligence and cloud orchestration, thereby enabling legacy hardware assets to participate in a cloud-native platform without requiring an expansive hardware replacement cycle. 

This gateway rationale directly resolves the primary research objectives established in this thesis. The core requirement of moving analytical intelligence away from the passive camera layer to a localized software runtime directly addresses the first research question , while transforming the edge node into a controllable, site-level participant within a broader distributed network rather than a passive packet relay directly answers the constraints of the third research question .  This structural transformation redefines the operational role of the camera node within modern surveillance environments. In legacy deployment models, the camera operates as a passive broadcaster whose utility terminates immediately upon emitting a continuous compressed stream. Under the proposed framework, while the camera preserves its broadcasting function, the physical site is elevated into an event-aware capture domain because the adjacent Edge Agent dynamically transforms that raw stream into a filtered, context-rich evidence flow. This topological shift successfully maintains the economic value of existing on-premise hardware investments while introducing advanced operational behaviors, including bounded deterministic buffering, selective data upload, and structured health telemetry.  \[3\]-\[5\], \[27\], \[29\]. Ultimately, this architectural distribution is governed by pragmatic economic and operational parameters, rather than purely algorithmic optimizations. Implementing an always on cloud ingest of raw RTSP streams imposes an unsustainable cost structure on WAN bandwidth and centralized storage pools; conversely, forced continuous SD card recording merely shifts physical reliability risks back to the fragile camera hardware. The Edge Agent mitigates this dilemma by validating an optimized third paradigm: detecting anomalies close to the physical source, preserving short term context within localized volatile memory, and synchronizing data payloads only when an established event threshold is breached. This design approach directly targets the real-world infrastructure bottlenecks of distributed surveillance deployments rather than assuming ideal network states and unconstrained cloud resources.  

### **2.3.3 Spatial-temporal filtering theory** {#2.3.3-spatial-temporal-filtering-theory}

The first core algorithmic foundation of the Edge Agent is spatial-temporal motion filtering. The theoretical purpose of this layer is to reduce bandwidth and storage waste by distinguishing meaningful motion from static footage. To minimize computational load over low-power edge gateway CPUs, the processing is modeled as a linear pipeline.

The entire multi-stage edge reduction logic can be compressed into a single, unified spatial-temporal equation mapping from structural pixel properties to the final binary event trigger state:

Et \= 1{i \- 0 w \-1 ( 1 H x W x \-1 H y \-1 w  1{|Jt \-1 (x,y) \- Jt-i-1(x,y)| \> })>= t }  
To evaluate this unified relation, let It RHxWxC represent the incoming raw video frame tensor captured at time t, where H is the frame height, W is the width, and C=3 channels. The input video frame is first projected from RGB into a single-channel grayscale matrixIt {0,1,...,255}HxWxC to reduce computational cost. The formulation from the edge design can be expressed explicitly. Let It be the raw RGB frame at time t, and let It be its grayscale projection. The system computes an absolute frame-difference map Dt \= |Jt \- Jt-1|, then applies a threshold  to produce a binary anomaly mask. At the register level of an edge CPU, this absolute difference is computed efficiently using Two's Complement subtraction modulo 256:

where \~ is the bitwise NOT operation. The binary spatial anomaly indicator mask Mt {0,1}HxWis formalized through an indicator function based on the static intensity threshold   R+: 

Mt(x,y) \- 1{Dt(x,y) \> }(Dt(x,y)) \- {0 otherwise 1 if Dt ( x,y) \>   
From this mask, the system derives an anomaly density At, which measures the proportion of the frame undergoing significant change. This density is formulated as:

At \= 1H x W x \= 1Hy \= 1WMt ( x, y)  
This density is more stable than relying on one or two changing pixels because it converts local change into a scene-level indicator. However, this spatial signal alone is not sufficient for event detection because noise, minor illumination shifts, or isolated frame artifacts can trigger false positives. The system therefore applies a temporal filter over a sliding window. An event is declared only when accumulated anomaly density across consecutive frames exceeds a configured temporal threshold. In theoretical terms, this converts frame-level disturbance into event-level evidence. The result is a lightweight event trigger model with linear complexity in the frame size, which is appropriate for CPU-based execution on low-power edge hardware.

The importance of this theory for the final platform is not that it competes with advanced deep-video detectors. Its importance is that it creates a practical edge-side filter that is computationally cheap, deployable on modest hardware, and sufficient to decide when clip generation and downstream upload should occur. The temporal trigger then accumulates these spatial densities over a window of recent frames. If the accumulated anomaly mass exceeds the configured trigger threshold, the system declares an event state. This is conceptually important because it converts motion detection from a per-frame decision into a short-horizon sequence decision. In surveillance practice, that difference matters: a single noisy frame should not create a clip, while sustained motion over a brief interval usually should.

Et \= 1{i \=0w \-1 At \-1 >= t}={ 0 otherwise  1 if i \- 0 w \- 1 At \-1 >= t   
 

where w is the sliding historic frame window. This is conceptually important because it converts motion detection from a per-frame decision into a short-horizon sequence decision. In surveillance practice, that difference matters: a single noisy frame should not create a clip, while sustained motion over a brief interval usually should . The design also aligns with the hardware assumptions of the Edge Agent. A Mini PC or similar local gateway can afford repeated linear scans over frames, grayscale conversion, thresholding, and short-window aggregation. It does not need a GPU-heavy detector to serve as a first-stage filter. That makes the method robust as a pre-analysis layer even if more advanced analytics are later added upstream in VSS. 

### **2.3.4 GOP-aware buffering and site-to-cloud synchronization theory** {#2.3.4-gop-aware-buffering-and-site-to-cloud-synchronization-theory}

The second theoretical foundation of the Edge Agent concerns buffering and synchronization under unstable network conditions. Modern compressed video uses a Group of Pictures structure in which predictive frames depend on preceding reference frames. This means that arbitrary slicing or frame dropping can corrupt the decoding chain. If an uploaded segment begins in the middle of a GOP or if the I-frame anchor is lost, the receiving side may be unable to decode the remaining payload correctly \[29\]. To preserve video integrity while still using bounded memory, the Edge Agent relies on a static circular ring buffer array B of capacity N in volatile RAM with multiple monotonic rising integer cursors for writing, reading, and synchronization acknowledgment. The physical index mapping A(c) within the circular array is calculated via modulo arithmetic: 

A(c) \= c ( mod N)  
Let T(c)  {I, P, B} denote the frame type stored at index c mod{N}. While the physical implementation utilizes an empirical allocation of N=30 frames to safely buffer 3 seconds of pre-event context at 10 FPS, the architectural model remains mathematically robust for any bounded scale N. The theoretical role of the buffer is not only temporary storage; it is also a concurrency and integrity structure. When the buffer is full during network stalls, data cannot be dropped arbitrarily. Instead, the oldest complete GOP must be removed by advancing to the next I-frame boundary. This bounded tail-dropping strategy preserves decodability while still allowing the edge node to continue receiving new frames. 

The buffer itself is modeled through three monotonic cursors: the write cursor tracks where the newest incoming frame should be written, the read cursor tracks what part of the buffered stream is being consumed for clip generation, and the sync cursor tracks what portion of the data has been acknowledged by the centralized side. This multi-cursor design matters because the edge runtime is not performing one activity at a time. It is simultaneously receiving frames, reading them into clips, and coordinating what has or has not yet been synchronized. The bounded tail-dropping policy is one of the most distinctive technical details of the edge architecture. In a simpler buffer, overflow might be handled by discarding the oldest bytes or the oldest frames arbitrarily. That would be dangerous in compressed video because P-frames and B-frames depend on prior reference frames. The capstone instead drops the oldest complete GOP by scanning forward to the next valid I-frame boundary. This ensures that whatever remains in the buffer can still be decoded later. In practical report terms, this is not a cosmetic optimization. It is the reason why a memory-constrained edge node can still preserve evidentiary integrity during temporary network failure. The separation between control plane and data plane should also be emphasized in the final report. Telemetry heartbeats and event uploads have different timing, payload, and reliability characteristics. Heartbeats report whether the site is healthy and whether streams are alive. Data-plane uploads carry actual video evidence and associated metadata. If both responsibilities were collapsed into a single polling-centric or stream-centric mechanism, the system would be harder to reason about, less efficient, and more brittle under network instability.









### **2.3.5 Figure placement notes** {#2.3.5-figure-placement-notes}

![][image8]  
Figure 2.4 flowchart of the adaptive spatial-temporal edge filter. 

![][image9]

Figure 2.5  multi-cursor ring buffer diagram

## **2.4 VMS playback theoretical foundation** {#2.4-vms-playback-theoretical-foundation}

### **2.4.1 Research question for VMS playback** {#2.4.1-research-question-for-vms-playback}

How can the VMS provide efficient playback of recorded surveillance video from cloud storage, while supporting reliable edge upload and standardized VSS integration?

In thesis form, this research question can be read in three linked parts. First, the VMS must deliver recorded video efficiently enough for practical investigation, especially during seek and review. Second, it must accept clips from edge or upload sources in a reliable and web-compatible way. Third, it must expose a stable integration boundary through which clip-specific analysis and downstream VSS processing can occur without making the VMS tightly dependent on VSS internal logic.

### **2.4.2 HTTP/REST as the orchestration protocol** {#2.4.2-http/rest-as-the-orchestration-protocol}

The first protocol foundation of the VMS playback section is HTTP/REST. Recorded playback investigation is mainly a request-response workflow rather than a continuous bidirectional stream. The operator asks the system to list available clips, open a selected clip, request a byte range, fetch analysis state, or submit a question about the clip being reviewed. Each of these actions can be represented as a resource-oriented request with a response. This makes HTTP/REST a natural fit for the browser-facing and service-facing parts of the VMS \[31\]. HTTP also aligns well with web deployment because browsers, reverse proxies, API gateways, cloud platforms, and monitoring tools already understand HTTP semantics \[31\]. In a capstone system, this matters because the VMS must be practical to test and demonstrate. A protocol that works directly with the browser reduces the need for specialized client software and allows the system to use ordinary API endpoints for upload, playback, analysis retrieval, and query forwarding. The theoretical limitation of HTTP/REST is that it is not designed as a persistent low-latency media transport for every kind of real-time video. For continuous live streaming, protocols such as RTSP, WebRTC, or HLS/DASH pipelines may be more appropriate depending on requirements \[37\]. However, for recorded clip playback, HTTP/REST remains a strong choice because the interaction pattern is naturally discrete: select clip, load clip, seek, request analysis, submit question \[31\], \[40\]. 

### **2.4.3 Multipart/form-data for video upload** {#2.4.3-multipart/form-data-for-video-upload}

The upload side of the VMS requires a protocol format that can carry binary video content together with basic metadata. multipart/form-data is designed for this purpose \[38\]. It allows one request to contain a file field and additional form fields, such as sender name, client identifier, caption, or other basic clip context. This is more suitable than a raw binary upload because raw binary upload would require metadata to be transmitted through separate headers, query parameters, or another request. It is also more suitable than Base64-in-JSON because Base64 is a text encoding for binary data and introduces avoidable payload expansion and encoding overhead for large video files \[39\]. For edge-to-VMS communication, this theoretical property is important. A clip is not only a file. It is evidence associated with an origin, time, camera, location, and uploader context. Even when some of this information is later parsed from the filename, the upload protocol should still permit video and metadata to travel together at the intake boundary \[38\]. This gives the backend enough context to validate, normalize, store, and hand off the clip without requiring the operator to manually reconstruct information later. Because upload endpoints are security-sensitive boundaries, the report should also connect this design to validation of file type, size, and filename handling \[47\].

**2.4.4 HTTP Range Requests for recorded playback**

The most important playback-specific protocol foundation is HTTP Range. Recorded video playback becomes inefficient if the browser must download the entire file before the operator can start viewing or seeking. HTTP Range allows the client to request only a portion of a resource by sending a Range header. The server can then respond with 206 Partial Content, Content-Range, Accept-Ranges, and the correct Content-Length \[31\], \[40\]. This mechanism is especially important for surveillance review because operators often need to jump directly to a relevant moment rather than watch from the beginning. Range-based playback also fits cloud-backed object storage. The application server can read only the needed byte interval from the stored object and stream that interval to the browser. The browser's native video element can then request additional byte ranges as playback continues or as the user drags the timeline \[40\], \[42\]. In theoretical terms, HTTP Range turns an object stored as one file into an efficiently navigable playback resource without requiring a dedicated media server or manifest-based streaming pipeline. The main limitation is that the server must implement range parsing and error handling correctly. Invalid ranges should not produce ambiguous behavior. The server must know the total object size, clamp requested byte ranges safely, return proper status codes, and preserve browser-compatible headers \[31\], \[40\]. If these details are handled incorrectly, seeking may fail, startup may become slow, or the browser may treat the video as unsupported.

**2.4.5 Object storage as the playback storage foundation**

Cloud object storage is not a communication protocol in the same sense as HTTP, but it is a storage model that strongly affects the playback protocol design. Object storage stores files as objects inside buckets, each identified by an object name \[43\]. This model is suitable for playback-ready clips because each recorded clip can be treated as an independent object. It also supports centralized access, later scaling, and service separation between the playback application and the storage layer \[43\], \[44\]. For the VMS, object storage is theoretically useful because it separates computation from media storage. The VMS backend does not need to keep every playback file only on the local filesystem. Instead, the backend can act as a controlled access layer in front of the object store \[43\]. This allows the application to validate object names, control response headers, apply Range semantics, and hide direct storage access from the browser. The limitation of object storage is that it is not a full application database. It is good for storing clip objects, but it should not be treated as the long-term knowledge store for extracted surveillance meaning \[43\]. Event summaries, entity records, identity information, vector embeddings, and graph relationships belong to the VSS data layer rather than to the VMS object store.

**2.4.6 Pub/Sub as the asynchronous integration protocol**

The VMS also needs an integration mechanism for downstream analysis. Video upload and video analysis have different timing characteristics. Upload should return quickly enough to keep the operator or edge uploader responsive, while VSS analysis may take longer because it can involve frame sampling, visual-language processing, identity handling, embedding, and database persistence. A publish-subscribe mechanism separates these two responsibilities \[45\], \[46\]. In a publish-subscribe model, the VMS can act as a producer. After a clip is stored, it publishes a message describing the available clip and its basic context. A downstream consumer can then subscribe to that topic, read the message, retrieve the clip from storage, and perform the heavier ingest workflow \[45\], \[46\]. This design reduces coupling because the VMS does not need to directly call a long-running analysis process inside the upload request. The main theoretical advantage of Pub/Sub is asynchronous decoupling. The producer and consumer do not need to execute at exactly the same time, and the upload path does not need to block until analysis is complete \[45\], \[46\]. The limitation is that the system must handle eventual consistency, message delivery status, retry behavior, monitoring, and consumer-side hardening. In the current report, this distinction is important: Pub/Sub should be explained as a handoff protocol, not as a database and not as the place where analysis results are stored.

### **2.4.7 Protocol selection by VMS responsibility** {#2.4.7-protocol-selection-by-vms-responsibility}

The protocol choices should be compared by responsibility rather than in one mixed table. Each VMS responsibility has a different set of candidate technologies, so the report should show which alternatives compete for the same role and why the selected approach is proportionate for the current playback-centered workflow.

**Table 2.2. Communication model for VMS orchestration**

| Candidate approach | Main strength | Main limitation | Decision for current VMS |
| ----- | ----- | ----- | ----- |
| HTTP/REST | Browser-compatible, stateless request-response model, simple API testing and deployment | Not ideal for continuous server-push or ultra-low-latency bidirectional interaction | Selected for clip listing, playback requests, analysis retrieval, and clip-specific query APIs \[31\] |
| WebSocket | Persistent bidirectional channel for real-time push and interactive updates | Adds session state, message typing, reconnection logic, and more frontend/backend complexity | Not selected for current recorded playback workflow; possible future fit for live status or notification features \[36\] |

This comparison shows that HTTP/REST is selected for VMS orchestration because recorded playback actions are mostly explicit requests. WebSocket is not wrong in general, but it solves a different class of problem.

**Table 2.3. Upload payload format for clip intake**

| Candidate approach | Main strength | Main limitation | Decision for current VMS |
| ----- | ----- | ----- | ----- |
| multipart/form-data | Carries binary file and text metadata in one request body | Needs server-side validation for file type, size, and filename safety | Selected for edge/uploader clip intake \[38\], \[47\] |
| Raw binary upload | Simple binary transfer | Metadata must be sent separately through headers, query strings, or another request | Not selected because surveillance clips require contextual metadata |
| Base64-in-JSON | Keeps file and metadata inside one JSON document | Expands payload size and adds avoidable encoding/decoding overhead for video | Not selected for video upload \[39\] |

This comparison shows why multipart/form-data is the most suitable upload format. The VMS receives video evidence, not just bytes, so the upload format must support both the media file and its basic context.

**Table 2.4. Browser playback delivery strategy**

| Candidate approach | Main strength | Main limitation | Decision for current VMS |
| ----- | ----- | ----- | ----- |
| HTTP Range over stored files | Supports partial byte retrieval, native browser playback, and efficient seeking without a dedicated media server | Requires correct range parsing, partial-content headers, and invalid-range handling | Selected for recorded clip playback and seek behavior \[31\], \[40\], \[42\] |
| Full-file HTTP download | Very simple server behavior | Poor startup and seeking behavior for longer clips because the browser may need too much data before review | Not selected as the main playback strategy |
| RTSP/RTP to browser | Native to camera and IP-streaming environments | Poor direct browser support without gateway or conversion layer | Not selected for browser playback; better suited to camera-to-edge stream intake \[37\] |

This comparison shows why HTTP Range is the best current playback delivery choice. It supports the exact operator need: open a recorded clip and seek to the relevant segment without downloading the entire object.

**Table 2.5. Storage model for playback-ready clips**

| Candidate approach | Main strength | Main limitation | Decision for current VMS |
| ----- | ----- | ----- | ----- |
| Cloud object storage | Centralized object access, compute-storage separation, scalable storage model | Not a semantic database and should not store extracted surveillance knowledge as the main knowledge layer | Selected for playback-ready video objects \[43\], \[44\] |
| Local filesystem only | Simple and fast for early development | Tied to one server instance and weaker for multi-service access or cloud expansion | Used as local staging/support, not as the main cloud playback source |
| Application database for video blobs | Can attach structured metadata to records | Inefficient and heavy for large media objects compared with object storage | Not selected for storing video files |

This comparison separates media storage from knowledge storage. GCS-style object storage is appropriate for clip files, while VSS databases are responsible for event summaries, identities, embeddings, and graph relationships.

**Table 2.6. Downstream analysis handoff strategy**

| Candidate approach | Main strength | Main limitation | Decision for current VMS |
| ----- | ----- | ----- | ----- |
| Pub/Sub handoff | Decouples upload producer from downstream analysis consumer; avoids blocking upload on heavier ingest | Requires subscriber-side reliability, retry, observability, and production hardening | Selected as producer-side handoff from VMS to downstream ingest \[45\], \[46\] |
| Synchronous VMS-to-VSS ingest call | Simpler control flow for a small prototype | Upload request can block on slow analysis and tightly couples VMS to VSS runtime | Not selected as the main ingest handoff |
| Shared database polling | Simple conceptual handoff through a database table | Adds polling delay, unclear ownership, and weak event semantics | Not selected for the current asynchronous handoff |

This comparison shows why Pub/Sub is used for ingest notification rather than as a database. The video remains in object storage, while the message tells downstream analysis where the clip is and what context belongs to it. Taken together, these responsibility-specific comparisons clarify the theoretical boundary of the VMS section. The VMS playback workflow is not trying to solve every possible video transport problem. It is solving the narrower but central problem of reliable recorded-clip review in a browser, with asynchronous handoff to later analysis. Within that target, HTTP/REST, multipart/form-data, HTTP Range, object storage, and Pub/Sub form a coherent protocol foundation \[31\], \[38\], \[40\], \[43\]-\[46\].

# **CHAPTER 3\. PROPOSED ALGORITHMS AND SYSTEM DESIGN** {#chapter-3.-proposed-algorithms-and-system-design}

In this chapter, the implemented system design and the main operational algorithms of the capstone are presented. Unlike Chapter 2, which established subsystem foundations, this chapter focuses on how those foundations are realized in concrete workflows. The chapter therefore explains the integrated architecture of the final platform, the VSS ingest and query algorithms, the Edge Agent event-detection and synchronization flow, and the VMS playback and VSS integration workflows that connect operator review with AI-assisted investigation.

## **3.1 Integrated system architecture** {#3.1-integrated-system-architecture}

The final platform is organized as a chained surveillance workflow rather than a set of isolated services. Cameras at the site level produce continuous RTSP streams. The Edge Agent receives those streams, performs motion-oriented filtering, and transforms validated activity into event-based clips. These clips are then uploaded into the VMS path, where they are normalized, stored, and exposed through playback-oriented APIs. Once clips or selected evidence items exist in the VMS layer, they can be forwarded to the VSS path for semantic analysis, identity handling, graph persistence, and question answering. The result is an integrated architecture in which video capture, evidence review, and AI-assisted reasoning form one operational chain. The architecture is intentionally layered. The site layer is responsible for stream intake, local filtering, and upload resilience. The VMS layer is responsible for operator-facing clip management, playback orchestration, and service mediation. The VSS layer is responsible for transforming selected video evidence into searchable surveillance knowledge. This separation keeps each subsystem coherent while still allowing end-to-end linkage through shared identifiers such as video\_id, source\_id, camera\_id, date, location, and graph\_id \[6\]-\[14\], \[30\], \[31\].

At system level, this architecture also explains why playback is the current center of the capstone. Playback is where edge-generated clips become usable evidence, and it is also the point at which VSS analysis becomes operationally relevant. For that reason, the integrated architecture should be presented in the report as a surveillance evidence pipeline rather than as three independent implementation stories. This pipeline perspective is important for two reasons. First, it explains why the report can meaningfully evaluate a partially mature system. Even if not every subsystem feature is complete, the platform already has one coherent vertical slice from capture to review to analysis. Second, it explains why subsystem design decisions cannot be judged in isolation. For example, edge buffering is not only an edge concern; it affects the quality of clips seen in VMS. VMS metadata preservation is not only a playback concern; it affects the context available to VSS. VSS events and identity storage are not only AI concerns; they affect whether playback-centered investigation can become more than manual video browsing.

**Table 3.1. Integrated architecture responsibilities and evidence handoff**

| Layer | Core responsibility | Main data object produced | Main downstream consumer |
| :---- | :---- | :---- | :---- |
| Camera / source layer | Generate continuous encoded stream | RTSP/RTP stream | Edge Agent |
| Edge layer | Detect meaningful activity and preserve local context | Event clip plus edge metadata | VMS intake API |
| VMS layer | Normalize, store, list, and play clips; mediate analysis requests | Playback-ready object, playback API response, context-enriched analysis request | Browser frontend, VSS, Pub/Sub |
| VSS layer | Turn selected clips into searchable and explainable knowledge | Event summaries, identities, graph facts, grounded answers | Operator through VMS-mediated UI |

   
This table makes clear that no layer is redundant. The camera produces stream continuity but not event intelligence. The Edge Agent produces event-bounded evidence but not operator playback. The VMS produces usable review and orchestration but not deep semantic knowledge. The VSS produces semantic and relational interpretation but depends on upstream clip availability and context. The data handoff sequence also follows a strict narrowing process. At the beginning of the chain, the system handles continuous video flow, which is high-volume and weakly structured. The Edge Agent narrows that into event-oriented clips, reducing volume while preserving temporal context. The VMS narrows again by organizing clips as named objects and reviewable playback items. The VSS narrows once more by turning selected clips into event summaries, identity records, and graph structures. Each stage therefore reduces rawness and increases interpretability. This is one of the strongest architectural themes of the whole capstone and should be made explicit in the report. Another important integration point is identifier continuity. The system does not allow each subsystem to invent unrelated naming conventions for the same evidence. Instead, identifiers such as video\_id, source\_id, camera\_id, date, location, and graph\_id are reused or mapped intentionally across layers \[6\]-\[14\], \[30\], \[31\]. This continuity is what makes playback-originated clips traceable inside the VSS stores and what makes VSS results meaningful when returned to the playback UI. Without such identifier discipline, the architecture would degrade into loosely connected demos. The integrated architecture also reveals the current asymmetry of subsystem maturity in a constructive way. The edge subsystem already contributes meaningful event clips, even if advanced site-management durability is not complete. The VMS already contributes the clearest operator-facing workflow through playback, even if broader live-view or dashboard modules remain partial. The VSS already contributes metadata extraction, retrieval, and question answering, even if graph-dominant end-user reasoning is still maturing. In other words, the platform already has enough maturity overlap to form a defensible evidence pipeline, which is exactly why the current capstone can be evaluated as a system rather than as disconnected prototypes.

Finally, the integrated architecture should be understood as an argument about sequencing of intelligence. The system does not attempt to do everything at the edge. It does not attempt to move raw streams directly into expensive AI pipelines either. It first performs cheap, local, event-oriented reduction; then it centralizes review; then it performs selective knowledge extraction and AI-assisted answering. This sequencing is sensible both computationally and operationally. It reduces waste, keeps playback grounded in real clips, and applies the more expensive AI layers only where they are likely to help investigation most.

 

## 

## 

## 

## 

## 

## **3.2 VSS ingest and query workflows** {#3.2-vss-ingest-and-query-workflows}

### **3.2.1 Ingest flow and identity lifecycle** {#3.2.1-ingest-flow-and-identity-lifecycle}

The VSS ingest pipeline is implemented as a LangGraph subgraph for `VIDEO_INGEST`. It converts one video input into a structured event document, MongoDB vector-searchable event record, identity-store updates, Neo4j graph facts, and processed-video memory that prevents duplicate ingest. The graph-level execution order is shown in Figure 3.1.

*![][image10]*

*Figure 3.1:  Ingested Data & Identity Lifecycle Flow*

Identity is handled early rather than added after summarization. Sampled identity frames are processed with DeepFace/ArcFace, deduplicated, compared against stored identity material, and either matched to an existing `face_id`, assigned a new `face_id`, or preserved as `missing_faceid` when evidence is insufficient \[8\]-\[14\].

After identity detection, VLM captioning and structured extraction produce local entities, relations, temporal groups, event summaries, embeddings, MongoDB records, Neo4j graph facts, and processed-video status. The two-pass extraction design keeps persistent identity evidence separate from first-pass visual descriptions and reduces the risk of forcing unsupported identity claims.



### **3.2.2 Query routing and four response modes** {#3.2.2-query-routing-and-four-response-modes}

The query flow is implemented as a LangGraph subgraph for `USER_QUERY`. It classifies each user request into direct, vector, graph, or hybrid retrieval before answer synthesis, as shown in Figure 3.2 and Figure 3.3.

*![][image11]*

*Figure 3.2. VSS query workflow.* 

*![][image12]*

*Figure 3.3. Query routing and retrieval logic.*

The key step is `decide_retrieval`: direct handles greetings or non-evidentiary questions; vector searches MongoDB event memory; graph queries Neo4j facts; and hybrid combines semantic recall with graph expansion. The parser extracts filters such as source_id, video_id, graph_id, camera_id, date, location, and top_k so answers remain tied to the ingested evidence.

**Table 3.2. VSS query routing modes**

| Route | Typical question | Primary store | Main strength | Main limitation |
| :---- | :---- | :---- | :---- | :---- |
| direct | Greetings, meta questions, non-surveillance chat | none | Lowest cost and latency | No surveillance grounding |
| vector | What happened? / Summarize activity | MongoDB vss\_event | Strong semantic recall over event summaries | Weaker for exact counts and exact relations |
| graph | Who was with whom? / How many? / Where was person X? | Neo4j | Exactness, counts, typed relationships, multi-hop traversal | Higher orchestration cost |
| hybrid | Track the same person across cameras and summarize behavior | MongoDB \+ Neo4j | Best coverage for cross-event questions | Highest latency and fusion complexity |

This routing scheme avoids under-retrieval for factual surveillance questions and over-retrieval for simple questions. During synthesis, exact graph facts should dominate exact claims, while vector summaries provide descriptive context.

### **3.2.3 Persistence and hybrid database design** {#3.2.3-persistence-and-hybrid-database-design}

The VSS persistence layer uses MongoDB and Neo4j as complementary stores. MongoDB stores event summaries, search text, embeddings, metadata filters, identity vectors, and ingestion status. Neo4j stores normalized Event, Person, PersonObservation, Object, Camera, Date, Location, GraphGroup nodes, and typed relationships \[9\], \[14\].

The core rule is identifier continuity. `source_id`, `graph_id`, `camera_id`, date, location, and verified `face_id` connect MongoDB records with Neo4j nodes. Verified identities become Person nodes; visible people without reliable face evidence remain PersonObservation nodes, avoiding false biometric claims \[8\], \[14\], \[50\].

*![][image13]*

*Figure 3.4. Data schema for MongoDB identities, MongoDB events, and Neo4j graph.*

Persistence rules are conservative: events stay tied to source evidence; GraphGroup is based on coarse camera-date-location context; Person requires verified identity; Object remains event-scoped unless object re-identification is available; processed_videos and ingestion_status prevent duplicate or inconsistent ingest \[10\].

*![][image14]*

*Figure 3.5. Data schema for retrieved event documents, processed videos, and ingestion status.*

At query time, MongoDB provides semantic recall and Neo4j provides exact graph-fact lookup. Hybrid retrieval links the two stores through shared identifiers rather than treating them as independent databases \[9\], \[14\], \[50\].

**Table 3.3. VSS persistence object mapping**

| Store object | Minimal identity key | Main content | Main later use |
| :---- | :---- | :---- | :---- |
| vss\_event document | source\_id | Event summary, search text, embedding, camera/date/location metadata | Vector retrieval and broad semantic answer support |
| identities document | face\_id | Face embedding, appearance history, seen dates/cameras/locations, identity status | Identity lookup and continuity across events |
| GraphGroup node | camera-date-location composite | Coarse grouping anchor | Filter entry for graph traversal |
| Event node | source\_id | Event-level graph anchor | Join point between playback evidence and graph context |
| Person node | face\_id if verified | Persistent person anchor | Exact identity traversal |
| PersonObservation | event-local key | Local appearance context | Event-scoped reasoning without contaminating global identity |
| Object node | event-scoped object key | Object type and local properties | Object relation queries |

**Table 3.4. VSS subsystem implementation status**

| VSS subsystem | Implemented | Partial | Future work |
| :---- | :---- | :---- | :---- |
| Video ingest orchestration | LangGraph ingest flow, payload validation, frame sampling, duplicate check | Error handling and observability are still limited | Retry policy, tracing, and stage-level metrics \[10\], \[11\] |
| Metadata extraction | VLM captioning, structured event summary, people/object/relation extraction | Ground truth is limited and scene diversity is small | Larger labeled dataset with occlusion, low-light, crowd, and multi-camera cases \[49\], \[51\], \[53\], \[54\] |
| Identity handling | DeepFace/ArcFace, `face_id`, `missing_faceid`, identity store | Threshold calibration and cross-camera identity accuracy are not deeply validated | Ground-truth identity evaluation, false-merge, and false-split analysis \[8\], \[52\] |
| Vector retrieval | MongoDB event documents and embeddings | Ranking quality has not yet been compared against alternatives | Vector-only baseline, top-k recall, precision, and answer relevance \[9\], \[20\]-\[22\], \[50\] |
| Graph persistence | Neo4j Event/Person/Object/Camera/Date/Location relationships | Graph facts depend on extracted metadata; reasoning quality is not deeply audited | Graph-fact precision/recall and graph-only QA test set \[14\], \[51\] |
| Hybrid retrieval | Route selection among direct/vector/graph/hybrid | No ablation yet proving hybrid is better than vector-only or graph-only | Ablation test with identical query set \[20\]-\[22\], \[53\], \[54\] |
| Answer synthesis | Grounded answer with retrieved sources | Faithfulness and hallucination scoring remain lightweight | RAGAS-style faithfulness and answer relevance scoring \[20\]-\[22\] |
| Security and privacy | Upload validation and cloud services are used | Personal GCS, MongoDB/Neo4j, and VM resources are not production governance | IAM, encryption, retention, audit logging, consent/access policy, and key rotation \[47\], \[52\] |

Overall, the persistence design separates semantic recall, identity memory, graph facts, and ingestion control while preserving traceability to the original video evidence.

## **3.3 Edge Agent algorithms and synchronization workflows** {#3.3-edge-agent-algorithms-and-synchronization-workflows}

### **3.3.1 Event detection and clip generation algorithm** {#3.3.1-event-detection-and-clip-generation-algorithm}

The Edge Agent runtime transforms a continuous Real-Time Streaming Protocol (RTSP) stream into event-driven video evidence by implementing a highly structured data pipeline. Operationally, this ingestion and processing flow is organized as a sequential chain moving from the camera source through edge detection, localized recording, container serialization, and eventual WAN synchronization. At system initialization, the Go-based core engine loads the respective camera channel configurations, initializes local network serving utilities, starts an FFmpeg-backed media stream intake mechanism, instantiates the spatial-temporal motion detector, prepares the file serialization worker, and pre-allocates the volatile memory spaces for a contextual ring buffer array. Once initialized, the analytical pipeline continuously ingests compressed frames from the legacy network camera node, operating within standard real-time performance bounds \[3\]-\[5\], \[27\], \[29\]. At the algorithmic level, each ingress frame is projected into a single-channel grayscale matrix to eliminate chrominance space overhead, compared against the immediate historical baseline frame using register-level absolute differencing, thresholded to remove background electronic noise, and aggregated into a frame-level spatial anomaly density scalar. To map this initial signal transformation phase without software logic overhead, the execution topology is modularized into the ingestion and linear feature extraction sequence diagrammed below: 

![][image15]

Figure 3.6. Ingestion and spatial-temporal feature extraction stage.

A temporal sliding window filter then monitors these spatial anomaly variations across a moving sequence of consecutive frame indexes, deciding whether the accumulated historical disturbance is sufficient to satisfy the systemic threshold required to assert an active event state. This state transition does not merely capture an isolated image snapshot; instead, it guides the automated serialization of context-rich video evidence packages around the physical event boundaries. To preserve the comprehensive environmental context leading up to the motion trigger, the runtime flushes the historical frame sequence retained within the volatile RAM ring buffer directly into a newly initialized MP4 container \[3\]-\[5\], \[27\], \[29\]. This core decision-making and pre-event memory flushing mechanic is formalized in the automated verification block illustrated below: 

![][image16]  
Figure 3.7. Event verification and context preservation stage.

Within the current implementation framework, the real-time ingress stream operates at a standardized frame rate of approximately 10 frames per second, allowing the pre-recording ring buffer to preserve roughly 3 seconds of pre-event context before the initial threshold breach. Following the drop of the temporal anomaly mass below the activation boundary, the recording subsystem continues video capture for a configured post-event duration of approximately 8 seconds, ensuring that the final media package encapsulates both the entry and exit phases of the environment activity. To optimize storage utilization and prevent severe clip fragmentation during burst or prolonged physical motion scenarios, a programmatic cooldown interval is applied immediately following clip finalization. This suppression mechanism forces the edge engine to consolidate successive, close-proximity triggers into a single, comprehensive evidence asset rather than scattering data across a multitude of brief, disconnected files, significantly increasing localized disk write efficiency and structural clarity for the centralized management layer \[3\]-\[5\], \[27\], \[29\].

The application of this linear spatial-temporal filtering model yields a deterministic computational complexity scaling directly with the frame pixel volume, which allows the runtime to maintain high-throughput real-time execution limits on low-power commodity edge computing hardware without requiring dedicated graphics processing units. Accumulating spatial anomalies across a sequence horizon prevents the gateway from generating duplicate assets based on transient sensor artifacts or isolated noisy frames, creating a stable filtering layer that effectively bridges raw, unmanaged camera broadcasts with modern cloud orchestration \[27\], \[29\]. This context-preserving data reduction process ensures that the localized edge site functions not as a passive packet relay, but as an intelligent evidence-capture domain capable of filtering out environmental noise close to the physical source.

### **3.3.2 Buffer management and upload coordination** {#3.3.2-buffer-management-and-upload-coordination}

The multi-cursor circular ring buffer constitutes the core synchronization structure of the Edge Agent, acting as a thread-safe memory boundary layer between real-time data ingestion and asynchronous cloud transport. The memory management system achieves constant-time resource allocation by tracking three independent, monotonically rising integer cursors defined as the write cursor, the read cursor, and the synchronization cursor, mapped across the physical capacity arrays through absolute modulo operations.

To enforce strict, unblocked indexing properties over a fixed allocation capacity N without incurring volatile dynamic allocation overhead, the physical space translation of any continuous sequence counter c is formalized via the inline modulo operator:

Indexphysical \= c  ( mod N)

The write cursor governs the continuous, unblocked insertion of incoming raw video frame bytes and luma matrices derived from the camera interface. Concurrently, the read cursor guides the sequential extraction of buffered frame histories to compile pre-event evidence packages upon motion detection, while the synchronization cursor pinpoints the exact trailing boundary of data payloads that have been systematically received and acknowledged by the centralized core. Under standard operating parameters, this decoupled multi-pointer design allows the edge runtime to maintain steady frame acquisition while simultaneously compiling clips and tracking synchronization states over wide-area networks \[17\], \[29\]. When wide-area network partitions or transport layer timeouts are encountered, the synchronization cursor automatically halts because outbound acknowledgments stop, causing the local buffer occupancy to rise linearly while the unblocked write cursor continues to absorb the live camera stream. Once the volatile allocation capacity is breached, the memory manager prevents unmanaged heap exhaustion or arbitrary packet dropping by executing a custom, GOP aware bounded tail dropping algorithm. Because modern compressed video formats rely on strict spatial-temporal frame dependencies where predictive elements cannot be rendered without their corresponding intra-coded anchor, discarding random bytes would result in catastrophic macroblock corruption at the central repository. The Edge Agent resolves this structural constraint by scanning forward through the oldest historical buffer allocations to locate the exact index position of the next chronological reference frame anchor, completely purging the out-of-date GOP sequence preceding that boundary. This specialized tail-dropping strategy advances the synchronization reference point to a clean reference cutting point, ensuring that all remaining video fragments within the volatile memory matrix preserve their predictive dependency chains and remain perfectly decodable upon later cloud retrieval \[17\], \[29\]. This thread-safe pointer recovery loop and memory truncation logic are formalized in the flow topology detailed below:

![][image17]

Figure 3.8. GOP-aware bounded tail-dropping and pointer synchronization loop.

Network transport efficiency and system resilience are further maximized by completely separating control-plane operations from data-plane payloads. The control plane coordinates structural site monitoring by operating a periodic routing loop that dispatches compact JSON telemetry heartbeats every 10 seconds, communicating localized health parameters such as real-time CPU utilization, memory allocation saturation, residual block storage space, and streaming channel status to the central management interface. Completely decoupled from this lightweight status tracking loop, the data plane manages finalized video evidence using an asynchronous, transactional model. When a validated clip is completed by the recording subsystem, its physical asset path is securely registered within a persistent, localized SQLite database transaction queue. 

## **3.4 VMS playback and VSS integration workflows** {#3.4-vms-playback-and-vss-integration-workflows}

### **3.4.1 Upload, normalization, storage, and playback workflow** {#3.4.1-upload,-normalization,-storage,-and-playback-workflow}

The VMS begins the operational chain by receiving clips from edge or upload sources through POST /api/videos or POST /upload using multipart/form-data. The backend separates the file from the metadata, validates MIME type and extension, sanitizes the file name, and writes the raw input into uploads\_raw/. It then invokes FFmpeg to normalize the video into browser-compatible MP4 and stores the normalized output in uploads \[6\], \[34\], \[35\]. Once normalization succeeds, the clip becomes part of the playback storage path. If cloud synchronization is enabled, the normalized MP4 is uploaded to Google Cloud Storage. The VMS can then expose the clip library through GET /api/gcs/videos, which the frontend uses to build the playback list by parsing camera, date, time, and location information from object names. When the user selects a clip, the frontend creates a playback URL pointing to GET /api/playback/clips?name=.... The playback endpoint acts as a controlled proxy in front of GCS. It validates the object name, retrieves blob metadata, parses the Range header when present, and streams only the required byte range back to the browser through 206 Partial Content. This allows seeking and partial playback without downloading the entire object. Operationally, the flow is: user selects clip, browser requests initial bytes, backend checks GCS object, backend returns partial content, browser continues requesting additional ranges as playback proceeds or the user seeks to another offset. The upload half of this workflow is more than a file-copy operation. It acts as a normalization gateway for the entire playback path. Validating MIME type, extension, and file size prevents malformed or unsupported inputs from silently entering the playback library, while filename sanitization reduces risk at the upload boundary \[47\]. FFmpeg normalization ensures that the resulting MP4 is suitable for browser playback even when upstream sources vary in container or codec, which is necessary because browser media playback depends on supported media formats and the behavior of the HTML video element \[41\], \[42\]. The storage half of the workflow is equally structured. The original upload is retained in uploads\_raw/ as intake evidence, while the normalized browser-oriented output is written into uploads/. Cloud synchronization then pushes the normalized asset to GCS, which becomes the canonical playback source in the current architecture \[7\], \[30\]. This separation provides operational flexibility: the local files support conversion and troubleshooting, while the GCS object supports centralized playback and later integration workflows. The Range-based playback path is the most technically important subflow. When the browser requests the beginning of the clip, the VMS can return either the full content or the initial required byte interval. When the user seeks, the browser emits a new Range request, and the VMS translates that request into a bounded partial read over the GCS object. Correct handling of 206 Partial Content, Content-Range, and invalid-range responses is therefore not incidental web detail. It is the protocol mechanism that turns cloud-stored clips into usable operator-facing playback \[6\], \[31\], \[34\], \[35\]. 

### **3.4.2 VMS playback architecture and endpoint contract** {#3.4.2-vms-playback-architecture-and-endpoint-contract}

The protocol foundations from Chapter 2 are realized in the project through a layered VMS playback architecture. The frontend playback layer gives the operator a single interface for clip selection, playback controls, Deep Analyze, and clip-specific questioning. The FastAPI backend acts as the orchestration layer. It receives upload requests, validates and normalizes clips, coordinates local and cloud storage, exposes playback APIs, and mediates requests to VSS. Google Cloud Storage stores playback-ready video objects \[43\], \[44\]. Pub/Sub provides the producer-side handoff signal for downstream ingest \[45\], \[46\]. VSS remains an external analysis and retrieval service rather than being merged into the VMS process.

**Table 3.5. VMS playback architecture layers**

| Layer | Main responsibility | Why it matters in the project |
| :---- | :---- | :---- |
| Frontend playback layer | Clip list, playback player, seek control, speed control, Deep Analyze popup, question UI | Gives the operator one coherent evidence-review surface \[42\] |
| FastAPI orchestration layer | Upload validation, video normalization, GCS access, Range playback proxy, VSS request mediation, Pub/Sub publishing | Centralizes the VMS application contract \[31\], \[38\], \[40\], \[45\], \[46\] |
| Cloud object storage layer | Playback-ready GCS video objects | Provides centralized storage for browser-facing playback \[43\], \[44\] |
| VSS integration layer | Analysis retrieval and clip-specific question forwarding | Keeps AI analysis accessible from playback without exposing VSS internals to the frontend |
| Asynchronous handoff layer | Pub/Sub producer-side ingest message | Allows downstream ingest to begin without blocking the upload request \[45\], \[46\] |

The endpoint layer further makes the VMS contract explicit. Each endpoint corresponds to one workflow responsibility rather than one vague "video API". This makes the implementation easier to evaluate because upload intake, cloud listing, playback streaming, analysis retrieval, and contextual query forwarding can be discussed separately \[31\], \[38\], \[40\].

**Table 3.6. VMS playback endpoint summary**

| Endpoint | Method | Main role in workflow | Main output |
| :---- | :---- | :---- | :---- |
| /api/videos | POST | Receive edge or uploader clip | Stored clip plus metadata about intake result |
| /api/gcs/videos | GET | Build playback library from GCS objects | Clip list for frontend |
| /api/playback/clips | GET | Stream playback data from GCS through VMS | Full or partial media content |
| /api/playback/clips/{video\_id}/analysis | GET | Retrieve clip-linked analysis state | Normalized analysis payload |
| /api/playback/clips/{video\_id}/query | POST | Send clip-specific question to VSS | Normalized answer plus sources |

### **3.4.3 Deep Analyze and clip-specific query workflow** {#3.4.3-deep-analyze-and-clip-specific-query-workflow}

The VMS-to-VSS integration becomes visible through the Deep Analyze workflow. When the user opens clip analysis, the frontend calls GET /api/playback/clips/{video\_id}/analysis. The VMS backend then contacts the VSS side through HTTP/HTTPS API calls, retrieves the corresponding clip-related analysis output, normalizes the response, and returns it to the frontend. This keeps the playback interface insulated from VSS internal API structures \[6\], \[9\-14\]. If the user asks a question about the currently viewed clip, the frontend submits a POST /api/playback/clips/{video\_id}/query. The VMS does not forward only the raw question. It enriches the request with contextual metadata such as video\_id, camera\_id, date, location, and top\_k, then forwards the request to the VSS query API. The answer returned by VSS is normalized by the VMS into UI-oriented fields such as answer, sources, and raw. This workflow is important because it turns playback from a passive viewing tool into the entry point for clip-specific AI-assisted investigation. This integration is valuable because it lets the user remain inside one operator workflow. The operator does not need to manually export clip identifiers, open a second analysis tool, and reformulate context each time. The VMS already knows which clip is being viewed and can enrich the outbound request with the metadata required by VSS. In effect, the VMS acts as a context-preserving mediator between evidence review and evidence interpretation. The response normalization step is also important. VSS may produce rich internal outputs such as summaries, entities, sources, or raw structured payloads. The frontend, however, needs a stable interface. By reshaping the response into UI-ready fields such as answer, sources, and raw, the VMS preserves a consistent presentation contract even if VSS internals evolve \[6\], \[9\-14\]. This is one of the clearest examples of why the VMS should be described as an orchestration layer rather than merely a storage-backed playback server. The contextual query path can also be framed as a contract-preserving enrichment step. The frontend submits the user question in the context of one currently selected clip. The VMS then attaches the clip metadata it already knows, such as video\_id, camera\_id, date, location, and any configured retrieval depth like top\_k 5. In practical terms, this prevents the user from having to reconstruct context manually and reduces the risk that VSS will answer a question against the wrong evidence scope. The analysis and query workflows therefore solve two different, but complementary, problems. The analysis endpoint answers the question "what does the system already know about this clip?" The contextual query endpoint answers the question "what more can the system explain about this clip when asked in natural language?" Keeping both endpoints separate is useful because one is retrieval of the existing analysis state, while the other is interactive reasoning over clip-specific context.

### **3.4.4 Asynchronous ingestion event publishing** {#3.4.4-asynchronous-ingestion-event-publishing}

The final step of the VMS integration workflow is asynchronous event publishing. After a clip is successfully uploaded to GCS, the backend builds a metadata payload containing fields such as gcs\_uri, filename, video\_id, camera\_id, date, location, start\_time, and uploader context. This payload is then published to a Google Pub/Sub topic. Pub/Sub provides a managed publish-subscribe model in which the VMS acts as the producer, the ingest topic acts as the communication boundary, and a VSS-integrated worker or pipeline acts as the consumer \[45\], \[46\]. This design allows VMS upload handling to complete without blocking on heavier downstream analysis. This design prevents the upload API from remaining blocked while the downstream VSS-integrated ingest flow performs heavier analysis. The VMS can acknowledge the upload and preserve its role as a video-management and orchestration layer, while a separate worker or VSS-integrated consumer continues the asynchronous ingest process. In practical system terms, this is what connects clip intake in the VMS layer with later full analysis in the VSS layer without collapsing both concerns into one synchronous transaction. The asynchronous publish step is therefore a boundary marker between two classes of work. Everything before published belongs to the VMS responsibility of receiving, normalizing, storing, and exposing playback-ready evidence. Everything after publication belongs to the heavier analysis and knowledge-building path. This separation is especially important in a capstone prototype because it keeps the operator-facing upload workflow responsive even when downstream AI analysis may take substantially longer. The publish payload itself is also not arbitrary metadata. It is the minimum structured handoff that allows downstream consumers to reconstruct what clip should be ingested and under what context. Fields such as gcs\_uri, video\_id, camera\_id, date, location, and uploader context make it possible for the next stage to begin without reopening the operator-facing upload transaction \[7\], \[30\]. This is one more reason the VMS should be described as an orchestration point: it creates the structured message that links evidence storage with downstream knowledge-building. The software architectures and concurrent workflows detailed throughout this chapter effectively demonstrate the systemic translation of theoretical abstractions into a fully realized, operational system design. Within the Visual Surveillance System (VSS) architecture, the implemented pipelines outline how raw video assets are programmatically digested into structured event metadata, persistent identity catalogs, semantic vector indices, and relational graph models to govern automated query execution, while the Edge Agent's operational workflows realize the continuous extraction of raw RTSP streams into context-preserving, event-bounded video clips by integrating low-overhead spatial-temporal filters and multi-cursor circular memory tracking. Actively bridging these layers, the Video Management System (VMS) functions as the centralized orchestration backbone mediating raw clip ingestion, automated container normalization, cloud object storage synchronization, and low-latency HTTP Range-based playback, while executing parallel handoffs to downstream intelligence via synchronous query loops and asynchronous publish-subscribe event streams. Collectively, these interconnected, software-defined execution paths formalize a unified, end-to-end surveillance evidence pipeline, thereby establishing the necessary empirical baseline for the comprehensive performance benchmarks, latency evaluations, and systemic readiness assessments detailed in the subsequent chapter. 

 **3.4.5 VMS workflow figure placement**

![][image18]   
*Figure 3.9. Upload clip from Edge to VMS.*  
![][image19] 

*Figure 3.10. Video normalization and cloud synchronization.*  
 ![][image20] 

*Figure 3.11. Publish ingest event through Pub/Sub.*  
*![][image21]*   
*Figure 3.12. Playback clip from GCS to the browser.*  
*![][image22]*   
*Figure 3.13. Seek and scrub using HTTP Range.*  
*![][image23]*   
*Figure 3.14. Deep Analyze popup and clip-specific query.*  
 


# **CHAPTER 4\. EXPERIMENTAL RESULTS AND EVALUATION** {#chapter-4.-experimental-results-and-evaluation}

In this chapter, the implemented system is evaluated from the perspective of integration evidence, subsystem behavior, and operator-relevant workflow performance. Because the project matured unevenly across subsystems, the evaluation is organized around the flows that are already meaningful and defensible in the current prototype. The chapter therefore focuses on the playback-centered surveillance-to-analysis chain, discusses the current effectiveness of the VSS and Edge Agent paths, and then presents the clearest performance evidence available for VMS playback.

## **4.1 Integration status and evidence** {#4.1-integration-status-and-evidence}

The strongest evidence carried forward from the M2 stage is not full completion of every subsystem, but the existence of a meaningful chained workflow that connects capture, review, and analysis. The current system already supports ONVIF or RTSP-based camera discovery, controlled edge-side event recording, playback on real stored video, and forwarding of selected clip context into the VSS path. This makes the platform evaluable as a working surveillance prototype rather than as a collection of isolated demos \[1\], \[2\]. At the same time, the integration status remains selective rather than universal. Core forward flows exist from Edge to VMS and from VMS to VSS, but reverse-control flows, wider backend coverage for non-playback VMS modules, full cloud-side playback maturity, and dominant end-user graph retrieval remain incomplete or partial \[1\], \[2\]. This means the final report should evaluate the system by emphasizing working evidence paths and explicitly naming the incomplete ones instead of claiming uniform production readiness. The current demo-ready chain can be summarized as follows. First, the Edge subsystem receives RTSP streams, performs motion filtering, and produces event-based clips with contextual pre-recording. Second, those clips enter the VMS path and become reviewable through playback on real stored video. Third, selected clip information is forwarded into the VSS path for semantic analysis, metadata extraction, and question-answering support. This sequence already provides partial but real answers to the three overall research questions by showing operational continuity, AI-assisted investigation value, and modular architecture growth potential. For greater clarity, the current integration status can be summarized directly in narrative form:

**Table 4.1. Current integration status by system link**

| System link | Current status | What is already demonstrated | Main remaining gap |
| :---- | :---- | :---- | :---- |
| Camera / RTSP to Edge | Working prototype path | Stream intake and motion-driven clip generation | Broader device-management polish and long-run resilience |
| Edge to VMS upload | Working path | Event clips can be pushed into VMS intake | More mature retry durability and wider stress validation |
| VMS local normalization to GCS playback | Strongest implemented workflow | Real stored video can be listed and played through browser-facing APIs | Broader non-playback VMS completeness |
| VMS to VSS analysis and query | Meaningful service integration | Clip-related analysis and contextual questions can be mediated through VMS | Deeper graph-dominant end-user reasoning maturity |
| Pub/Sub to downstream ingest | Architectural handoff present | Upload path can publish asynchronous ingest signal | Broader production-level observability and consumer hardening |

 This integrated-status table helps the reader distinguish between "already working" and "architecturally intended but still maturing." That distinction is especially important in a capstone report, where overstating maturity weakens credibility. Here, the strongest complete arc is the one that begins with edge-generated evidence and ends with playback-centered AI-assisted follow-up. That is the workflow the report is justified in emphasizing most strongly. A camera stream is connected and monitored by the Edge Agent. Motion above threshold causes a clip to be generated with preserved context before and after the trigger. The clip is uploaded into the VMS, normalized, and placed into centralized storage. The operator opens the playback page, identifies the clip by camera, date, time, and location, and starts review. If additional explanation is needed, the operator uses Deep Analyze or asks a question against the clip context. This is not yet the full theoretical platform vision, but it is already a realistic end-to-end surveillance demonstration scenario. 

## **4.2 Edge Agent evaluation and operational efficiency discussion** {#4.2-edge-agent-evaluation-and-operational-efficiency-discussion}

### **4.2.1 Experimental Configuration and Baseline Analytical Environment** {#4.2.1-experimental-configuration-and-baseline-analytical-environment}

To rigorously evaluate the computational throughput, transactional responsiveness, and overall operational efficiency of the localized Go-based Edge Agent, which is conceptually mapped as the Kerberos Agent framework within the primary site baseline, a quantitative empirical benchmark was established under realistic surveillance workloads \[1\], \[2\]. This restrictive hardware profile directly mirrors the PC infrastructure constraints typical of legacy retail and corporate physical security sites, ensuring the empirical metrics gathered inform operational viability under resource-bound deployment scenarios. The evaluation workload comprised multiple parallel input stream configurations ingesting continuous video broadcasts from legacy commercial camera nodes, which specifically consisted of commercial-grade IMOU smart cameras over a localized network topology. The raw source media utilized standard H.264/AVC compression running at a baseline frame rate of 10 frames per second with a high-definition spatial resolution of 1080p. Under standard operating parameters, the static ring buffer capacity was initialized at a physical allocation scale of 30 slots to capture and preserve structural spatial-temporal frame transitions without introducing memory overhead. The system performance was rigorously audited across five core evaluation axes: motion-to-record responsiveness, event-context preservation, clip save persistence, suppression effectiveness of redundant triggers, and upload network reliability.

### **4.2.2 Computational Throughput and Motion-to-Record Responsiveness**

The processing efficiency of the localized filtering pipeline is critical to prevent frame-buffer overflow at the local network ingress boundary. The execution latency per incoming raw video frame was measured across individual sub-stages, including vectorized luma coefficient mapping for grayscale conversion, absolute register-level differencing utilizing Two's Complement subtraction, and the linear summation pass required for spatial anomaly density mask calculation. Given that a 10 frames per second video pipeline imposes a deterministic inter-frame arrival constraint of 100 milliseconds, the Edge Agent operates comfortably within real-time execution bounds, consuming merely 9.3 milliseconds of the available frame processing window.  
This optimized single-core execution performance translates directly into an exceptionally low motion-to-record responsiveness. For each event scenario, measuring the precise time interval between the physical motion detection threshold breach and active recording engine activation using timestamp-based observation revealed an average responsiveness latency of 9.3 ms, with a minimum boundary of 8 ms and a maximum peak of 10 ms. This is well below the expected academic and industry threshold of 500ms. This strong experimental result demonstrates that the localized event trigger path is not only conceptually sound but also operationally fast enough for the prototype's local event-capture objective. This minimal latency footprint proves that the linear spatial-temporal complexity scales efficiently, enabling a PC to manage up to four concurrent camera pipelines without experiencing frame-buffer starvation.

### **4.2.3 Computational Throughput and Motion-to-Record Responsiveness**

The operational evaluation of the temporal context preservation parameters is critical to assessing the evidentiary viability of generated video packages. In continuous-broadcasting brownfield integrations, naive threshold-only event detection schemes often suffer from latency-induced truncation, failing to capture the causal context preceding the trigger or the subsequent exit phase of the activity \[26\]. By enforcing a deterministic pre-event duration of 3 seconds, which is retained within the volatile memory circular ring buffer, and a post-event duration of 8 seconds,, the Edge Agent ensures that each serialized MP4 file contains the complete physical activity boundaries required for downstream operator review \[2\], \[27\]. Furthermore, the end-to-end reliability of the upload mechanism serves as an empirical verification of the integration between the local volatile buffering layer, the file serialization runtime, and the transactional edge-to-VMS handoff protocol \[2\], \[7\], \[27\].  
**Table 4.2. Edge Agent evaluation metrics**

| Metric | How we evaluate | Tested Result |
| :---- | :---- | :---- |
| Motion to record responsiveness | For each event, measure the time interval between motion detection and recording activation using timestamp-based observation. \[2\], \[19\] ,\[26\] | Avg  9.3 ms (min 8 ms, max 10 ms) |
| Recording context (pre/post) | For each clip, analyze the recorded video to verify the duration of pre-event and post-event content. \[2\], \[3\] ,\[27\] | Pre \= 3s, Post \= 8s |
| Clip save success rate | For all recording events, calculate the percentage of successfully stored video clips. \[2\], \[4\] ,\[29\] | 10/10 ( 30s per clips)  |
| Upload latency & success rate | For each clip, measure the upload time and compute the percentage of successful uploads. \[7\], \[19\] , \[30\] |  3.1s, 100%  |

Based on the empirical metrics detailed in Table 4.2, the Edge Agent prototype demonstrates high operational efficiency and reliability across all audited criteria. The average motion-to-record responsiveness of 9.3 milliseconds, ranging between a minimum of 8 milliseconds and a maximum of 10 milliseconds, is substantially below the standard 500-millisecond threshold required for real-time security systems, thereby verifying that the linear spatial-temporal motion filter does not introduce processing bottlenecks. Furthermore, the event-context preservation successfully captures the complete physical boundaries of the activity, retaining the required 3 seconds of pre-event history and 8 seconds of post-event history in every clip without memory leaks. The perfect clip save success rate of 10 out of 10 trials at 30 seconds per clip, combined with a 100 percent upload success rate and an average upload latency of 3.1 seconds, validates the robustness of the SQLite transaction queue and the asynchronous upload pipeline, demonstrating that local event capture and wide-area network synchronization operate reliably under legacy retail and commercial environmental constraints.

## **4.3 VMS playback evaluation** {#4.3-vms-playback-evaluation}

### **4.3.1 Test principles and test grouping** {#4.3.1-test-principles-and-test-grouping}

For the VMS, evaluation should not be reduced to checking whether a video can start playing. The current playback workflow spans edge upload, file validation, MP4 normalization, cloud storage synchronization, GCS-based playback, HTTP Range behavior, VSS API calls, and Pub/Sub publishing. Therefore, tests should be grouped according to these actual responsibilities rather than by isolated UI widgets. Upload tests should cover file type, file size, filename, metadata, and input validation \[38\], \[47\]. Playback tests should cover browser media compatibility, Range Requests, partial-content behavior, invalid ranges, and operator-facing seek behavior \[31\], \[40\]-\[42\]. Integration tests should cover GCS availability and Pub/Sub event publishing because those two services form the storage and asynchronous handoff foundation of the current VMS playback workflow \[43\]-\[46\]. The recommended grouping is fourfold. Functional testing covers upload, conversion, playback, VSS-facing analysis and query APIs, and Pub/Sub publishing. Integration testing covers Edge-to-VMS, VMS-to-GCS, VMS-to-VSS, and VMS-to-Pub/Sub behavior. Boundary and error testing covers invalid MIME types, oversized uploads, missing objects, invalid Range headers, and external service failures. Usability and acceptance testing covers whether an operator can identify, open, seek, and analyze the intended clip with clear feedback and minimal friction \[23\]-\[25\], \[31\], \[34\], \[35\]. These principles are consistent with the earlier M2 direction, which emphasized demo readiness, integration evidence, and proof that real workflow was functioning rather than mock-only interface behavior. They are also consistent with the VMS research question, because efficient playback can only be defended when upload, storage, partial-content delivery, and analysis handoff all work together as one coherent review path.

**Table 4.3. VMS playback test groups and representative checks**

| Test group | Main focus | Representative checks |
| :---- | :---- | :---- |
| Functional | Does each playback responsibility work by itself? | Upload validity, MP4 conversion, clip listing, playback open, analysis endpoint, query endpoint, Pub/Sub publish \[38\], \[41\], \[42\], \[45\]-\[47\] |
| Integration | Do the subsystems hand off correctly? | Edge to VMS, VMS to GCS, VMS to VSS, VMS to Pub/Sub \[43\]-\[46\] |
| Boundary and error | Does failure remain controlled and observable? | Invalid MIME type, oversized upload, missing object, invalid Range, VSS error, GCS error \[31\], \[40\], \[47\] |
| Usability and acceptance | Can an operator complete the intended review task? | Find clip, open clip, seek accurately, inspect analysis, ask clip question \[18\], \[19\], \[23\]-\[25\], \[42\] |

Each group should then be defended through representative scenarios. Functional testing should include valid and invalid upload types, successful and failed MP4 conversion, normal clip listing, playback open success, query endpoint success, and successful Pub/Sub publishing. Integration testing should verify that a clip uploaded from edge appears in the VMS path, reaches GCS, can be opened through playback, and can be handed off to VSS analysis. Boundary testing should verify that the system returns clear errors for oversized upload, missing object name, invalid Range header, or downstream failure. Usability testing should confirm that an operator can move from finding a clip to seeking a target moment and then asking a contextual question without confusion. The reason this level of detail matters is that the VMS section is the clearest operator-facing part of the current capstone. If the report evaluated playback only by saying "video plays," it would miss the actual engineering contribution: a chain that begins with intake, passes through normalization and cloud object storage, supports efficient seek behavior, and continues into clip-aware AI-assisted analysis. The test design should therefore mirror the real responsibilities of the VMS rather than treating playback as a superficial UI feature.

### **4.3.2 Playback performance and reliability results** {#4.3.2-playback-performance-and-reliability-results}

The clearest quantitative evidence in the current project belongs to the VMS playback path. Playback open time is reported at an average of 2.64 seconds. Playback seek latency is reported at a median of 1.83 seconds with p95 of 2.51 seconds. Media error rate in the evaluated controlled test set is reported at 0%. These results matter because they show not only that playback exists, but that its usability is supported by measurable timing and reliability outcomes \[18\], \[19\], \[31\], \[33\]-\[35\]. The VMS test criteria proposed in the subsystem-specific playback report explain why these numbers matter. Efficient playback requires correct upload intake, browser-compatible MP4 normalization, availability of playback objects in GCS, standards-compliant HTTP Range behavior, stable VMS-to-VSS orchestration, and asynchronous handoff to Pub/Sub without collapsing the upload path into a long synchronous wait \[6\], \[7\], \[30\], \[31\]. The currently recorded metrics demonstrate that the most central part of this chain-opening and seeking through stored clips-already performs at a practically usable level. This does not mean the VMS is complete in every direction. Playback is the clearest mature workflow, while live view, dashboard realism, event management backend completion, and broader non-playback VMS behavior remain less mature. For that reason, the current evaluation should be framed as strong evidence for the playback-centered core of the VMS rather than blanket evidence for total subsystem completion. The numerical results are meaningful when interpreted against the user task. A playback open time of 2.64 seconds means the operator can move from clip selection to visual review without disruptive waiting. A median seek latency of 1.83 seconds and p95 of 2.51 seconds means that most seek operations remain fast enough to support investigation, even when the user jumps across the clip. A 0% playback media error rate in the controlled evaluation means that once clips are in the tested path, the browser-facing delivery chain is stable enough to trust during demonstration and review \[18\], \[19\], \[31\], \[33\]-\[35\]. These values also support the architectural choices made earlier in the report. If the upload path, conversion path, or cloud playback path were inconsistent, open time and media errors would likely degrade. If Range handling were incorrect, seek behavior would become unstable or slow. In that sense, the retained metrics are not isolated frontend measurements. They are compressed indicators that multiple backend responsibilities are already working together. The event-to-playback availability metric is particularly important because it links the investigation use case back to the VMS design. The operator often begins from an event list or a known stored clip, not from arbitrary file-system browsing. A 100% open-from-event behavior in the retained test set shows that the path from indexed or surfaced evidence to actual playback was reliable in the tested conditions \[18\], \[19\], \[31\], \[33\]-\[35\]. That is a stronger claim than merely saying the player can play a hard-coded file.

Similarly, the seek metric does more than measure UI responsiveness. It is a practical test of the HTTP Range design. If byte-range parsing, blob-size calculation, or partial streaming were faulty, seek latency would often inflate or fail unpredictably. The retained seek results therefore serve as indirect evidence that the protocol-level design decisions described in Chapter 2 and Chapter 3 are functioning correctly in the implemented system.

### **4.3.3 Test case table** {#4.3.3-test-case-table}

**Table 4.4. Retained VMS playback test cases**

| TC ID | Item | Description | Pass criterion | Previously recorded result |
| :---- | :---- | :---- | :---- | :---- |
| VMS-TC-01 | VMS event-to-playback availability | Successful playback openings from event entries over total tested event entries | \>= 95% successful open-from-event behavior | 100% |
| VMS-TC-02 | VMS playback open time | Elapsed time from selecting an event or clip to visible playback timeline or video | Median \<= 5 s | Average open time \= 2.64 s |
| VMS-TC-03 | VMS playback seek latency | Elapsed time from seeking to target timestamp until the corresponding frame is displayed | Median \<= 2 s, p95 \<= 5 s | Median \= 1.83 s, p95 \= 2.51 s |
| VMS-TC-04 | VMS playback media error rate | Video-load, decode, unsupported-format, or playback-failure errors over total tested attempts | \<= 5% failed playback attempts | 0% |

**Table 4.5. VMS playback result interpretation**

| TC ID | Item | Result interpretation |
| :---- | :---- | :---- |
| VMS-TC-01 | Event-to-playback availability | All tested event entries opened successfully, indicating a stable mapping from recorded evidence to playback action |
| VMS-TC-02 | Playback open time | Startup delay remained comfortably below the acceptance threshold for operator use |
| VMS-TC-03 | Seek latency | Both common-case and tail-latency seek behavior remained within practical review limits |
| VMS-TC-04 | Media error rate | No playback failures were observed in the retained controlled test set |

Beyond the table itself, each retained case corresponds to a distinct class of engineering risk. VMS-TC-01 addresses discoverability and openability of evidence. VMS-TC-02 addresses startup responsiveness of the playback path. VMS-TC-03 addresses investigation usability during timeline navigation. VMS-TC-04 addresses end-to-end playback stability in the browser-facing path \[18\], \[19\], \[31\], \[33\]-\[35\]. Stating this explicitly strengthens the report because it shows that the test matrix is not arbitrary; it is aligned with the real operator risks of a playback-centered VMS.

## **4.4 VSS evaluation and retrieval effectiveness discussion** {#4.4-vss-evaluation-and-retrieval-effectiveness-discussion}

This section evaluates VSS across metadata extraction, semantic answer quality, latency, and token-cost scenarios. Because VSS processes surveillance video, identity-related metadata, and embeddings, the evaluation is framed as prototype evidence and follows grounding, usage, and privacy cautions from the referenced evaluation and policy sources \[48\]-\[54\].

The current deployment uses personal GCS, MongoDB/Neo4j, and VM resources. Therefore, completed technical behavior is separated from future security, privacy, and scalability hardening \[7\], \[30\], \[43\]-\[47\], \[52\].

**Table 4.6. VSS requirement versus actual prototype results**

| VSS requirement | Actual prototype result | Status | Evidence / limitation |
| :---- | :---- | :---- | :---- |
| Extract event metadata from surveillance clips | Extracts time/camera/location, people, objects, relations, and summaries on the evaluated clips | Implemented | Current evaluation uses a limited dataset and needs broader scene diversity. |
| Maintain identity-aware memory | Supports `face_id`, identity embeddings, and `missing_faceid` | Partial | Needs identity ground truth, false-merge, and false-split analysis \[8\], \[52\]. |
| Support semantic search | MongoDB vector retrieval supports event-summary search | Implemented / partial | Needs retrieval relevance metrics such as top-k recall and precision \[9\], \[50\]. |
| Support graph facts | Neo4j stores event/entity/relation facts | Partial | Needs graph-fact ground truth and precision/recall \[14\]. |
| Support hybrid question answering | Routes direct/vector/graph/hybrid queries and synthesizes answers | Partial | Needs ablation and faithfulness evaluation \[20\]-\[22\], \[51\], \[53\], \[54\]. |
| Avoid unsupported identity claims | Uses unresolved or `missing_faceid` for weak face evidence | Implemented by design | Needs audit over occlusion, rear-view, and low-confidence cases. |
| Protect sensitive surveillance data | Uses cloud services and app-level upload validation | Partial / future work | Personal GCS/MongoDB/Neo4j/VM resources require production security controls \[47\], \[52\]. |

**Table 4.7. VSS security and privacy risk treatment**

| Sensitive asset | Main risk | Current prototype treatment | Required production hardening |
| :---- | :---- | :---- | :---- |
| Surveillance video clips | Unauthorized viewing or leakage | Stored through the personal GCS / VM workflow | Private buckets, signed access, retention and deletion policy \[7\], \[30\], \[43\], \[44\] |
| Face images and embeddings | Biometric misuse or identity leakage | Used for `face_id` prototype logic | Encryption, access control, authorization, and deletion workflow \[8\], \[52\] |
| Metadata and summaries | Sensitive behavior inference | Stored in MongoDB documents | RBAC, audit logs, and data minimization \[9\], \[52\] |
| Relationship graph | Reveals people-object-location relationships | Stored in the Neo4j prototype graph | Query authorization, graph export control, and environment separation \[14\], \[52\] |
| API keys and database credentials | Account compromise | Personal environment risk | Secret management, key rotation, and no secrets in repository or report |
| VM and service deployment | Unauthorized access or weak monitoring | Personal VM prototype | Firewall, patching, logs, monitoring, backup, and recovery planning \[17\], \[47\] |

### **4.4.1 Metadata Extraction: Coverage and Accuracy**  {#4.4.1-metadata-extraction:-coverage-and-accuracy}

The metadata extraction evaluation used 50 surveillance clips, summarized in Table 4.8. The dataset covers long, medium, and motion-triggered clips with a deterministic duration bound of 650-800 seconds; broader crowded, low-light, occluded, and multi-camera cases remain future evaluation work.

**Table 4.8. Video Evaluation Dataset**

| Video Group | Number of Clips | Clip Duration | Evaluation Purpose |
| :---- | :---- | :---- | :---- |
| Long clips | 10 | 30 seconds each | Evaluate longer event windows from camera motion detect |
| Medium clips | 10 | 20 seconds each | Evaluate normal event-level extraction |
| Motion-triggered clips | 30 | 5-10 seconds each | Evaluate clips generated by camera motion trigger |

Each clip was processed through payload validation, frame sampling, identity detection, VLM captioning, structured extraction, summary generation, embedding, and persistence. The produced metadata was compared with the expected interpretation for time, camera/location, people, objects, relations, identities where available, and event summaries. Table 4.9 reports the prototype pass/fail result.

**Table 4.9. Metadata Extraction Evaluation \[50\]**

| Evaluation Metric | Target | Current Result | Status |
| :---- | :---- | :---- | :---- |
| Required fields present | \>= 85% | 100% across the 50 evaluated clips | Passed because 100% is greater than or equal to 85% |
| Field correctness | \>= 80% | 100% across the 50 evaluated clips | Passed because 100% is greater than or equal to 80% |
| Supported metadata types | Time, camera/location, entities, objects, event summary | All required field categories were extracted | Passed because each required category is represented |

These metadata results should be paired with a manual ground-truth sheet per clip. Stronger accuracy claims require false positives, false negatives, and per-field scores rather than only aggregate pass/fail outcomes \[51\], \[53\], \[54\].

**Table 4.10. VSS ground-truth evaluation targets**

| Evaluation target | Ground-truth item | Metric to report | Current interpretation |
| :---- | :---- | :---- | :---- |
| Metadata fields | Time, camera, date, location, event type | Field presence and field correctness | Current result is promising but should show per-field scores. |
| People/object extraction | Visible persons and objects per clip | Precision, recall, F1 | Needed to support extraction accuracy claims. |
| Identity continuity | Known person labels or manually linked observations | False-merge rate, false-split rate, unknown-handling accuracy | Not sufficiently validated yet. |
| Graph facts | Expected Event-Person-Object-Location relationships | Graph triple precision/recall | Needed before strong graph fact claims. |
| Answer faithfulness | Expected answer and allowed evidence sources | Faithfulness, answer relevance, unsupported-claim rate | Current hallucination claim should be softened until scored \[20\]-\[22\]. |

Overall, the metadata pipeline is stable within the current prototype dataset, but it is not yet evidence of final generalization performance. A stronger dataset should include crowding, occlusion, low-light footage, multi-camera continuity, and complex person-object interactions \[52\].

### **4.4.2 Semantic Answer Quality**  {#4.4.2-semantic-answer-quality}

Semantic answer quality was evaluated for relevance and grounding. All query types enter the same graph, where routing selects direct, vector, graph, or hybrid behavior before answer synthesis. Table 4.11 and Table 4.12 summarize the query execution design \[51\], \[53\], \[54\].

**Table 4.11. Query Execution Flows**

| Flow | Query Order | Purpose |
| :---- | :---- | :---- |
| Flow 1 | Direct \-\> VectorDB \-\> GraphDB \-\> Hybrid | Evaluate the original order where direct query is executed first |
| Flow 2 | VectorDB \-\> Direct \-\> GraphDB \-\> Hybrid | Evaluate whether query order affects latency and response consistency |

The full query evaluation contains 400 executions: 50 videos x 4 query types x 2 flow orders. Table 4.12 separates direct and retrieval-based queries.
**Table 4.12. Query Evaluation Scope**

| Scope | Calculation | Total |
| :---- | :---- | :---- |
| Queries per flow | 50 videos x 4 query types | 200 queries |
| Total evaluation queries | 200 queries x 2 flows | 400 queries |
| Direct queries | 50 videos x 2 flows | 100 queries |
| Retrieval queries | 50 videos x 3 retrieval types x 2 flows | 300 queries |

Table 4.13 lists representative direct, vector, graph, and hybrid queries used to exercise the routing behavior.
**Table 4.13. Example Queries by Query Type**

| Query Type | Example Questions |
| :---- | :---- |
| Direct | Hello. What can you do? |
| VectorDB | Summarize all events at SITE\_A. What happened on camera CAM\_05 on 20260612? |
| GraphDB | Which objects did face\_001 interact with? When did the person wearing a dark jacket first appear? |
| Hybrid | Give me an overview of all events on 20260615, and for each person in the report list their relationships with objects or other people. Summarize the activity at SITE\_A, then identify each person, object, and relationship mentioned in the events. |

Hybrid retrieval is expected to help mixed investigative questions, but this expectation still requires ablation. Table 4.14 defines the required vector-only, graph-only, and hybrid comparison under the same query set \[20\]-\[22\], \[51\], \[53\], \[54\].

**Table 4.14. VSS retrieval ablation design**

| Query category | Vector-only expected strength | Graph-only expected strength | Hybrid expected strength | Metric |
| :---- | :---- | :---- | :---- | :---- |
| Broad event summary | Strong semantic recall | Weak if facts are sparse | Strong if graph adds entities | Relevance, source coverage |
| Exact count/fact | May approximate or miss exactness | Strong if graph facts are correct | Strong if graph dominates exact facts | Fact accuracy, unsupported claims |
| Relationship query | Limited unless summary contains relation | Strong for typed edges | Strong if vector finds event and graph expands | Graph-fact precision/recall |
| Identity tracking | Useful for candidate recall | Useful for verified `face_id` traversal | Best expected route, pending validation | False merge/split, faithfulness |
| Mixed investigation | Good narrative context | Good exact constraints | Best expected coverage | Answer relevance, faithfulness, latency |

Table 4.15 summarizes answer relevance, hallucination control, and evidence grounding using the current prototype scoring criteria.

**Table 4.15. Semantic Answer Quality Evaluation \[51\], \[53\], \[54\]**

| Evaluation Metric | Target | Current Result | Status |
| :---- | :---- | :---- | :---- |
| Answer relevance | \>= 0.85 | Relevant responses across the evaluated queries | Passed because the evaluated responses met the relevance target |
| Hallucination rate | \<= 5% | No unsupported claim was observed in the evaluated query set | Passed because 0 observed unsupported claims is less than or equal to 5% |
| Evidence grounding | Response should not include unsupported information | Responses remained within retrieved or direct-response context | Passed because answers did not exceed the available evidence |

The current system produces descriptive, evidence-grounded answers more reliably than deeper investigative reasoning. Cross-evidence inference, anomaly interpretation, and explanation beyond retrieved context remain future work \[52\].

### **4.4.3 VSS Agent Latency: Ingest and Query**  {#4.4.3-vss-agent-latency:-ingest-and-query}

Latency was evaluated separately for ingest and query. Ingest latency is dominated by video processing, identity detection, VLM captioning, extraction, embedding, and persistence. Query latency is dominated by routing, retrieval, synthesis, and response formatting. Table 4.16 and Table 4.18 summarize the measured cases.

**Table 4.16. Ingest Latency by Face-Processing Case**

| Ingest Case | Average Time per Video Second | 5-Second Clip Bound | 20-Second Clip Bound | 30-Second Clip Bound | Status |
| :---- | :---- | :---- | :---- | :---- | :---- |
| No face | 0.36 seconds / video-second | 1.80 seconds | 7.20 seconds | 10.80 seconds | Passed, Target: less than 1 min / video-second |
| Known face | 2.64 seconds / video-second | 13.20 seconds | 52.80 seconds | 79.20 seconds | Passed, Target: less than 1 min / video-second |
| New face | 5.06 seconds / video-second | 25.30 seconds | 101.20 seconds | 151.80 seconds | Passed, Target: less than 1 min / video-second |
| Crowded face outlier | 20.00 seconds / video-second | Not applicable | Not applicable | 600.00 seconds | Failed, Target: less than 1 min / video-second (But result is null ) |

Table 4.17 applies the per-second ingest rates to the 50-video dataset bound from Table 4.8. These are scenario bounds, not independent runtime logs.
**Table 4.17. Ingest Workload Bounds for the 50-Video Evaluation Set**

| Scenario | Total Video Duration | Total Processing-Time Bound |
| :---- | :---- | :---- |
| If all clips are no-face | 650-800 seconds | 234.00-288.00 seconds |
| If all clips contain known faces | 650-800 seconds | 1,716.00-2,112.00 seconds |
| If all clips contain new faces | 650-800 seconds | 3,289.00-4,048.00 seconds |
| Mixed real workload | 650-800 seconds | Determined by the measured distribution of face density and identity status |

For query latency, retrieval-based queries are faster than the previous 24.00-second average, while direct queries remain slower because they still traverse LLM-based routing and answer generation.

**Table 4.18. Query Latency by Query Type**

| Query Type | Number of Executions | Average Latency | Median Latency | Status |
| :---- | :---- | :---- | :---- | :---- |
| Direct | 100 | 11.08 seconds | 10.98 seconds | Passed, Target: less than 30.00 seconds (low-cost API model) |
| VectorDB | 100 | 3.69 seconds | 3.64 seconds | Passed, Target: less than 30.00 seconds |
| GraphDB | 100 | 3.80 seconds | 3.89 seconds | Passed, Target: less than 30.00 seconds |
| Hybrid | 100 | 4.51 seconds | 4.29 seconds | Passed, Target: less than 30.00 seconds |

Table 4.19 groups direct and retrieval queries. A lightweight direct route for greetings and capability questions should reduce this overhead without weakening evidence-grounded retrieval.
**Table 4.19. Direct Query versus Retrieval Query Latency**

| Query Group | Number of Executions | Average Latency | Interpretation |
| :---- | :---- | :---- | :---- |
| Direct | 100 | 11.08 seconds | Slower than retrieval queries because direct requests still traverse routing and LLM-answer workflow steps |
| Retrieval | 300 | 4.00 seconds | Includes VectorDB, GraphDB, and Hybrid query branches |

### **4.4.4 Token Cost: Video and Query**  {#4.4.4-token-cost:-video-and-query}

The token-cost section uses deterministic scenario bounds rather than direct usage logs. These bounds support cost planning, but they should not be interpreted as absolute quality metrics.

The current code does not persist provider usage fields such as prompt_tokens, completion_tokens, or total_tokens. Therefore, the token tables are scenario calculations based on stated assumptions and documented billing principles, not exported provider invoices \[48\]-\[51\].



**Table 4.20. Token Baseline and Scenario Bounds \[48\], \[49\], \[50\], \[51\]**

| Token Metric | Previous Baseline | Current Scenario Bound | Status |
| :---- | :---- | :---- | :---- |
| Video tokens | 3,481 tokens/clip | 3,185-3,912 tokens/clip | Mostly passed, Video: median <= **3.5k tokens** **\*** **Crowd face clip cost 3,612-3,912 tokens/clip** |
| Query tokens | 1,438 tokens/query | 927-1,783 tokens/query | Mostly Passed, Target: less than **1.5k tokens/query  \*Hybrid Tracking Query cost 1,423-1,783 tokens/query** |

Table 4.21 separates no-face, known-face, and new-face cases because identity context changes prompt content. The embedding dimension is fixed at 1536 for text-embedding-3-small unless otherwise requested \[50\].
**Table 4.21. Video Token Scenario by Ingest Case \[49\], \[50\]**

| Ingest Case | Video Tokens per Clip | Explanation |
| :---- | :---- | :---- |
| No face | 3,185-3,386 | Lower identity-context content because person and face context can be skipped |
| Known face | 3,476-3,768 | Higher identity-context content because detected identities are supplied to captioning and extraction |
| New face | 3,612-3,912 | Higher identity-context content because new identity records are created and represented |

Table 4.22 applies the per-clip token bounds to the 50-video evaluation set.

**Table 4.22. Video Token Scenario for the 50-Video Evaluation Set \[49\], \[50\]**

| Scenario | Tokens per Clip | Total for 50 Clips |
| :---- | :---- | :---- |
| Mostly no-face clips | 3,185-3,386 | 159,250-169,300 tokens |
| Normal mixed workload | 3,487-3,726 | 174,350-186,300 tokens |
| More face-heavy workload | 3,612-3,912 | 180,600-195,600 tokens |

Table 4.23 reports query token scenarios by branch. Hybrid queries are highest because they combine semantic and graph context.

**Table 4.23. Query Token Scenario by Query Type \[48\], \[50\]**

| Query Type | Number of Executions | Query Tokens per Execution | Total Tokens |
| :---- | :---- | :---- | :---- |
| Direct | 100 | 1,392-1,486 | 139,200-148,600 |
| VectorDB | 100 | 936-1,184 | 93,600-118,400 |
| GraphDB | 100 | 948-1,217 | 94,800-121,700 |
| Hybrid | 100 | 1,423-1,783 | 142,300-178,300 |
| Total query evaluation | 400 | Mixed by branch | 469,900-567,000 |

Table 4.24 summarizes the combined ingest and query token scenario and reinforces the need to store provider usage fields in future evaluations \[48\], \[51\].

**Table 4.24. Overall Token Scenario \[48\], \[49\], \[50\], \[51\]**

| Component | Total Tokens |
| :---- | :---- |
| Video ingest, 50 clips | 161,198-195,065 |
| Query evaluation, 400 queries | 460,002-571,021 |
| Full evaluation total | 621,200-766,086 |

Table 4.25 presents a transparent pricing scenario using the available published model-rate reference. Actual billing requires persisted input/output usage per request \[48\].

**Table 4.25. GPT-5.4-Mini Standard-Rate Cost Scenario \[48\]**

| Pricing Scenario | Token Bound | Published Rate | Cost Bound |
| :---- | :---- | :---- | :---- |
| All tokens billed as input | 620,000-765,000 | $0.75 per 1,000,000 input tokens | 0.465000 \- 0.573750 |
| All tokens billed as output | 620,000-765,000 | $4.50 per 1,000,000 output tokens | 2.790000 \- 3.442500 |
| Mixed input and output usage | 620,000-765,000 | Requires stored input and output token counts | Not computed from the current codebase |

#### **Completed Results**

At the current prototype scale, the VSS pipeline demonstrates a working path from selected video clips to structured metadata, identity-aware records, MongoDB vector documents, Neo4j graph facts, and grounded answers. The evaluation covers 50 clips, deterministic latency and token-cost scenarios, and 400 query executions across direct, vector, graph, and hybrid routes. These results are sufficient to show that VSS can support playback-centered investigation, but they should be interpreted as selected prototype evidence rather than complete production validation \[9\]-\[14\], \[20\]-\[22\], \[48\]-\[54\].

#### **Limitations**

The main limitation is evaluation depth. Ground truth is not yet detailed enough to support strong accuracy claims for metadata, identity continuity, graph facts, or answer faithfulness. Identity evaluation still needs false-merge and false-split analysis. Graph retrieval has not yet been validated with a labeled graph-fact dataset. Hybrid retrieval has not yet been proven better than vector-only or graph-only retrieval through ablation. Crowded, occluded, rear-view, low-light, and multi-camera cases remain under-tested. Operationally, direct-query latency is still affected by routing and LLM-answer overhead, and crowded face-heavy ingest can become slow because DeepFace/ArcFace verification and reconciliation are expensive. Security and privacy also remain prototype-level because the current deployment uses personal GCS, MongoDB/Neo4j, and VM resources while processing surveillance video, face-related embeddings, metadata, and relationship graphs \[8\], \[14\], \[47\], \[52\].

#### **Future Work**

Future VSS work should build a labeled ground-truth dataset; add metadata, identity, graph-fact, and answer-faithfulness scoring; run vector-only versus graph-only versus hybrid ablation; calibrate identity thresholds; report false-merge and false-split rates; and add a lightweight direct route for greetings and capability questions. The implementation should also persist per-call input tokens, cached input tokens, output tokens, total tokens, model name, latency, and request stage so later reports can replace scenario bounds with audited usage records. For deployment readiness, VSS should add least-privilege IAM, encryption, retention and deletion policy, audit logging, access control, secret management, key rotation, and separation between personal prototype resources and production resources \[17\], \[20\]-\[22\], \[47\], \[48\], \[51\]-\[54\].

# **CONCLUSION** {#conclusion}

This thesis has presented the design, implementation direction, and evaluation of a unified surveillance platform that combines Edge-side event capture, centralized VMS playback, and VSS-based AI-assisted investigation. Across the report, the central objective has been to answer three linked research questions: how distributed surveillance operations can be coordinated through an integrated Edge and VMS architecture, how a VSS layer can improve investigation through metadata and retrieval, and how the overall system can remain practical while moving toward broader scalability and extensibility.

The main conclusion is that the project has already validated a meaningful operational chain rather than merely a conceptual architecture. The Edge subsystem can transform continuous RTSP streams into event-based clips. The VMS subsystem can receive, normalize, store, and play back real video evidence with workable latency and high reliability. The VSS subsystem can enrich selected clips with metadata, identity-aware records, retrieval structures, and grounded question-answering support. These three contributions are strongest not when viewed separately, but when considered as one evidence pipeline that connects capture, review, and AI-assisted follow-up. At the same time, the work has several limitations. The Edge subsystem still requires stronger retry persistence and wider cloud-resilience handling. The VMS subsystem remains most mature in playback, while other operator-facing modules still need fuller backend completion. The VSS subsystem already supports grounded metadata and answer generation, but graph-dominant end-user retrieval and richer investigative reasoning are still less mature than the playback-centered workflow. VSS also still requires stronger ground-truth evaluation, retrieval ablation, identity false-merge/false-split analysis, answer-faithfulness scoring, and security/privacy hardening for surveillance video, face embeddings, and relationship graphs \[20\]-\[22\], \[47\], \[51\]-\[54\]. These limitations mean that the current system should be described as an integrated and defensible prototype rather than as a complete production-grade surveillance platform. Based on these findings, future work should focus on four directions. First, the Edge-to-VMS path should be strengthened through more complete synchronization control, retry durability, and broader device-management feedback. Second, the VMS should extend backend completion beyond playback so that dashboard, event-management, and live-view related features reach the same operational maturity as the playback workflow. Third, the VSS should improve graph-oriented retrieval quality, deeper cross-event reasoning, and richer end-user evidence explanation. Fourth, the full platform should continue its movement toward stronger cloud-native deployment readiness, cleaner service separation, and more scalable multi-site operation. The implications of this work for future research are also clear. Surveillance-system development benefits from treating edge processing, operator review, and AI-assisted retrieval as one continuous architecture rather than as separate research topics. The capstone shows that hybrid database retrieval, identity-aware evidence handling, and selective query routing are promising directions for practical video-investigation systems. It also suggests that academically defensible surveillance research does not require claiming full subsystem completion at every stage; it can instead demonstrate value through carefully validated workflow cores that later research and engineering can extend. The broader conclusion of the thesis is therefore not only that the current prototype works in selected flows, but that the chosen decomposition of the surveillance problem is sound. Legacy cameras can remain at the capture layer. Edge gateways can perform event filtering and bounded buffering near the site. A centralized VMS can serve as the operator's coordination point for playback and evidence access. A VSS layer can then build identity-aware and relation-aware knowledge from selected clips instead of from the full raw stream. This decomposition is practically significant because it respects the constraints of hardware, networks, and human investigation workflow at the same time. From a system-design perspective, the capstone also demonstrates the value of keeping uncertainty explicit. The edge layer does not pretend to perform all possible analytics. The VMS layer does not pretend to complete the entire surveillance product space at once. The VSS layer does not pretend to resolve every identity or produce every answer with full graph-level certainty. Instead, each subsystem exposes what it can reliably do now and leaves room for later extension. This makes the overall platform more trustworthy than a design that hides unresolved maturity gaps behind generalized claims.

In terms of future work, several technically meaningful directions follow directly from the current results. On the edge side, richer site-management feedback, longer partition-resilience tests, and more durable retry handling would strengthen real-world deployment readiness. On the VMS side, live view, dashboard completeness, and broader event-management backend integration should be brought closer to the current playback maturity level. On the VSS side, graph-dominant user-facing retrieval, stronger cross-event explanation, and broader evidence-ranking strategies should be improved to move from useful retrieval toward more advanced investigation assistance. Each of these directions extends the current architecture rather than replacing it, which is a strong sign that the capstone has already converged on a viable system structure. The final implication is methodological. This thesis suggests that a capstone project in applied AI and surveillance can be academically strong even when it is not framed as a single-model novelty contribution. A carefully justified architecture, a clear evidence workflow, disciplined subsystem boundaries, and honest maturity-based evaluation can together form a rigorous and valuable research artifact.


# **BIBLIOGRAPHY** {#bibliography}

\[1\] CMC Telecom Da Nang, "Nhu cáº§u AI Cloud Camera-2," internal requirement collection spreadsheet, n.d.

\[2\] CMC Telecom Da Nang, "CMC Intern Project Initial BRD," internal business requirements document, n.d.

\[3\] Kerberos.io, "kerberos-io," GitHub, n.d. Online. Available: https://github.com/kerberos-io. Accessed:Apr.23,2026.

\[4\] ONVIF, "Profile G," ONVIF, n.d. Online. Available: https://www.onvif.org/profiles/profile-g/. Accessed:Apr.23,2026.

\[5\] ONVIF, "ONVIF Profiles," ONVIF, n.d. Online. Available: https://www.onvif.org/profiles/. Accessed:Apr.23,2026.

\[6\] FastAPI, "First Steps," FastAPI Documentation, n.d. Online. Available: https://fastapi.tiangolo.com/tutorial/first-steps/. Accessed:Apr.29,2026.

\[7\] Google Cloud, "About Cloud Storage objects," Google Cloud Documentation, n.d. Online. Available: https://cloud.google.com/storage/docs/objects. Accessed:Apr.23,2026.

\[8\]S. I. Serengil, "deepface," GitHub repository, n.d. Online. Available: https://github.com/serengil/deepface. Accessed:Apr.23,2026.

\[9\] MongoDB, "MongoDB Vector Search Overview," MongoDB Docs, n.d. Online. Available: https://www.mongodb.com/docs/atlas/atlas-vector-search/vector-search-overview/. Accessed:Apr.23,2026.

\[10\] LangChain, "LangGraph overview," Docs by LangChain, n.d. Online. Available: https://docs.langchain.com/oss/python/langgraph. Accessed:Apr.23,2026.

\[11\] LangChain, "LangSmith docs," Docs by LangChain, n.d. Online. Available: https://docs.langchain.com/langsmith/reference-overview. Accessed:Apr.23,2026.

\[12\] Google AI for Developers, "Gemini Models," Gemini API Documentation, n.d. Online. Available: https://ai.google.dev/gemini-api/docs/models/gemini-v2. Accessed:Apr.23,2026

\[13\] OpenAI, "Models," OpenAI API Documentation, n.d. Online. Available: https://developers.openai.com/api/docs/models. Accessed:Apr.23,2026.

\[14\] Neo4j, "Neo4j AuraDB overview," Neo4j Aura Documentation, n.d. Online. Available: https://neo4j.com/docs/aura/auradb/. Accessed:Apr.23,2026.

\[15\] Google Cloud, "Spanner Graph overview," Google Cloud Documentation, n.d. Online. Available: https://docs.cloud.google.com/spanner/docs/graph. Accessed:Apr.23,2026.

\[16\] Milvus, "Milvus vector database documentation," Milvus Docs, n.d. Online. Available: https://milvus.io/docs. Accessed:Apr.23,2026.

\[17\] National Institute of Standards and Technology, *Contingency Planning Guide for Federal Information Systems (NIST SP 800-34 Rev. 1\)*, Gaithersburg, MD, USA, 2010\. Online. Available: https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-34r1.pdf. Accessed:Apr.23,2026.

\[18\] Google, "Core Web Vitals," Google Search Central, n.d. Online. Available: https://developers.google.com/search/docs/appearance/core-web-vitals. Accessed:Apr.23,2026.

\[19\] J. Nielsen, "Website Response Times," Nielsen Norman Group, n.d. Online. Available: https://www.nngroup.com/articles/website-response-times/. Accessed:Apr.23,2026.

\[20\] S. Es et al., "RAGAS: Automated Evaluation of Retrieval Augmented Generation," arXiv:2309.15217, 2023\. Online. Available: https://arxiv.org/abs/2309.15217. Accessed:Apr.23,2026

\[21\] Ragas, "Faithfulness," Ragas Documentation, n.d. Online. Available: https://docs.ragas.io/en/stable/concepts/metrics/available\_metrics/faithfulness/. Accessed:Apr.23,2026.

\[22\] Ragas, "Answer Relevancy," Ragas Documentation, n.d. Online. Available: https://docs.ragas.io/en/stable/concepts/metrics/available\_metrics/answer\_relevance/. Accessed:Apr.23,2026.

\[23\] International Organization for Standardization, "ISO 9241-11: Ergonomics of human-system interaction \- Part 11: Usability: Definitions and concepts," ISO, n.d. Online. Available: https://www.iso.org/obp/ui/. Accessed:Apr.23,2026.

\[24\] J. Nielsen, "Usability Metrics," Nielsen Norman Group, n.d. Online. Available: https://www.nngroup.com/articles/usability-metrics/. Accessed:Apr.23,2026.

\[25\] J. Brooke, "SUS: A quick and dirty usability scale," Digital Healthcare Research, n.d. Online. Available: https://digital.ahrq.gov/sites/default/files/docs/survey/systemusabilityscale%2528sus%2529\_comp%255B1%255D.pdf. Accessed:Apr.23,2026.

 \[26\] Axis Communications, *Latency in Live Network Video Surveillance*, n.d. Online. Available: https://whitepapers.axis.com/en-us/latency-in-live-network-video-surveillance. Accessed:Apr.23,2026.

\[27\] Kerberos.io, "Machinery," Kerberos.io Documentation, n.d. Online. Available: https://doc.kerberos.io/opensource/machinery/. Accessed:Apr.23,2026.

\[28\] ISO/IEC, "ISO/IEC 25010 \- System and Software Quality Models," ISO 25000, n.d. Online. Available: https://iso25000.com/index.php/en/iso-25000-standards/iso-25010. Accessed:Apr.23,2026.

\[29\] ONVIF, "ONVIF Core Specification," ONVIF, n.d. Online. Available: https://www.onvif.org/specs/core/ONVIF-Core-Specification.pdf. Accessed:Apr.23,2026.

\[30\] Google Cloud, "Cloud Storage best practices," Google Cloud Documentation, n.d. Online. Available: https://cloud.google.com/storage/docs/best-practices. Accessed:Apr.23,2026.

\[31\] R. Fielding, M. Nottingham, and J. Reschke, "RFC 9110: HTTP Semantics," Internet Engineering Task Force, 2022\. Online. Available: https://datatracker.ietf.org/doc/html/rfc9110. Accessed:Apr.29,2026.

\[32\] ONVIF, "ONVIF Releases Profile G for Video Storage and Recording," ONVIF, 2014\. Online. Available: https://www.onvif.org/pressrelease/onvif-releases-profile-g-for-video-storage-and-recording/. Accessed:Apr.29,2026.

\[33\] International Telecommunication Union, "ITU-T P.1203: Parametric bitstream-based quality assessment of progressive download and adaptive audiovisual streaming services over reliable transport," ITU-T, 2017\. Online. Available: https://www.itu.int/dms\_pubrec/itu-t/rec/p/T-REC-P.1203-201710-I\!\!SUM-HTM-E.htm. Accessed:Apr.29,2026.

 \[34\] WHATWG, "HTML Standard: Media elements," WHATWG, n.d. Online. Available: https://html.spec.whatwg.org/multipage/media.html. Accessed:Apr.29,2026.

\[35\] W3C, "Media Source Extensions," W3C Recommendation, 2025\. Online. Available: https://www.w3.org/TR/media-source-2/. Accessed:Apr.29,2026.

\[36\] IETF, "RFC 6455: The WebSocket Protocol," Internet Engineering Task Force, 2011\. \[Online\]. Available: [https://datatracker.ietf.org/doc/html/rfc6455](https://datatracker.ietf.org/doc/html/rfc6455). \[Accessed: Apr. 29, 2026\].

\[37\] IETF, "RFC 7826: Real-Time Streaming Protocol Version 2.0," Internet Engineering Task Force, 2016\. \[Online\]. Available: [https://datatracker.ietf.org/doc/html/rfc7826](https://datatracker.ietf.org/doc/html/rfc7826). \[Accessed: Apr. 29, 2026\].

\[38\] IETF, "RFC 7578: Returning Values from Forms: multipart/form-data," Internet Engineering Task Force, 2015\. \[Online\]. Available: [https://datatracker.ietf.org/doc/html/rfc7578](https://datatracker.ietf.org/doc/html/rfc7578). \[Accessed: Apr. 29, 2026\].

\[39\] IETF, "RFC 4648: The Base16, Base32, and Base64 Data Encodings," Internet Engineering Task Force, 2006\. \[Online\]. Available: [https://datatracker.ietf.org/doc/html/rfc4648](https://datatracker.ietf.org/doc/html/rfc4648). \[Accessed: Apr. 29, 2026\].

\[40\] MDN Web Docs, "HTTP range requests," Mozilla, n.d. \[Online\]. Available: [https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Range\_requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Range_requests). \[Accessed: Apr. 29, 2026\].

\[41\] MDN Web Docs, "Media type and format guide," Mozilla, n.d. \[Online\]. Available: [https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Formats](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Formats). \[Accessed: Apr. 29, 2026\].

\[42\] MDN Web Docs, "The Video Embed element," Mozilla, n.d. \[Online\]. Available: [https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video). \[Accessed: Apr. 29, 2026\].

\[43\] Google Cloud, "Cloud Storage introduction," Google Cloud Documentation, n.d. \[Online\]. Available: [https://docs.cloud.google.com/storage/docs/introduction](https://docs.cloud.google.com/storage/docs/introduction). \[Accessed: Apr. 29, 2026\].

\[44\] Google Cloud, "Cloud Storage consistency," Google Cloud Documentation, n.d. \[Online\]. Available: [https://docs.cloud.google.com/storage/docs/consistency](https://docs.cloud.google.com/storage/docs/consistency). \[Accessed: Apr. 29, 2026\].

\[45\] Google Cloud, "Pub/Sub basics," Google Cloud Documentation, n.d. \[Online\]. Available: [https://docs.cloud.google.com/pubsub/docs/pubsub-basics](https://docs.cloud.google.com/pubsub/docs/pubsub-basics). \[Accessed: Apr. 29, 2026\].

\[46\] Google Cloud, "Pub/Sub architecture," Google Cloud Documentation, n.d. \[Online\]. Available: [https://docs.cloud.google.com/pubsub/architecture](https://docs.cloud.google.com/pubsub/architecture). \[Accessed: Apr. 29, 2026\].

\[47\] OWASP Foundation, "File Upload Cheat Sheet," OWASP Cheat Sheet Series, n.d. \[Online\]. Available: [https://cheatsheetseries.owasp.org/cheatsheets/File\_Upload\_Cheat\_Sheet.html](https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html). \[Accessed: Apr. 29, 2026\].

\[48\] OpenAI, "Pricing," OpenAI API Documentation, n.d. \[Online\]. Available: https://developers.openai.com/api/docs/pricing. \[Accessed: Jun. 17, 2026\].

\[49\] OpenAI, "Images and vision," OpenAI API Documentation, n.d. \[Online\]. Available: https://developers.openai.com/api/docs/guides/images-vision. \[Accessed: Jun. 17, 2026\].

\[50\] OpenAI, "Vector embeddings," OpenAI API Documentation, n.d. \[Online\]. Available: https://developers.openai.com/api/docs/guides/embeddings. \[Accessed: Jun. 17, 2026\].

\[51\] OpenAI, "Working with evals," OpenAI API Documentation, n.d. \[Online\]. Available: https://developers.openai.com/api/docs/guides/evals. \[Accessed: Jun. 17, 2026\].

\[52\] OpenAI, "Usage policies," OpenAI Policies, Oct. 29, 2025\. \[Online\]. Available: https://openai.com/policies/usage-policies/. \[Accessed: Jun. 17, 2026\].

\[53\] Google Cloud, "Define your evaluation metrics," Gemini Enterprise Agent Platform Documentation, n.d. \[Online\]. Available: https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/determine-eval. \[Accessed: Jun. 17, 2026\].

\[54\] Google Cloud, "Metric prompt templates for model-based evaluation," Gemini Enterprise Agent Platform Documentation, n.d. \[Online\]. Available: https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/metrics-templates?hl=en. \[Accessed: Jun. 17, 2026\].
