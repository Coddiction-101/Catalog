# The Project Cave

> A problem-first catalog. Every entry starts with something that actually hurts, isolates, endangers, or wastes someone's life — described in full, not summarized in a sentence — followed by an honest account of why the tools that exist today only partially fix it. Only then comes the idea: something that treats computation as more than a screen, and tries to close the gap the existing solution leaves open.
>
> This is not a list of "cool tech." It's a list of unresolved damage, followed by an attempt to imagine what a computer would have to become to actually help.

---

## How This Document Works

Each entry follows the same shape, deliberately:

1. **The Problem** — who suffers, how it actually plays out day to day, why it's not a minor inconvenience.
2. **The Blind Spot** — the specific mechanism by which the problem hides, spreads, or gets ignored.
3. **What Already Exists** — the real solutions people currently use.
4. **Why That's Not Enough** — precisely where the existing solution stops working, and for whom.
5. **The Unimagined Response** — an approach that doesn't currently exist in mainstream form, using computation as sensing, environment, or presence rather than an app.
6. **How It Could Work** — a real technical mechanism, not hand-waving.
7. **The Hard Part** — the actual reason nobody has built this well yet.

No entry here is "AI does X for you." Several use no AI at all. The common thread is that the computer has to notice something about the physical world, a body, a relationship, or the passage of time that today's software is structurally blind to.

---

# I. The Body Nobody Is Watching

## 1. The Symptom That Only Exists in Hindsight

**The Problem.** Millions of people live with a condition — migraines, seizures, fibromyalgia flares, panic attacks, asthma, autoimmune flares — that arrives without warning and is gone by the time anyone, including a doctor, can examine it. The person is left trying to reconstruct, hours or days later, what led up to it: what they ate, how they slept, what the weather did, what stressed them. Human memory for "what was different three days before this happened" is almost worthless. Doctors, working from a patient's vague recollection, are essentially diagnosing blind.

**The Blind Spot.** The relevant data — sleep quality, barometric pressure, screen time, meals, stress, hydration, hormonal cycle, posture, noise exposure — exists in scattered form across five different apps and zero human memories, and nothing is watching all of it *continuously and correlating it automatically* before the event happens, only after.

**What Already Exists.** Symptom-diary apps (Bearable, Flaredown, migraine trackers). Wearables that log sleep and heart-rate variability. Weather apps.

**Why That's Not Enough.** Diaries require the person to remember to log, honestly, every single day, including on days nothing happens — and humans stop doing this within two to three weeks. The data streams that do exist (wearable, weather, calendar) sit in separate silos that never talk to each other, so no correlation is ever actually computed; the person is still doing the pattern-matching by eye, from memory, which is exactly the failure this was supposed to fix.

**The Unimagined Response.** A background service that requires zero daily logging from the person at all. It quietly, continuously pulls every ambient signal already being generated anyway — wearable HRV/sleep, phone accelerometer-inferred activity, ambient light and noise sensed passively by the phone microphone/light sensor, hyperlocal weather/pressure, calendar density — and only asks the person one question, once, at the actual moment of a flare: "rate this, right now." Everything else is inferred, never manually entered. Weeks later, it surfaces not a chart but a plain sentence: "your last six flares were preceded by a pressure drop of 6+ hPa within 18 hours, combined with under 6 hours of sleep — this combination hasn't occurred without a flare following it."

**How It Could Work.** A local-first daemon on the phone aggregating HealthKit/Google Fit data, a barometer/weather API, and passive ambient sensing (never recording audio content — only computing loudness/activity envelopes on-device, discarded immediately). A simple Bayesian or logistic model retrained weekly per-user, since triggers are wildly individual. The single manual input (severity, at the moment of flare) is the only label the model ever needs.

**The Hard Part.** Getting real signal out of genuinely noisy, low-n, single-subject data without either overfitting to coincidence or requiring more manual input than a diary would. This is a statistics problem before it's a software problem, and it has to be honest about uncertainty rather than presenting spurious correlations as facts — a wrong "your trigger is X" is worse than no answer at all.

---

## 2. Pain That Doesn't Show Up on a Face

**The Problem.** Chronic pain, especially invisible conditions (endometriosis, complex regional pain syndrome, rheumatoid arthritis, long COVID), routinely gets disbelieved — by employers, by family, by the medical system itself — because there's no visible evidence of it. People describe having to perform being unwell just to be taken seriously, or conversely masking pain to avoid being seen as "difficult," both of which are exhausting on top of the pain itself.

**The Blind Spot.** Pain is subjective and self-reported, and self-report is the single input every clinical pain scale relies on (the 1–10 scale), which is both the only honest source of truth and the easiest thing for others to dismiss as unreliable or exaggerated.

**What Already Exists.** Pain diary apps. The 1–10 verbal scale used in every clinic in the world.

