# AI Build Log: Razorpay Open Box Delivery

**Vibhor Mittal** | Razorpay AI PM Build Challenge, ISB | Track 2: Recover and Grow with AI | 26 September to 13 October 2026

The idea and the product calls were mine. Three AI assistants did much of the research, critique and build work. This log covers the tools, how I set them up and prompted them, one output I threw away and rebuilt, who did what, and what the submission does not prove.

---

## 1. AI tools and models used

| Tool | Model or version | What I used it for |
|---|---|---|
| Instinct (AI assistant on WhatsApp) | Underlying model not disclosed by the tool | Ideation, research on Razorpay products and prior art, reading RBI and NPCI rules, unit economics, devil's-advocate reviews, early prototypes, an alternative male narration |
| ChatGPT, with image generation | Model version not recorded | Critique as a Razorpay PM and UI designer, scope cuts, buyer and merchant UX, making AI decisions visible, static screen mockups |
| Claude (Anthropic), agentic workspace | Claude Sonnet 5.5, then Claude Opus 5.5 | Sourcing segment data, critiquing the note against the brief, coding the final prototype, automated testing, building the demo video, drafting this log |
| Kokoro TTS | Kokoro-82M v1.0 (open source, ONNX) | Voiceover for the final demo video |
| faster-whisper | base.en (open source) | Transcribing the voiceover back to check every word and its timing |
| Playwright with Chromium | Open source | Clicking through every prototype path automatically, screenshots, recording the demo |
| FFmpeg, docx (npm), LibreOffice | Open source | Video encoding and audio mix; layout of the product note and logs |

---

## 2. Key prompts, workflows and agent setup

### Agent setup

Claude ran as an agent in a cloud workspace with web search, file read and write, a shell (Python and Node), and installed skills for Word documents and for auditing AI-sounding writing. It could build files, run scripts, render its own output to images and check them before handing anything back. It also published the prototype as a hosted page.

Instinct and ChatGPT were used as chat assistants, Instinct over WhatsApp text and voice notes.

### Workflows

1. **Explore, then narrow.** Instinct proposed directions and researched overlap with existing Razorpay products. I kept only ideas that Razorpay does not already ship.
2. **Critique loop on every deliverable.** Each output got a review pass as a PM, a judge or a devil's advocate before I accepted it.
3. **Prototype pipeline.** Claude read my four screen-flow boards (PDF) and coded a single-page HTML and JavaScript prototype. One live merchant policy drives both the customer and merchant screens. Playwright then clicked every step and in-screen action, checked for console errors, and tested laptop and phone widths.
4. **Video pipeline.**
   - An animation layer drives the real prototype: cursor, spotlight rings, camera zoom and captions.
   - Playwright records it frame by frame and FFmpeg encodes it.
   - Kokoro generates each line of the script, and faster-whisper transcribes it back so each line can be timed to the screen it describes.
5. **Writing pass.** Every document went through an AI-writing audit (em dashes, filler, stock phrasing) before it was finalised.

### Representative prompts (my own words)

- "Razorpay can build something like escrow on delivery for a COD alternative. Solves for user trust and the collections part for the merchant."
- "Now act as devil's advocate and break each claim and point."
- "Use Razorpay's theme. Focus on the MVP and how you pitch AI."
- "Why this customer, why this merchant?"
- "Critique as a PM and finalise. Do not change too much from the original idea, and format it like a product note, not an assignment."
- "Read each and every screen in the doc and build a prototype that looks exactly like the doc workflow."
- "This version seems too confusing. Make the easiest version of the customer journey."
- "As an expert product manager, a marketing head and a Razorpay evaluator, argue and give me the best structure."

---

## 3. One AI output I rejected and rebuilt: the prototype

**What the AI produced.** Claude's first working prototype reproduced every screen on my boards: 34 screens, 27 steps in a sidebar labelled with board codes ("Phase 1", "1A", "2C"), and two header toggles that changed the merchant rule and the item condition. When I asked for a critique, it added a welcome screen, renamed the sections as "journeys" and added a human-review flow.

**Why I rejected it.** It was faithful to my boards and wrong for the audience. A judge opening the link would see a long menu with no clear starting point, two doorstep flows that looked like duplicates, and controls that silently changed outcomes. Accurate is not the same as easy to evaluate.

**What I asked for instead.** "Make the easiest version of the customer journey. If some screens are not required we can remove them."

**What shipped.** A 13-step guided walkthrough in five chapters, with Back and Next and one plain-language line beside each screen. The chapters are:

1. Customer checks out
2. Merchant sets the rules
3. Delivery day
4. Merchant changes a rule
5. When the AI is unsure

The sidebar, the toggles, the dispute flow and the duplicate doorstep flow are gone.

