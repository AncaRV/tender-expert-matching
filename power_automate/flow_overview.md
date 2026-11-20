# Power Automate flow: Save incoming CVs

Automated cloud flow that saves CVs received by email and logs them in a Microsoft List.

## Steps

1. **Trigger: When a new email arrives (V3)** (Office 365 Outlook)
   - Folder: Inbox
   - Include Attachments: Yes
   - Only with Attachments: Yes
   - From: test sender only (so the flow never touches other emails in the test mailbox)
2. **Apply to each** attachment
   1. **Condition:** attachment name ends with `.pdf`, `.docx` or `.md`
      - **False:** do nothing (for example images or email signatures)
      - **True:**
        1. **Create file** (OneDrive for Business): folder `/Incoming CVs`, file name and content from the attachment (dynamic content)
        2. **Create share link** (OneDrive for Business): link type View, scope Organization
        3. **Create item** (SharePoint, Microsoft List `CV Register`):

| List column | Value |
| --- | --- |
| Title | Email subject |
| Sender | Email sender |
| Received | Email received time |
| File link | Web URL from Create share link |
| Status | New |
| Screening score | Empty, filled in after the AI screening |

## Design decisions

- **Filter on file type, not on the subject.** A first version only processed emails with "CV" in the subject; a CV sent with another subject would have been missed.
- **Organisation-only share link.** The first version created an anonymous link. Real CVs are personal data, so the link was restricted.
- **One row per attachment.** If an email contains several CVs, each gets its own row.

## Screenshots

See the `screenshots/` folder: the flow run with all steps successful, the OneDrive folder, and the CV Register list.