**Why That's Not Enough.** A self-reported number, collected once every few months at an appointment, carries no evidentiary weight and captures none of the actual texture of how pain interacts with a real week — it flattens weeks of lived variation into a single digit recalled from memory in a doctor's office.

**The Unimagined Response.** Instead of trying to "prove" pain exists (an ethically fraught and probably impossible goal), build a system that captures the *behavioral signature* of pain automatically and continuously — typing rhythm and pressure changes, gait changes detected from phone accelerometer while walking, voice micro-tremor during calls, sleep fragmentation — not to diagnose, but to generate an objective, continuous timeline that sits *alongside* the person's own self-report, so a doctor sees both the number and the independently measured behavioral context around it.

**How It Could Work.** On-device inference only (privacy-critical): keystroke dynamics via existing keyboard APIs, gait analysis from phone IMU during detected walking bouts, sleep fragmentation from phone motion + optional wearable. All of it reduced to a single daily "functional variability index," never raw biometric data leaving the device, exportable as a PDF timeline for a medical appointment.

**The Hard Part.** Avoiding turning "documented proof of suffering" into a new burden — the tool must never become another thing the person has to manage or be judged by, and must resist any temptation (from insurers, employers) to be repurposed as surveillance rather than self-advocacy. The consent and data-ownership model here is as important as the sensing.

---

## 3. Care Work That Nobody Sees Happening

**The Problem.** Someone — usually a woman, usually unpaid — is managing an aging parent's twelve medications, three specialists, insurance appeals, and daily wellbeing, entirely inside their own head and a stack of sticky notes, while also working a job. This labor is enormous, constant, and almost completely invisible to everyone except the person doing it, including often to other family members who assume "someone's got it handled."

**The Blind Spot.** Caregiving coordination software is built for the *patient*, not for the invisible mental load of the *coordinator* — nothing exists to make the sheer volume and constancy of that labor visible to the rest of the family, which is precisely what causes caregiver burnout and family resentment.

**What Already Exists.** Shared calendar apps. Medication reminder apps (single-person, single-condition). CaringBridge-style update pages.

**Why That's Not Enough.** These tools coordinate logistics for one task at a time; none of them represent the *aggregate weight* of everything being juggled, so a sibling glancing at a shared calendar sees "doctor appt Tuesday" and has no idea that behind it sits four hours of insurance calls that never got a calendar entry at all.

**The Unimagined Response.** A shared household dashboard that doesn't just list tasks but visually and honestly represents *load* — every phone call, every form, every 2am worry — as an accumulating, visible weight the whole family can see, with a built-in mechanism for other family members to "pick up" a specific piece of that weight, converting invisible labor into something nameable and shareable rather than leaving it to be silently absorbed by one person.

**How It Could Work.** A quick-capture widget (voice memo or one-tap "logged a task" button) that requires near-zero friction to log *anything* — a phone call, a form, a worry — timestamped and categorized automatically via lightweight on-device classification, aggregated into a weekly "load report" visible to everyone with access, plus explicit hand-off requests other family members can claim.

**The Hard Part.** The entire value of this tool depends on the primary caregiver actually using the quick-capture consistently, which means the friction has to be genuinely near-zero — this is a product-design problem as hard as any technical one, since the target user has no spare attention to give a complicated app.

---

# II. Rooms and Spaces That Don't Know What's Happening in Them

## 4. The Elderly Person Living Alone

**The Problem.** An older adult lives alone, cognitively fine, physically capable, and fiercely wants independence — but a fall, a stroke, or simply "didn't get out of bed today and nobody knew for six hours" is a real and terrifying risk, both for them and for family who live far away. The tension is genuine: too much monitoring feels like surveillance and a loss of dignity; too little leaves real danger unaddressed.

**The Blind Spot.** Safety monitoring and privacy are currently treated as mutually exclusive — the industry's answer is a camera (constant surveillance, dignity cost) or a panic button (useless if the person is unconscious and can't press it).

**What Already Exists.** Wearable fall-detection pendants (Life Alert and similar). Camera-based "check on grandma" systems. Passive motion-sensor systems that alert if no motion is detected for X hours.

**Why That's Not Enough.** Pendants are frequently not worn (people forget, or resent the visible marker of frailty) and can't detect anything if they're on the nightstand when the fall happens. Cameras solve the detection problem but at a real cost to dignity that many elderly people rightly refuse. Simple motion-timeout sensors produce so many false alarms (a long nap, a day at a friend's house) that families tune them out.

**The Unimagined Response.** A system that senses *presence and normal activity patterns*, not identity or image — using ambient acoustic and Wi-Fi-signal-disturbance sensing (the way a body moving through a room subtly perturbs Wi-Fi signal propagation, a real and published technique) rather than a camera, learning what a *normal day* sounds and moves like for this specific person over weeks, and only alerting on genuine deviation from their own baseline — never transmitting or storing anything that could be reconstructed as an image or recording, by design, not just by policy.

