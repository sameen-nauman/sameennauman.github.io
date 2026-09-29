# [DZ EG] PAA — Add `completion_status` field to Properties for Sale and Buildings, Lands & Other

**Issue type:** Story · **Priority:** High · **Market (customfield_11941):** EG · **Requested by (customfield_12253):** Product
**Context:** Slack, New Projects thread (#C09CJLPLV4K): https://dubizzle.slack.com/archives/C09CJLPLV4K/p1787067355973119 · Parent: [New projects](https://app.clickup.com/t/3692463/86exywy7w)

## WHAT
Add a `completion_status` category field (**Ready** / **Off-Plan**) to the Post-an-Ad form for the `properties-for-sale` and `buildings-lands-other` categories. It has to be saved on the ad and be filterable.

## WHY
- The New Projects "project ads" LPV must show only off-plan units. It filters on `completion_status = off-plan` together with the existing location filter.
- Right now these categories have no such field, so neither the app nor BE can apply the filter. This is blocking the New Projects build.

## SCOPE
Portal: dubizzle-eg | Surfaces: PAA (post and edit ad), ad data/index, category fields API | Area: Properties

## ENHANCEMENTS
### 1. `completion_status` field in PAA
**User stories:**
- As a seller or agency posting a property for sale, I want to say whether it is ready or off-plan, so that buyers looking for new projects can find it.
- As a buyer on the New Projects LPV, I want to see only off-plan units, so that the results match what I'm looking for.

**Change:**
- New single-select field **Completion Status** on the PAA form for every sub-category under `properties-for-sale` and `buildings-lands-other`.
- Options: `Ready` (`ready`) and `Off-Plan` (`off-plan`).
- The field appears in both the post and edit ad flows.
- The value is saved on the ad and returned in the category fields API, so it can be used as a filter (attribute `completion_status`).

**AC:**
1. Given a user is posting an ad in any `properties-for-sale` or `buildings-lands-other` sub-category, when the details step loads, then a **Completion Status** field with options **Ready** and **Off-Plan** is shown.
2. Given a user picks a value and submits, when the ad goes live, then the selected `completion_status` is saved on the ad and returned in the ad detail response.
3. Given a user edits an existing ad in these categories, when the form opens, then the saved value is pre-selected and can be changed.
4. Given the category fields API is requested for these categories, when it responds, then it includes `completion_status` with its choices.
5. Given the listings search is filtered by `completion_status = off-plan` plus a location, when results come back, then only off-plan ads in that location are returned.
6. Given a user posts in any other category, then the field is not shown.

**Copy:**
- Field label: "Completion Status" / AR: "حالة الإنجاز"
- Options: "Ready" / "جاهز" · "Off-Plan" / "على الخريطة"
- Validation (if made mandatory): "Please select completion status" / "يرجى اختيار حالة الإنجاز"

**Edge cases:**
- Existing ads with no value: not returned by the `off-plan` filter, and shown with the field empty when edited. No backfill in this ticket.
- Ads posted through bulk/feed or the agency portal without the field: saved without a value, not rejected.
- The field is **optional** at launch, so the posting flow and conversion are not affected. Making it mandatory is a separate decision.

**Suggested technical implementation (optional, non-binding):**
- Follow the existing Bayut `completion_status` attribute (same key and values) so the New Projects app filter works unchanged.
- Clients must read choices from the live categoryFields API rather than hard-coding them.

## SUCCESS METRICS
| Metric | Baseline | Target | Timeframe | Owner |
|---|---|---|---|---|
| % of new ads in the two categories with `completion_status` filled | 0% (field doesn't exist) | ≥ 60% | 60 days after release | Sameen Nauman Janjua |

## ANALYTICS
- No new events. The field value goes into the existing PAA submit event's ad attributes.

## BLOCKERS
None. Categories confirmed by product in the thread (29 Sep 2026).

---
*Spec authored by horizontals-product-agent v1.2.0 · 2026-09-29*