**Other corrections:**
- **Regulation.** Instinct claimed no new permission was needed for the payment hold. After it read the RBI payment aggregator directions, the claim was dropped and feasibility became a launch gate.
- **Product note.** I rejected a rewrite that drifted from my idea and read like an assignment.
- **Video.** I cut it from about 30 screens to 15 and rejected a generic voiceover script.

---

## 4. What AI did versus what I decided or built

| Area | What AI did | What I decided |
|---|---|---|
| Problem choice | Proposed five directions and researched overlap with Agent Studio, ACE, Magic Checkout and Vulcan | Dropped all of them. Brought my own idea: escrow on delivery as a COD alternative |
| Segment | Sourced COD and RTO benchmarks and tested other segments | Chose electronics, because returns there come from trust in the product |
| Where AI fits | Drafted the Open Box Policy Engine and the scripted AI decisions | AI reads photos against each merchant's own policy. Money states stay rule-based, and if the AI cannot beat a simple rule, the rule ships |
| Trust and failure | Built the human-review screens | "AI triages, merchant decides." Below a merchant-set confidence threshold a person decides and the money stays on hold |
| Scope | Built 34 screens and a 30-screen video cut | Cut the prototype to 13 steps and the video to 15 screens. Not building change-of-mind returns, courier integration or order-level fraud scoring |
| Measurement | Drafted metric options | North star: incremental completed orders per merchant against a pre-launch baseline, proven with a holdout test |
| Design and story | Coded screens, animations, the voiceover and documents | The Razorpay look, the five-chapter story, the cold-open video structure and the final script |
| Inputs I supplied | | The original idea, the one-page product note draft and the four screen-flow boards |

---

## 5. Known limitations, risks and verification steps

### Limitations

- **Simulated AI.** Every AI decision in the prototype (photo checks, confidence scores, rule structuring, escalation) is a scripted rule. No vision model or LLM runs in the prototype.
- **Simulated payments.** UPI reservations, releases, pickups and disputes are simulated. No money moves and no real customer data is used.
- **Synthetic data.** All orders, products and merchants are synthetic.
- **Synthetic voice.** The voiceover is a synthetic voice (Kokoro), not a recording of me.
- **Vendor numbers.** Market figures are vendor benchmarks, not measured outcomes: Shipway FY25 (about 26% RTO on COD against under 2% prepaid), Cashfree on customer-intent RTO, and vendor estimates on electronics return costs. The early economics model was illustrative.

### Risks

- **Payment rail.** UPI Reserve Pay limits, release timing and dispute handling need confirmation from Razorpay and legal review. This includes the Rs 10,000 cap, which comes from secondary documentation.
- **Demand.** No merchant interviews or pilot data back this yet.
- **Fraud.** Customers could claim false damage or swap items. The design relies on timestamped in-app photos and caps on repeat rejections, and none of this has been tested.
- **Model risk.** A production vision model would need calibration, and an AI that is too strict or too lenient costs either the merchant or the buyer.

### Verification steps taken

- Checked AI regulatory claims against primary sources (RBI payment aggregator directions, NPCI OC-228 on UPI Reserve Pay) and dropped the one that failed.
- Labelled every market figure with its source and marked vendor estimates as such.
- Ran automated tests on the prototype: every step by Next, every in-screen action, no console errors, and no horizontal scroll at phone width.
- Transcribed the voiceover with faster-whisper to confirm the words and timing, and reviewed frames from every scene of the video.
- Cross-checked the product note, video, prototype and logs for the same claims and numbers.

### Before a pilot

- Confirm rail and legal feasibility with Razorpay.
- Interview 5 to 10 electronics merchants and use their order data: COD share, RTO reasons and defect claims.
- Run a holdout test to prove incremental completed orders, and compare the AI against a simple rule.

---

### Sources

- Shipway FY25 shipment benchmark, via MediaBrief: https://mediabrief.com/shipnotes-reveals-26-rto-rate-on-cod-orders-across-india/
- RBI Master Direction on payment aggregators: https://www.rbi.org.in/Scripts/BS_ViewMasDirections.aspx?id=12896
- NPCI OC-228, UPI Reserve Pay: https://www.npci.org.in/uploads/UPI_OC_No_228_FY_2025_26_Enhancement_in_UPI_Single_Block_Multiple_Debits_UPI_Reserve_Pay_a9095c181d.pdf
- Razorpay Magic Checkout: https://razorpay.com/magic-checkout/
- Razorpay Agent Studio: https://razorpay.com/blog/agent-studio-ai-agents-by-razorpay/
- Razorpay ACE: https://razorpay.com/agentic-commerce/
- Shiprocket Pay After Delivery: https://checkout.shiprocket.in/pay-after-delivery/
