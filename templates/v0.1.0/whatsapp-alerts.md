# Carly WhatsApp Alert Templates

Version: v0.1.0

Use these templates in n8n after the per-call extraction is complete. Keep WhatsApp messages short and avoid unnecessary personal details.

## Standard Post-Call Alert

```text
Carly call processed

Customer: {{customer_display_name_or_unknown}}
Booking: {{booking_reference_or_unknown}}
Call: {{retell_call_id}}
Quality: {{survey_quality_label}} / {{quality_score}}

Why they booked:
{{primary_booking_reason_or_unknown}}

Most important factors:
{{top_decision_factors}}

Hesitations:
{{hesitations_or_none}}

Follow-up needed: {{yes_or_no}}
Priority: {{priority_level}}

JSON: {{json_file_url}}
Sheet: {{sheet_row_url}}
```

## Priority Alert

```text
Priority Carly insight

Priority: {{priority_level}}
Reason: {{priority_reason}}

Customer: {{customer_display_name_or_unknown}}
Booking: {{booking_reference_or_unknown}}
Call: {{retell_call_id}}

Issue:
{{priority_summary}}

Recommended next step:
{{recommended_next_step}}

Follow-up requested: {{follow_up_requested_yes_or_no}}
Owner: {{owner_or_unassigned}}

JSON: {{json_file_url}}
Sheet: {{sheet_row_url}}
```

## Trend Report Alert

```text
Carly 5-call trend report ready

Trend run: {{trend_run_id}}
Calls analyzed: {{call_count}}
Period: {{period_start}} to {{period_end}}

Top pattern:
{{top_pattern}}

Main hesitation:
{{main_hesitation}}

Action needed:
{{most_important_action_or_none}}

Report: {{trend_report_file_url}}
Sheet: {{trend_sheet_url}}
```

## Processing Failure Alert

```text
Carly processing failed

Call: {{retell_call_id_or_unknown}}
Stage: {{failed_stage}}
Error: {{short_error_summary}}

Raw files saved: {{raw_files_saved_yes_or_no}}
Retry needed: yes

Failed run: {{failed_run_file_url_or_unknown}}
```

## Template Field Notes

- `customer_display_name_or_unknown`: Use first name only when available. Otherwise use `unknown`.
- `quality_score`: Format as a percentage or decimal consistently.
- `top_decision_factors`: Limit to 3 short bullets or a comma-separated list.
- `priority_reason`: Use the strongest matching reason from `analysis_flags`.
- `recommended_next_step`: Pull from the highest-priority action item when available.
- `json_file_url`: Google Drive link to the extracted JSON.
- `sheet_row_url`: Google Sheets row or sheet link.