**How It Could Work.** Wi-Fi Channel State Information (CSI) sensing, already the subject of real published research for through-wall presence and activity sensing, run against a per-household learned baseline (this household's normal "still moving around by 10am" pattern) rather than a fixed rule, combined with acoustic activity-envelope sensing (loudness/rhythm only, never content) as a second independent signal to reduce false alarms — an alert fires only when both signals agree something is wrong.

**The Hard Part.** CSI-based sensing is real but still research-grade in accuracy and highly sensitive to a home's specific Wi-Fi hardware and layout — getting a false-alarm rate low enough that a family actually trusts and keeps the system on is the entire product problem, not the sensing technique itself.

---

## 5. The Home That's Slowly Making Someone Sick

**The Problem.** Mold, off-gassing furniture, poor ventilation, and elevated CO2 quietly degrade health — headaches, fatigue, respiratory issues, poor sleep — for months or years before anyone connects the symptoms to the building itself, because the causal link is invisible and slow.

**The Blind Spot.** Indoor air and moisture problems are diagnosed almost entirely after the fact, once visible mold or a health crisis forces an inspection — there is no ambient, ongoing sense of a home's "health trajectory," only a single point-in-time inspection when something has already gone wrong.

**What Already Exists.** Standalone CO2/VOC monitors (Awair, uHoo). Mold test kits. Professional inspections (expensive, one-off).

**Why That's Not Enough.** Standalone monitors show a live number nobody checks daily and give no historical trend or correlation to actual health symptoms; a person sees "VOC: 340 ppb" and has no idea whether that's new, worsening, or related to why they've had headaches for two weeks.

**The Unimagined Response.** Not another dashboard number, but a system that correlates a home's slowly changing air/humidity signature against the household's *own* logged symptoms (reusing the near-zero-friction quick-capture idea from Entry 3) to surface, months later, statements like "the guest bedroom's humidity has trended up 15% since March, and every week anyone sleeps in it, someone in the house logs a headache the next morning" — treating the building itself as a patient with a health history.

**How It Could Work.** Cheap ESP32-based sensor nodes (CO2, VOC, humidity, temperature) per room, logged continuously to a local time-series database, cross-referenced against a household symptom log via straightforward correlation analysis, refreshed weekly.

**The Hard Part.** Multi-room sensor placement and calibration drift over months are genuinely fiddly hardware problems, and correlating slow environmental trends against sparse, noisy human symptom logs needs the same statistical honesty as Entry 1 — flagging real patterns without inventing false ones from a handful of data points.

---

## 6. The Sound a Deaf Person Never Gets a Second Chance To Hear

**The Problem.** Deaf and hard-of-hearing people miss doorbells, smoke alarms, a kettle boiling over, a child calling from another room, a car horn — not because they're inattentive, but because these events are one-shot audio signals with no visual or tactile equivalent in most homes, and missing the wrong one (a smoke alarm) is not a minor inconvenience, it's dangerous.

**The Blind Spot.** Captioning technology has focused almost entirely on *speech* (video captions, live-transcription apps) because that's what hearing-loss advocacy historically prioritized for communication access — but the actual daily safety and awareness gap is largely in *non-speech* environmental sound, which remains almost completely unaddressed.

**What Already Exists.** Flashing-light doorbells and smoke alarms (single-purpose, one device per sound, expensive to outfit a whole home). Live captioning apps for speech (irrelevant to this problem). Smartwatch haptic notifications tied to specific smart-home sensors (only if you already own that specific smart device).

**Why That's Not Enough.** Every existing fix requires buying a dedicated device *per sound type*, which means a household is stuck retrofitting piecemeal (a flashing doorbell here, a bed-shaker smoke alarm there) rather than getting general awareness of whatever sound happens to occur, including sounds nobody thought to buy a device for — a stranger at the back door, an unfamiliar dog barking, water running somewhere it shouldn't be.

**The Unimagined Response.** A general-purpose, always-listening (on-device only, nothing streamed anywhere) environmental sound classifier paired with a simple microphone array that gives *direction*, not just "a sound happened" — a smartwatch buzz that means "smoke alarm, kitchen direction" is categorically more useful than a single undirected buzz, because it tells the person not just that something happened but where to look.

**How It Could Work.** An open pretrained audio event classification model (e.g., architectures built on datasets like AudioSet) run entirely on-device on a small always-on hub with a 3–4 mic array for coarse direction-of-arrival estimation, paired to a smartwatch or simple wearable for haptic + text alerts, with a short on-device fine-tuning flow letting a household teach it their *own* front-door buzzer or their *own* smoke alarm's specific tone.

**The Hard Part.** General-purpose sound classification is reasonably solved in research; the actual hard problem is direction-of-arrival estimation from a cheap, small mic array with enough accuracy to be trustworthy in a real, echo-filled home, plus making per-household fine-tuning simple enough for a non-technical user to do in under a minute.

---

# III. Time, Memory, and the Things That Vanish Without a Trace

## 7. The Dead Person's Digital Half-Life

**The Problem.** When someone dies, their digital footprint doesn't die with them — a parent's Gmail keeps receiving newsletters, their Facebook keeps surfacing "memories," their subscriptions keep charging a now-frozen card, their photo library sits orphaned on a device nobody can unlock, and family members are left doing painful, bureaucratic digital-estate cleanup during the worst weeks of their grief, often for months, often hitting dead ends because they don't have passwords and platforms' "memorial" processes are inconsistent, slow, and inhumane.

**The Blind Spot.** Death is treated by almost every digital platform as an edge case handled (badly) after the fact, rather than as a planned event a person could prepare for while alive — and the tools that do exist for this (legal wills) were designed for physical property, not for the specific mess of scattered logins, recurring charges, and algorithmically-surfaced "memories" that can genuinely re-traumatize a grieving family member who wasn't expecting Facebook to remind them of their dead spouse's birthday.

**What Already Exists.** Password managers with "emergency access" features. Platform-specific "legacy contact" settings (Google, Facebook, Apple) that most people never configure. Traditional estate planning/wills, which almost never address digital accounts specifically.

**Why That's Not Enough.** Legacy-contact settings exist per-platform, are buried in settings menus almost nobody finds, cover only that one platform, and don't address the emotional-landmine problem (algorithmic memory surfacing) at all — even a fully "prepared" digital estate today is still a dozen separate, disconnected manual configurations across a dozen platforms, most of which the average person will never complete.

**The Unimagined Response.** A single, plain-language "digital afterlife plan" a person fills out once while alive — not a password vault, but a *behavioral will*: which accounts should go fully dark immediately, which should be memorialized and by whom, which recurring charges should be canceled automatically upon confirmed death, and critically, an opt-in "grief pacing" setting that tells connected platforms to suppress algorithmic memory-surfacing (birthday reminders, "on this day" posts) for a chosen mourning period rather than forever or not at all — treating the pacing of digital grief as something a person can actually plan for their survivors.

**How It Could Work.** A trusted-executor model (similar to legacy-contact features but unified across platforms via a standardized, published protocol other services could adopt) triggered by a verified death certificate upload, which then fires a pre-configured sequence of actions per account (freeze, memorialize, notify a specific person, cancel a specific subscription) rather than leaving survivors to rediscover and manually action each one.

**The Hard Part.** This is far more a policy, standards, and trust problem than a technical one — getting even a handful of major platforms to support a shared "digital estate" protocol is a multi-year undertaking, which is exactly why a single developer's realistic MVP is a personal tool (a private, encrypted instruction sheet plus automation scripts for accounts that expose an API) rather than a platform-adopted standard, at least at first.

---

## 8. The Voice That's About to Disappear

**The Problem.** As people age or develop degenerative illness (ALS, Parkinson's, dementia), their voice, their specific turns of phrase, and their way of telling a story quietly disappear — usually without anyone deliberately capturing them, because nobody thinks to record "the way Dad tells the fishing story" until it's too late and all that's left is a fading memory of a memory.

**The Blind Spot.** Recording equipment has never been the barrier — every phone has a microphone — the barrier is that nobody prompts the recording *before* it's needed, because the loss is slow and its exact moment is invisible until it's already happened.

**What Already Exists.** StoryWorth and similar "weekly prompt, get a book at the end" services. Voice-banking services for people with ALS specifically (for future synthetic speech). Simple voice memo apps.

**Why That's Not Enough.** StoryWorth produces a static book, not a searchable, livable archive, and it's opt-in enough that most families never start it until a diagnosis forces the issue, by which point the most natural, unprompted version of a person's voice and stories is already gone. Voice-banking for ALS is excellent but narrowly built for one condition and one purpose (future synthetic speech), not for the broader, quieter goal of preserving *how someone talks*, for its own sake.

**The Unimagined Response.** A near-invisible household tool — not a formal "interview," which people freeze up in front of — that passively and consensually captures natural storytelling moments (a grandparent telling the same story for the fifth time at dinner, precisely because it's the fifth time it's told slightly differently each time) with the household's explicit, ongoing awareness and control, building an archive that is searchable, annotated, and structured around the *person's own recurring stories and phrases*, rather than a single formal recording session.

**How It Could Work.** A dedicated, clearly-lit-when-active recording device (never ambient/hidden, consent has to be unambiguous and visible) placed during specific chosen moments (holiday dinners, weekly calls), on-device transcription and speaker diarization to build a searchable archive, with simple tagging ("Grandpa's fishing story, version 4") so family can find and compare retellings over years, not just hear one final recording.

**The Hard Part.** The ethics here are the load-bearing part of the design, not an afterthought — this only works if the person being recorded has full, continuous, easy control over what's kept and what's deleted, and any version of this that drifts toward passive/hidden recording of someone without robust ongoing consent (especially someone with cognitive decline, who may not reliably remember agreeing) crosses a real line and should not be built that way.

---

## 9. The File Nobody Can Find Again

**The Problem.** A person takes a photo, jots a note, or saves a document, and within a year it's functionally gone — not deleted, just unfindable, buried under folder names that made sense at the time and don't anymore, across five different devices and cloud accounts, none of which agree with each other about what exists where. Multiply this by a decade and most people are sitting on tens of thousands of digital objects they will functionally never see again.

**The Blind Spot.** File and photo organization software assumes the person will maintain a consistent, disciplined naming/folder system indefinitely — nobody does, including the people who build these tools — so the burden of imposing order is placed entirely on the human, forever, and it always eventually collapses.

**What Already Exists.** Search (file search, Google Photos search, Spotlight). Auto-organized photo libraries by date/face/location. Cloud sync across devices.

**Why That's Not Enough.** Search only works if you remember the file exists and roughly what it's called or contains — it does nothing for the vastly larger category of things you've completely forgotten you have, which is precisely the material actually being lost. Auto-organization by date/location helps browsing but still requires the person to think to browse.

**The Unimagined Response.** A background "resurfacing" layer, not a search tool — a system that periodically and quietly resurfaces genuinely forgotten material based not on relevance to a query (there is no query) but on time-based and contextual triggers: the anniversary of a photo's date, arriving at a location where old photos were taken nearby, a note written a year ago today. The computer proactively remembers *for* you, on a schedule you never have to think about, the way a physical photo album accidentally flipped open to an old page does.

**How It Could Work.** A lightweight local index of EXIF/creation metadata across all connected storage (phone, cloud drives, local disks) with a simple scheduled job that surfaces "on this day" and "near this place" candidates, presented as a small, ignorable daily card rather than a notification demanding action — closer to a memory that surfaces unbidden than a task.

**The Hard Part.** The signal-to-noise ratio is the whole game — resurfacing genuinely meaningless files (a random screenshot of a Wi-Fi password) as often as it resurfaces something meaningful trains the user to ignore it within a week, so ranking "is this worth resurfacing" well, from metadata alone with no content understanding required for privacy, is a real and unsolved ranking problem.

---

# IV. Trust, Truth, and the Erosion of Being Sure of Anything

## 10. Knowing Whether a Voice on the Phone Is Real

**The Problem.** Voice-cloning scams — a caller using a few seconds of a real person's voice, often lifted from a public social media video, to convincingly impersonate a family member in a fabricated emergency ("Mom, I've been in an accident, I need money now") — are a fast-growing and genuinely devastating fraud category, and the people most targeted (often older adults) have the least technical means to verify what they're hearing in the panicked seconds when it matters.

**The Blind Spot.** Voice has always been treated by humans as inherently trustworthy — "I'd know my daughter's voice anywhere" was true for all of human history until about eighteen months ago, and neither individual habits nor institutions have caught up to the fact that this is no longer reliably true.

**What Already Exists.** Generic advice ("agree on a family safe word"). Some banks now offer voice-print verification for their own call centers. Deepfake-detection research tools (mostly academic, not consumer-facing, not real-time).

**Why That's Not Enough.** A safe word only works if it's remembered in a moment of genuine panic, and there is no real-time consumer tool that helps a person *in the actual moment of a suspicious call* get a fast, calm answer to "is this really them" — the entire current defense is "hope you remember the workaround," which is a thin defense against a designed-to-panic-you attack.

**The Unimagined Response.** A phone-level, real-time voice authenticity signal — not a deepfake-proof guarantee, no such thing exists, but a fast, honest "confidence" indicator running locally during a call, comparing live audio characteristics against a small voluntarily-enrolled voiceprint of trusted family members, surfaced not as an alarming false-certainty verdict but as a calm, simple prompt: "this doesn't match [Name]'s enrolled voice pattern — consider calling back on a known number before doing anything."

**How It Could Work.** On-device speaker verification (a well-established, real technique distinct from and simpler than deepfake generation/detection) against a small set of voluntarily enrolled family voiceprints, running locally during live calls with zero audio ever leaving the device, triggering a simple UI prompt rather than blocking anything — this is a "second signal to consider," never a sole decision-maker.

**The Hard Part.** Being scrupulously honest about the limits of this — a confident "verified real" signal is more dangerous than no signal at all if it's ever wrong, so the entire design has to lean toward "flag uncertainty, never assert false certainty," and the tool needs to be genuinely usable by the least technical, most vulnerable users it's meant to protect, in the middle of a panic, which is an extremely high bar for interface design.

---

## 11. The Slow Loss of Being Able to Read Deeply

**The Problem.** People — including people who used to read for pleasure and now genuinely can't sustain attention on a book for more than a few pages — describe a real, felt loss of the capacity for sustained, deep reading, correlated with years of short-form, notification-interrupted digital consumption. This isn't nostalgia; it's a reported, lived change in cognitive capacity that people are distressed by and don't know how to reverse.

**The Blind Spot.** "Just read more" is the standard advice, but it assumes the capacity for sustained attention still exists and only needs exercising — for many people the actual problem is that the environment (constant notification interruption, the learned expectation of infinite scroll) has trained a specific, different attentional habit that fighting willpower alone rarely overcomes, the same way a habituated environment shapes any other behavior.

**What Already Exists.** Screen-time limit apps (App blockers, Freedom, Forest). E-readers with no notifications by design (Kindle). "Digital detox" retreats and advice.

**Why That's Not Enough.** Blocking apps addresses avoidance of distraction but does nothing to actively rebuild the specific *capacity* for sustained attention, which is closer to a trainable skill (like physical endurance) than a switch that flips back on once distractions are removed — someone who blocks their phone and picks up a novel often finds the restlessness is still there, just now unmedicated by a phone to check.

**The Unimagined Response.** Treat attention rebuilding like couch-to-5k treats physical endurance — a structured, progressive program that doesn't just remove distraction but actively, gradually retrains sustained reading capacity, starting from wherever the person's honest current baseline actually is (which might be four minutes, and that's fine), with the software's only job being to make the *next* session slightly, achievably longer than the last, based on real measured performance, not an arbitrary schedule.

**How It Could Work.** An e-reading environment that measures actual continuous reading time (via page-turn cadence and simple on-device gaze-stability heuristics from the front camera, opt-in, never recorded) as ground truth rather than trusting self-report, and adaptively sets the next session's target duration slightly above the person's real recent average — the software equivalent of a running coach, not a lecture about screen time.

**The Hard Part.** This only works if it resists every incentive of an attention-economy product — no streaks-as-guilt, no engagement-maximizing dark patterns, no gamified anxiety — which is a genuinely difficult design discipline to hold onto, since most of the industry's proven engagement techniques are precisely the mechanisms that caused this problem in the first place.

---

## 12. The Argument Nobody Can Actually Resolve Because Nobody Remembers It Right

**The Problem.** Recurring relationship conflicts — with a partner, a co-parent, a business partner — often aren't really about the current disagreement; they're fueled by both people having a confidently held, mutually incompatible memory of what was actually said or agreed to weeks or months ago, and there is no neutral record to check against, so the argument becomes about whose memory is "right," which is unresolvable and corrosive.

**The Blind Spot.** Human memory for conversation is reconstructive, not recorded — it's confidently, honestly wrong all the time, in ways people cannot detect from the inside, and relationship conflict tools (therapy, communication frameworks) all assume this away and focus on communication *style* rather than the more basic problem of a shared factual record simply not existing.

**What Already Exists.** Couples therapy and communication frameworks (Gottman Method, nonviolent communication). Shared notes/calendar apps for co-parents (OurFamilyWizard) which log *logistics*, not conversations. Text message history (only for written communication, not spoken).

**Why That's Not Enough.** Existing co-parenting/relationship tools log scheduling and formal messages but not the actual lived texture of spoken disagreements, which is where the "you said / I never said that" conflict actually lives — and nobody is going to formally minute their own arguments in real time while having them.

**The Unimagined Response.** An opt-in, both-parties-consenting "shared memory" tool used specifically for planned, difficult conversations (not passive surveillance of daily life) that records and transcribes with mutual, explicit, session-by-session consent, and afterward produces a neutral, non-editorializing summary of what was actually said and agreed — not to "win" future arguments, but so that a disagreement about *whether X was agreed to* has an actual answer to check, taking that specific fuel out of the fire.

**How It Could Work.** A simple recording session both parties explicitly start together (a visible, mutual button-press, never one-sided), on-device transcription, and a neutral summarization step that extracts stated agreements and open disagreements as plain factual bullet points, stored for both parties to access equally.

**The Hard Part.** This is dangerous if built carelessly — it must never be usable one-sided (secretly recording a partner) and must never let one party's phrasing or framing bias the "neutral summary," since a biased summary would make conflict worse, not better; the entire value proposition collapses if either party can reasonably distrust the tool's neutrality.

---

# V. The Mind Under Load

## 13. Burnout That's Invisible Until It's a Crisis

**The Problem.** Burnout — in caregivers, in high-intensity jobs, in parents of young children — builds slowly and is almost invisible to the person experiencing it, because the erosion happens gradually enough that each individual day still feels roughly normal; by the time it's undeniable (a breakdown, a resignation, a health crisis), months of warning signs have already been ignored, not out of denial exactly, but because there was never a clear enough signal to notice.

**The Blind Spot.** Burnout self-assessment tools (Maslach Burnout Inventory and similar) are periodic questionnaires taken voluntarily, usually only after someone already suspects a problem — which means the tool only fires after the person has already noticed, defeating the entire point of early detection.

**What Already Exists.** Periodic burnout questionnaires. Wellness-check-in features in some workplace apps (frequently viewed with suspicion as management surveillance). General stress-tracking wearable features (Whoop, Oura strain scores).

**Why That's Not Enough.** Wearable "strain" scores measure physiological load reasonably well but say nothing about the specific slow behavioral drift that characterizes burnout — declining response to things that used to bring joy, shrinking social contact, increasingly terse communication — which is a *pattern* visible in ordinary digital behavior, not in heart rate alone, and nothing currently looks at that pattern honestly.

**The Unimagined Response.** A strictly personal (never employer-visible — this distinction is the entire design, since workplace-visible burnout tracking would itself become a source of the stress it's meant to detect), local-only tool that watches a person's own baseline drift over months across signals they already generate — response latency to friends' messages, sleep consistency, self-reported mood on the rare days they do log something, calendar density — and says, plainly, at the point the drift crosses their own historical baseline, not a generic threshold: "Your patterns over the last six weeks look like the lead-up to your burnout in [previous logged period], if you had one. Worth a check-in with yourself."

**How It Could Work.** A local, on-device longitudinal model trained per-person on their own historical data (requiring at minimum one prior self-identified burnout period to have useful comparison data — genuinely limits usefulness for first-time detection, which is an honest constraint, not a flaw to hide), using simple statistical drift detection (change-point detection) across a handful of privacy-preserving behavioral proxies, never raw message content, ever.

**The Hard Part.** Cold-start: the tool is far less useful for someone experiencing burnout for the first time with no prior baseline to compare against, and there's no honest way around that without either guessing at generic population thresholds (much weaker signal) or waiting for the person to suffer once "on the record" first, which is an uncomfortable but real limitation to be upfront about.

---

## 14. Decisions Made Exhausted, Regretted Later

**The Problem.** Significant decisions — accepting a job offer, sending an angry email, agreeing to a major purchase, ending a relationship — are disproportionately made during windows of low sleep, high stress, or emotional flooding, and are disproportionately regretted afterward specifically because they were made in that state; the person, in the moment, has no way to know they're currently a worse decision-maker than they will be in twelve hours.

**The Blind Spot.** Nothing in a person's environment ever says "you are currently in a worse state to decide this than you will be later" — the phone that's about to let you send that email has no idea it's 1am and you haven't slept properly in four days, and even if it theoretically had that data, no product currently connects it to the specific moment of a high-stakes action.

**What Already Exists.** General "sleep on it" advice. Some email clients' "undo send" delay (seconds, not meaningfully different states of mind). Meditation/mindfulness apps unrelated to specific decision moments.

**Why That's Not Enough.** "Sleep on it" is correct advice nobody follows in the actual charged moment, precisely because being in a compromised state also compromises the judgment needed to remember and apply that advice — the gap is never information, it's a lack of any external check at the actual moment of action.

**The Unimagined Response.** A lightweight, narrowly-scoped intervention that only activates for a small, deliberately pre-defined set of genuinely high-stakes actions the person themselves opts to protect (sending an email to a specific flagged contact, a purchase over a self-set dollar amount, a message containing certain emotionally charged phrasing) and, only when the person's own measured current state (sleep debt, time of day, recent app-switching frenzy as a stress proxy) is significantly worse than their personal baseline, inserts a single calm, skippable pause: "You're running on 4 hours of sleep and it's 1:14am. This is still your call — want to send it now, or in the morning?" — never blocking, always their choice, just removing the silent assumption that right now is a neutral moment to decide.

**How It Could Work.** A small local rules engine watching for user-defined trigger actions (specific recipients, specific apps, amount thresholds) cross-referenced against a simple personal-baseline state estimate (sleep data if available, time of day, recent typing-cadence/app-switch-rate as a rough stress proxy), inserting a single non-blocking prompt only when both a trigger and a bad-state condition are met.

**The Hard Part.** The entire tool is worthless, or worse, patronizing and ignored, if it fires too often or on the wrong things — getting the trigger scope narrow and genuinely opt-in (the person chooses what counts as "high stakes" for *them*) is the actual design problem; a generic "you seem stressed, are you sure?" on every action would be exactly the kind of nagging that gets uninstalled in a day.

---

# VI. Places and People, Disconnected by Distance

## 15. The Grandchild Who's a Stranger on a Screen

**The Problem.** Grandparents who live far from young grandchildren often end up with a relationship mediated entirely through short, stilted video calls that toddlers and young children — who don't yet have the attention span or conversational skill for "talk to a screen" — actively resist, leaving the grandparent with a real, aching sense of not actually knowing this child who is growing up without them.

**The Blind Spot.** Video calling assumes both parties can hold a two-way conversation, which is precisely the skill young children don't yet have — the format itself is a poor fit for the actual relationship being attempted, not a matter of insufficient effort from either side.

**What Already Exists.** Video calling (FaceTime, WhatsApp video). Shared photo-sharing apps for family (Google Photos shared albums). Physical mailed photo books.

**Why That's Not Enough.** All of these are either synchronous and demanding (video call requiring the child's cooperation in the moment) or purely one-directional and passive (photo albums the grandparent looks at but doesn't meaningfully participate in) — nothing offers a low-pressure, asynchronous, genuinely *shared* activity a young child can do in short bursts on their own schedule that also builds a real, specific relationship with a specific distant person.

**The Unimagined Response.** A shared, physical-feeling, asynchronous "activity mailbox" — not a screen the child has to perform for, but a small tangible or near-tangible device where a grandparent can leave a short voice message, a doodle, or a simple question ("what did you find outside today?"), and the child (with a parent's help if needed) can leave a reply — a drawing on a screen, a voice note, a photo of a rock they found — on their own time, building an actual accumulating shared history the child can revisit, rather than a live call that vanishes the moment it ends.

**How It Could Work.** A simple dedicated device (or shared app running on an existing tablet) with a deliberately tiny, toddler-usable interface — big buttons, no text required to navigate — storing an ever-growing shared timeline of exchanged voice notes, drawings, and photos between exactly two specific people, explicitly not a general social feed.

**The Hard Part.** Genuinely good interface design for a pre-literate user is a real and underrated design challenge (not a technical one), and the product has to resist every temptation to add features, notifications, or "engagement" mechanics that would turn a slow, gentle relationship-building tool into just another attention-grabbing screen.

---

## 16. The Language That's Going to Die With Its Last Fluent Speakers

**The Problem.** Of the world's roughly 7,000 languages, a very large share are endangered, some down to a handful of elderly fluent speakers, and when the last fluent speaker of a language dies, an entire irreplaceable structure of thought, oral history, and cultural knowledge goes with them — usually with far less documentation than the scale of the loss warrants, because serious linguistic fieldwork is slow, expensive, academic, and reaches only a tiny fraction of at-risk languages before it's too late.

**The Blind Spot.** Language documentation has historically required a trained linguist physically present with recording equipment and years of fieldwork — a model that simply cannot scale to the number of languages currently at risk, most of which will never see a funded fieldwork project before their last speakers are gone.

**What Already Exists.** Academic linguistic documentation projects (well-funded, but cover a small fraction of at-risk languages). General-purpose translation apps (don't cover most endangered languages at all, and aren't built for documentation anyway). Community-led oral history recording efforts (valuable but usually not structured for the specific needs of linguistic preservation — grammar, not just stories).

**Why That's Not Enough.** The bottleneck is entirely about scale and access — there simply aren't enough trained linguists to reach every at-risk language community in time, and the communities themselves, who are the actual experts on their own language, usually lack the specialized tools (not the will) to document it in a linguistically useful, structured way without a linguist physically present.

**The Unimagined Response.** A guided, offline-capable self-documentation toolkit designed to be used *by the community itself*, without a linguist present, that walks a fluent elder speaker and a younger community member through a structured elicitation process (recording core vocabulary, basic grammar patterns, and natural storytelling) using a simple, translated interview protocol adapted from real linguistic fieldwork methodology — turning the linguist's role from "the only person who can do this" into "the person who designs the protocol once, for anyone to run."

**How It Could Work.** An offline-first mobile app (critical — many at-risk-language communities have poor or no connectivity) with a pre-built, linguist-designed elicitation protocol (a structured sequence proven to capture the essentials — core vocabulary, verb paradigms, natural narrative), guided audio/video recording with automatic timestamping and basic structural tagging, and a simple export format any linguist can later analyze — the app doesn't do the analysis, it makes sure the raw material is captured in a usable form before it's too late.

**The Hard Part.** This has to be built *with*, not merely *for*, the communities it serves, ideally in direct partnership with documentary linguists who understand both the methodology and the real ethical stakes (language data ownership, cultural sensitivity, who controls access to recordings of an elder telling a sacred story) — a well-meaning but community-uninformed version of this tool could genuinely cause harm, and that risk has to shape the project from day one, not be bolted on afterward.

---

# VII. What This Catalog Deliberately Leaves Out

Every entry above shares three constraints, on purpose:

- **No entry solves its problem by watching more of someone.** Several explicitly reject the obvious camera-based or content-reading version of themselves (Entry 4, 6, 10, 12) because the "just add more surveillance" answer is the *existing* weak solution's failure mode, not a fix for it.
- **No entry pretends a hard human problem becomes easy because a computer is involved.** Grief, burnout, endangered languages, and chronic illness are not solved by software; each entry tries to close one specific, real, currently-unclosed gap, honestly, without overclaiming.
- **Every "hard part" section is real, not decorative.** If an idea's hardest problem is listed as trivial, it isn't in this document — the point of this catalog is to sit with the genuinely unsolved difficulty, not to pitch around it.

This is a smaller, denser list than a typical idea catalog on purpose — sixteen problems, treated properly, are worth more than sixty features nobody needed described in one line each.
