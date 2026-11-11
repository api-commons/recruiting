# Recruiting

The common recruiting vocabulary -- the jobs a company opens, the candidates who apply, their applications as they move through stages, the interviews and offers along the way -- distilled from the APIs that already exist. Part of [Schema Commons](https://github.com/api-commons/schema-commons),
published by the [API Commons](https://apicommons.org).

Built from the providers tagged Recruiting, Hiring, Applicant Tracking, Applicant Tracking System, ATS, Talent Acquisition, Recruitment in [APIs.io](https://apis.io) that
publish their own OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| job | 32 | 26 | id 81%, name 81%, status 69%, description 65%, created_at 62%, location 54%, employment_type 50% | jobs 20, positions 6, postings 4, openings 3 |
| candidate | 23 | 19 | id 84%, phone 84%, email 79%, first_name 68%, last_name 68%, created_at 63%, resume_url 53% | candidates 17, applicants 7, people 2, profiles 2 |
| application | 15 | 11 | id 91%, job_id 73%, candidate_id 64%, metadata 64%, updated_at 64%, created_at 55%, source 55%, status 55% | applications 15, application 1 |
| stage | 9 | 6 | id 83%, name 50%, updated_at 50% | stages 6, stage 2, pipeline-stages 1 |
| interview | 8 | 7 | id 100%, created_at 71%, start_at 71%, status 71%, application_id 57%, end_at 57%, location 57% | interviews 6, scheduled-interviews 1, scheduled_interviews 1, interview 1 |

Cohort: 67 providers with harvested OpenAPI, 32 crosswalks. Of the providers exposing a job: 59% create, 100% list, 84% fetch, 53% update, 31% delete. Generated 2026-11-10 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## What the measurement shows

- Candidates agree well: name, email, phone, resume and created_at are all core, so a candidate moves between applicant-tracking systems with little loss.
- Applications agree on the join itself -- candidate, job, status, source -- which is what every ATS integration actually needs.
- Interviews agree on start, end, location and the application they belong to; Greenhouse's scheduled_interviews set the shape.

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments and
  their qualifiers, aliases, reviewed exclusions, merged duplicates, and the decisions below.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- A job is a posting or requisition: `/jobs` (and postings, openings, requisitions, vacancies, positions in this cohort). In the HR context the same word is an employee's job record, which is why HR uses `/employments`.
- `/positions` that are an employee's position history (Harri, HR Partner) are HR, not recruiting, and excluded by name.
- Offers were measured and dropped: three providers expose them as a resource.
- Everything but the job `requires: [job]`, so a provider's `/people` or `/profiles` counts as candidates only if it also posts jobs.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
