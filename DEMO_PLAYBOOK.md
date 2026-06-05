# Teladoc HHS Demo — Build & Run Playbook

Audience: Nav (PM, HHS hospital division) + Belén (UX, hospital sites). They are vetting
guides/tooltip vendors and want to ride the corporate renewal. Every guide below maps to a
requirement Nav stated on the intro call.

---

## Part 1 — Get the app live (~20 min)

1. Open `index.html`, replace `REPLACE_WITH_YOUR_DEMO_ORG_KEY` with your **demo org** API key
   (Settings → Projects → API key). Set `SERVER_ZONE` if your demo org is EU.
2. **Verify the Engagement snippet.** The file loads the standard CDN script and calls
   `amplitude.add(window.engagement.plugin())`. Compare against the exact snippet in your org:
   *Guides and Surveys → Settings → Installation* — paste theirs over mine if it differs.
3. Deploy: drag the folder into https://app.netlify.com/drop (or `vercel deploy`). Present from
   the real URL, not localhost — fewer surprises with the visual editor and looks better on screen.
4. Open the site, check the bottom strip says **"Amplitude: connected"**, click around, and confirm
   events arrive in the demo org (Dashboard Viewed, Role Selected, Virtual Visit Started…).
   Disable ad-blockers in the browser profile you'll present from.

## Part 2 — Build these 3 guides + 1 survey (~40 min)

All key elements carry stable `data-guide-target` attributes — use them as selectors in the
visual editor instead of auto-generated CSS paths.

### A. First-login walkthrough (their #1 requirement: replace manuals)
- Type: multi-step walkthrough, 3 steps max:
  1. `[data-guide-target="kpi-row"]` — "Your shift at a glance."
  2. `[data-guide-target="patient-queue"]` — "Patients ready for virtual visits appear here."
  3. `[data-guide-target="start-visit"]` — "Start a visit in one click."
- Targeting: new users / first session.
- Live re-fire on stage: click **Reset first-visit state** in the bottom strip → the app reloads
  with a fresh user ID, so the "new user" condition matches again. Rehearse this once.

### B. Tooltip/hotspot on a missed feature
- Anchor: `[data-guide-target="note-templates"]`.
- Copy: "Note templates cut documentation time — most coordinators never find this."
- Talking point: built in the Amplitude UI, no code deploy, no release train.

### C. Role-based guide (Nav asked for this verbatim)
- Duplicate guide B (or make a small banner) targeted to user property `role = physician` only.
- On stage: flip the Role dropdown from Care Coordinator → Physician. The app sends an
  `identify` with the new role; the physician-only guide appears. Ten seconds, requirement proven.
- Note: depending on session evaluation timing, a reload after switching roles is the reliable
  path — rehearse which behavior your org shows and script accordingly.

### D. Micro-survey
- Anchor near `[data-guide-target="survey-anchor"]` or trigger after the `Virtual Visit Started`
  event: 1 question, "How easy was it to start this visit?" (CES 1–5).
- Talking point: non-invasive, in-context, replaces their stale feedback channels.

### The closer: one data model
After running the guides, switch to Amplitude and show guide-interaction events sitting in the
same project as `Virtual Visit Started` — build a 2-step funnel (Guide Completed → Virtual Visit
Started) live if time allows. This is the thing point-solution vendors can't show.

## Part 3 — Demo beats (the 25-min Guides block)

1. 2 min — "This is a stand-in for your console" (call out it's fictitious data, demo env chip is visible).
2. 5 min — First-login walkthrough fires (use the reset button beforehand, off-screen).
3. 4 min — Tooltip on Note templates; show how it was built in the visual editor.
4. 4 min — Role switch → physician-only guide appears. Mention localization: guides support
   multi-language variants; the EN/ES toggle sets a `language` user property you can target on.
5. 4 min — Survey fires after starting a visit.
6. 5 min — Flip to Amplitude: guide events + product events in one funnel; guide performance dashboards.
7. Close on HIPAA/enterprise slide → next steps → renewal timing (end of June — create urgency politely).

## Part 4 — The trust-builder for the 30-min Amplitude half

Nav said his engineers *hypothesize* instrumentation may already carry over because two front
ends share a backend, and he can't confirm it. Show him exactly how to check:
**Data → Sources** (which SDKs/keys are sending), then **Events stream filtered by platform or
project**. If you solve that live, you've delivered real value before the sales motion even starts.

## Gotchas checklist

- [ ] Demo org key only — never Teladoc's production org.
- [ ] Ad-blocker off in the presenting browser profile.
- [ ] Guides published (not draft) and targeting verified with a preview link as backup.
- [ ] Rehearse the reset-button → walkthrough sequence once end-to-end.
- [ ] Have the Netlify URL ready to share — Nav will want to click around after the call.
- [ ] Screenshot fallback of each guide in your deck appendix in case the live demo misbehaves.
