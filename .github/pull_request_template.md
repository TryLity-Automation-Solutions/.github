<!-- Keep it short. If a section does not apply, write N/A. -->

## Checkpoints

* [ ] Breaking change (API / URL / response)
* [ ] DB change (new table / column / migration)
* [ ] Redis change
* [ ] New / changed / removed API
* [ ] Storage (R2) change
* [ ] New env variable or config
* [ ] Security / permission impact
* [ ] Deploy order matters

**Ticket:**
**Risk:** Low / Medium / High

## 1. Problem

<!-- What was wrong or missing? 1–3 lines. -->

## 2. Solution

<!-- What you changed. Mention any trade-off. -->

## 3. Database

**SQL**

| Table | Action                              | What changed or why used |
| ----- | ----------------------------------- | ------------------------ |
|       | New / Updated / Dropped / Read only |                          |

**Migration file:**

### Redis

| Key | Action                             | TTL |
| --- | ---------------------------------- | --- |
|     | Created / Updated / Deleted / Read |     |

## 4. APIs

| Method | Endpoint | Action                  | Note |
| ------ | -------- | ----------------------- | ---- |
|        |          | New / Changed / Removed |      |

## 5. Storage (R2)

* **Path pattern:**
* **Action:** Upload / Read / Delete
* **Access:** Private / Public / Signed
* **Old files or saved URLs affected:**

## 6. Deploy Notes

* **Env variables (name only):**

## 7. Developer Testing

* [ ] Main flow tested locally
* [ ] DB / Redis / R2 data checked after testing

## 8. Screenshots

<!-- Only required for UI changes. -->
