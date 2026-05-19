# Google Sheets Repository Map

Version: v0.1.0

Use one Google Sheets file as the Carly insight repository. Retell call ID should be the unique key for call-level tabs.

## Tab: Calls

One row per Carly call.

Recommended columns:

```text
retell_call_id
processed_at
call_started_at
call_ended_at
call_duration_seconds
agent_name
customer_identifier
booking_reference
experience_booked
experience_date
group_size
group_type
occasion
survey_completed
survey_quality_score
usable_for_research
primary_booking_reason
top_decision_factors
hesitations
price_sensitivity_level
alternatives_considered
competitors_or_substitutes
follow_up_requested
priority_required
priority_level
has_complaint
has_kb_gap
has_action_items
needs_manual_review
manual_review_reason
raw_webhook_file_url
transcript_file_url
json_file_url
trend_run_id
processing_status
processing_error
```

## Tab: Decision Insights

One row per usable research call.

Recommended columns:

```text
retell_call_id
processed_at
booking_reference
primary_booking_reason
secondary_booking_reasons
most_important_decision_factors
hesitations_before_booking
what_almost_stopped_them
price_sensitivity_level
price_sensitivity_summary
decision_maker
group_influencers
group_decision_summary
decision_timeline
emotional_drivers
alternatives_considered
discovery_channels
positioning_language
customer_segment_signals
evidence_summary
```

## Tab: Follow Up

One row per follow-up need.

Recommended columns:

```text
follow_up_id
retell_call_id
created_at
booking_reference
requested_by_customer
required_by_context
preferred_channel
summary
deadline_or_timing
priority_level
owner
status
completed_at
notes
json_file_url
```

Suggested statuses:

```text
new
assigned
in_progress
waiting
done
not_needed
```

## Tab: Action Items

One row per action item.

Recommended columns:

```text
action_item_id
retell_call_id
created_at
title
description
category
priority
owner
due_timing
status
source
evidence_summary
json_file_url
```

Suggested categories:

```text
follow_up
operations
knowledge_base
customer_success
product
marketing_research
technical
other
```

## Tab: KB Gaps

One row per knowledge-base gap.

Recommended columns:

```text
kb_gap_id
retell_call_id
created_at
topic
question_or_gap
impact
recommended_kb_update
priority
status
owner
evidence_summary
json_file_url
```

Suggested statuses:

```text
new
reviewing
added_to_kb
rejected
needs_more_info
```

## Tab: Trend Runs

One row per every-5-call analysis.

Recommended columns:

```text
trend_run_id
created_at
call_count
period_start
period_end
included_retell_call_ids
top_booking_motivations
top_decision_factors
main_hesitations
price_sensitivity_summary
alternatives_summary
group_decision_summary
kb_gaps_summary
complaints_or_risks_summary
recommended_improvements
priority_actions
trend_report_file_url
whatsapp_sent_at
```

## Tab: Processing Log

One row per processing attempt.

Recommended columns:

```text
processing_log_id
retell_call_id
attempt_number
started_at
finished_at
stage
status
error_summary
raw_webhook_saved
raw_transcript_saved
json_saved
sheets_updated
whatsapp_sent
trend_triggered
raw_webhook_file_url
transcript_file_url
json_file_url
failed_run_file_url
```

Suggested statuses:

```text
started
processed
failed
retrying
skipped_duplicate
```
