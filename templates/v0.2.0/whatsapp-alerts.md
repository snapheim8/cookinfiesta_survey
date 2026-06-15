# WhatsApp Alert Templates

Version: v0.2.0

## Template 1: Escalated Conversation

```
🔔 Carly Escalation Alert

A customer conversation was escalated.

Channel: {channel}
Time: {timestamp}
Reason: {escalation_reason}

Summary: {summary}

Action needed: review and follow up with the customer.
```

## Template 2: Unanswered Question

```
❓ Carly Knowledge Gap

Carly could not answer a customer question.

Channel: {channel}
Time: {timestamp}
Question(s):
{unanswered_questions}

Consider adding this information to the knowledge base.
```

## Template 3: Combined (escalated + unanswered)

```
🔔 Carly Alert — Escalation + Knowledge Gap

Channel: {channel}
Time: {timestamp}
Escalation reason: {escalation_reason}

Unanswered question(s):
{unanswered_questions}

Summary: {summary}

Action needed: follow up with customer and update knowledge base.
```

## Notes

- Use Template 1 when `escalated == true` and `unanswered_questions` is empty
- Use Template 2 when `escalated == false` and `unanswered_questions` is not empty
- Use Template 3 when both conditions are true
- All variables in `{braces}` are populated from the extraction output
