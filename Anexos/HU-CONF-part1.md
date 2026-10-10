# HU-CONF-001: Configure system alerts

## Story

**As** an authenticated user  
**I want** to configure the alerts and notifications of the application  
**So that** I can customize how the system informs me about relevant events such as attendance records, justifications, and system alerts.

---

## Acceptance criteria

> Format: "Given [context/initial state], when [user action], then [expected and verifiable result]."

- [ ] **AC1:** Given that the user is registered and has logged into the system, when the user accesses the alert configuration module, then the system displays the available alert configuration options.

- [ ] **AC2:** Given that the user modifies one or more alert configuration options, when the changes are saved, then the system stores the new configuration and reflects the changes in the system's behavior.

- [ ] **AC3:** Given that the user is not registered or has not logged into the system, when the user attempts to access the alert configuration module, then the system denies access and shows a clear authentication error message.

- [ ] **AC4:** Given that the alert configuration is saved successfully, when the system processes the changes, then the system shows a confirmation message indicating that the configuration was updated.

- [ ] **AC5:** Given that the alert configuration fails to save due to a system error, when the operation is processed, then the system shows a clear error message and does not apply the changes.

- [ ] **AC6:** Given that the user's alert configuration is updated, when a relevant event occurs in the system, then the system sends notifications according to the user's configured preferences.

- [ ] **AC7:** Given normal network conditions, when the user saves the alert configuration, then the system responds in less than 5 seconds, according to RNF 1.

- [ ] **AC8:** Given that the alert configuration is updated, when the operation is completed, then the system records an audit event containing the user, date, and time.

---

## Technical notes

- RF 4.1 indicates that the system must validate that the user is registered and has logged into the system before allowing alert configuration.
- This story covers the general alert configuration module. Specific sub-options such as enabling/disabling alerts and changing notification tones are handled in separate stories (HU-CONF-002 and HU-CONF-003).
- The alert configuration should be stored per user to allow personalization.
- The system should consider the following types of alerts:
  - Attendance records (entry/exit).
  - Justification results (approved/rejected).
  - System alerts (device failures, maintenance).
  - Academic alerts (delays, absences).
- The alert configuration should be compatible with the notification delivery mechanism defined in HU-JUS-003.
- The system must validate that the user is authenticated before processing any configuration change.
- Configuration changes must be auditable for traceability.
- This story does not cover IoT device failure alerts. That is handled in HU-CONF-007.
- The team must define the default alert configuration for new users.

**Responsible service(s):** Configuration service / Notification service, proposed.  
**Endpoint(s) implemented:**  
- `GET /api/v1/config/alerts` — proposed, returns the user's current alert configuration.  
- `PUT /api/v1/config/alerts` — proposed, updates the user's alert configuration.  

**Events generated:**  
- `AlertConfigurationUpdated`  
- `AlertConfigurationFailed`  

**Required permissions:**  
- Authenticated user (any role).  
- Configuration is personal and applies only to the authenticated user's notifications.

---

## Definition of Done (DoD)

> This HU can only be closed when it meets the team's full DoD.

**Additional checks specific to this HU:**

- [ ] Alert configuration module is accessible to authenticated users.
- [ ] Successful configuration update tested.
- [ ] Unauthenticated access attempt tested.
- [ ] Confirmation and error messages are clear.
- [ ] Configuration is stored per user.
- [ ] Notifications are sent according to configured preferences.
- [ ] Audit event is generated.
- [ ] Response time is validated under normal conditions and remains under 5 seconds.
- [ ] Traceability to RF 4.1 is documented.

---

## Estimation and priority

| Field | Value |
|---|---|
| Story Points | 3 |
| Priority | Medium |
| Target sprint | Sprint 3 |
| Dependencies | HU-IAM-001: User login |

---

# HU-CONF-002: Enable or disable alerts

## Story

**As** an authenticated user  
**I want** to enable or disable specific alerts and notifications  
**So that** I can control which notifications I receive and reduce unnecessary interruptions.

---

## Acceptance criteria

> Format: "Given [context/initial state], when [user action], then [expected and verifiable result]."

- [ ] **AC1:** Given that the user is registered and has logged into the system, when the user accesses the alert enable/disable module, then the system displays the list of available alerts with their current enabled/disabled status.

- [ ] **AC2:** Given that the user selects an alert to enable, when the change is saved, then the system enables the selected alert and reflects the change in the system's behavior.

- [ ] **AC3:** Given that the user selects an alert to disable, when the change is saved, then the system disables the selected alert and limits the notifications the user receives for that alert type.

- [ ] **AC4:** Given that the user is not registered or has not logged into the system, when the user attempts to enable or disable alerts, then the system denies access and shows a clear authentication error message.

- [ ] **AC5:** Given that the alert status is changed successfully, when the system processes the change, then the system shows a confirmation message indicating that the alert was enabled or disabled.

- [ ] **AC6:** Given that an alert is disabled, when the corresponding event occurs in the system, then the system does not send notifications for that alert type to the user.

- [ ] **AC7:** Given that an alert is enabled, when the corresponding event occurs in the system, then the system sends notifications for that alert type to the user according to their configured preferences.

- [ ] **AC8:** Given normal network conditions, when the user enables or disables an alert, then the system responds in less than 5 seconds, according to RNF 1.

- [ ] **AC9:** Given that the alert status is changed, when the operation is completed, then the system records an audit event containing the user, alert type, new status, date, and time.

---

## Technical notes

- RF 4.1.1 indicates that the system must validate that the user is registered and has logged into the system before allowing alert enable/disable operations.
- This story specifically covers the enable/disable toggle for individual alerts, complementing the general configuration module in HU-CONF-001.
- The alert types that can be enabled or disabled should align with the alert categories defined in HU-CONF-001:
  - Attendance records (entry/exit).
  - Justification results (approved/rejected).
  - System alerts (device failures, maintenance).
  - Academic alerts (delays, absences).
- The system should consider a master toggle to enable or disable all alerts at once, as well as individual toggles for each alert type.
- When an alert is disabled, the system must not send notifications for that alert type, but should still log the event for audit purposes.
- The alert status should be stored per user to allow personalization.
- The team must define whether critical alerts (such as security incidents) can be disabled by users or should always remain enabled.
- This story does not cover changing notification tones. That is handled in HU-CONF-003.

**Responsible service(s):** Configuration service / Notification service, proposed.  
**Endpoint(s) implemented:**  
- `GET /api/v1/config/alerts/status` — proposed, returns the enabled/disabled status of each alert.  
- `PATCH /api/v1/config/alerts/{alertType}/status` — proposed, enables or disables a specific alert.  

**Events generated:**  
- `AlertEnabled`  
- `AlertDisabled`  
- `AlertStatusChangeFailed`  

**Required permissions:**  
- Authenticated user (any role).  
- Configuration is personal and applies only to the authenticated user's notifications.

---

## Definition of Done (DoD)

> This HU can only be closed when it meets the team's full DoD.

**Additional checks specific to this HU:**

- [ ] Alert enable/disable module is accessible to authenticated users.
- [ ] Successful alert enable tested.
- [ ] Successful alert disable tested.
- [ ] Unauthenticated access attempt tested.
- [ ] Notifications are not sent for disabled alerts.
- [ ] Notifications are sent for enabled alerts.
- [ ] Master toggle (if implemented) tested.
- [ ] Audit event is generated.
- [ ] Confirmation and error messages are clear.
- [ ] Response time is validated under normal conditions and remains under 5 seconds.
- [ ] Traceability to RF 4.1.1 is documented.

---

## Estimation and priority

| Field | Value |
|---|---|
| Story Points | 2 |
| Priority | Low |
| Target sprint | Sprint 3 |
| Dependencies | HU-IAM-001: User login; HU-CONF-001: Configure system alerts |

---

# HU-CONF-003: Change notification tone

## Story

**As** an authenticated user  
**I want** to change the tone or sound of the application's notifications  
**So that** I can customize the audio feedback I receive when the system alerts me about relevant events.

---

## Acceptance criteria

> Format: "Given [context/initial state], when [user action], then [expected and verifiable result]."

- [ ] **AC1:** Given that the user is registered and has logged into the system, when the user accesses the notification tone configuration module, then the system displays the list of available notification tones.

- [ ] **AC2:** Given that the user selects a notification tone from the available options, when the change is saved, then the system stores the selected tone and applies it to the user's notifications.

- [ ] **AC3:** Given that the user selects a notification tone, when the user wants to preview the sound, then the system allows playing a preview of the selected tone before saving.

- [ ] **AC4:** Given that the user is not registered or has not logged into the system, when the user attempts to change the notification tone, then the system denies access and shows a clear authentication error message.

- [ ] **AC5:** Given that the notification tone is changed successfully, when the system processes the change, then the system shows a confirmation message indicating that the tone was updated.

- [ ] **AC6:** Given that a notification is triggered after the tone change, when the notification is delivered, then the system plays the newly selected tone.

- [ ] **AC7:** Given that the user does not select a tone or cancels the operation, when the module is closed, then the system retains the previous tone without applying changes.

- [ ] **AC8:** Given normal network conditions, when the user changes the notification tone, then the system responds in less than 5 seconds, according to RNF 1.

- [ ] **AC9:** Given that the notification tone is changed, when the operation is completed, then the system records an audit event containing the user, selected tone, date, and time.

---

## Technical notes

- RF 4.1.2 indicates that the system must validate that the user is registered and has logged into the system before allowing notification tone changes.
- This story specifically covers the audio customization of notifications, complementing the general configuration module in HU-CONF-001.
- The system should provide a predefined set of notification tones for the user to choose from. The team must define the available tones.
- The system should consider:
  - A default tone for new users.
  - A "silent" or "vibration only" option, if applicable.
  - Preview functionality before saving.
- The notification tone setting should be stored per user to allow personalization.
- On mobile devices, the application must request appropriate permissions to play sounds.
- On web browsers, the system should handle browser autoplay policies that may restrict audio playback.
- The tone change should apply to all notification types unless the team decides to allow per-alert-type tone configuration.
- This story does not cover enabling/disabling alerts. That is handled in HU-CONF-002.

**Responsible service(s):** Configuration service / Notification service, proposed.  
**Endpoint(s) implemented:**  
- `GET /api/v1/config/notification-tone` — proposed, returns the user's current notification tone.  
- `PUT /api/v1/config/notification-tone` — proposed, updates the user's notification tone.  

**Events generated:**  
- `NotificationToneUpdated`  
- `NotificationToneChangeFailed`  

**Required permissions:**  
- Authenticated user (any role).  
- Configuration is personal and applies only to the authenticated user's notifications.

---

## Definition of Done (DoD)

> This HU can only be closed when it meets the team's full DoD.

**Additional checks specific to this HU:**

- [ ] Notification tone module is accessible to authenticated users.
- [ ] Available tones are displayed correctly.
- [ ] Successful tone change tested.
- [ ] Preview functionality tested.
- [ ] Unauthenticated access attempt tested.
- [ ] New tone is played on subsequent notifications.
- [ ] Cancel operation retains previous tone.
- [ ] Mobile permissions handling tested.
- [ ] Web browser autoplay policy handling tested.
- [ ] Audit event is generated.
- [ ] Confirmation and error messages are clear.
- [ ] Response time is validated under normal conditions and remains under 5 seconds.
- [ ] Traceability to RF 4.1.2 is documented.

---

## Estimation and priority

| Field | Value |
|---|---|
| Story Points | 2 |
| Priority | Low |
| Target sprint | Sprint 3 |
| Dependencies | HU-IAM-001: User login; HU-CONF-001: Configure system alerts |

---

# HU-CONF-004: Configure application language

## Story

**As** an authenticated user  
**I want** to configure the language of the application  
**So that** I can interact with the system in my preferred language and improve my user experience.

---

## Acceptance criteria

> Format: "Given [context/initial state], when [user action], then [expected and verifiable result]."

- [ ] **AC1:** Given that the user is registered and has logged into the system, when the user accesses the language configuration module, then the system displays the list of available languages.

- [ ] **AC2:** Given that the user selects a language from the available options, when the change is saved, then the system stores the selected language and applies it to the user's interface.

- [ ] **AC3:** Given that the language is changed successfully, when the system processes the change, then the system shows a confirmation message in the newly selected language.

- [ ] **AC4:** Given that the user is not registered or has not logged into the system, when the user attempts to change the language, then the system denies access and shows a clear authentication error message.

- [ ] **AC5:** Given that the user selects a language, when subsequent screens and messages are displayed, then all interface texts, labels, and messages are shown in the selected language.

- [ ] **AC6:** Given that the user does not select a language or cancels the operation, when the module is closed, then the system retains the previous language without applying changes.

- [ ] **AC7:** Given that a new user registers in the system, when no language preference has been set, then the system uses the default language defined by the team.

- [ ] **AC8:** Given normal network conditions, when the user changes the language, then the system responds in less than 5 seconds, according to RNF 1.

- [ ] **AC9:** Given that the language is changed, when the operation is completed, then the system records an audit event containing the user, selected language, date, and time.

---

## Technical notes

- RF 4.2 indicates that the system must validate that the user is registered and has logged into the system before allowing language configuration.
- This story supports the non-functional requirement RNF 2 (Disponibilidad), which states that the interface should support different languages.
- The team must define the available languages. Proposed initial languages:
  - Spanish (default).
  - English.
- The system should consider implementing internationalization (i18n) best practices:
  - Externalize all user-facing strings into translation files.
  - Use locale-based formatting for dates, times, and numbers.
  - Support right-to-left (RTL) languages if applicable in the future.
- The language preference should be stored per user to allow personalization.
- The system should apply the language change immediately without requiring a logout/login cycle, if feasible.
- Error messages, validation messages, and notification texts should also be translated.
- The system should consider a fallback language (default) in case a translation is missing.
- This story does not cover color palette configuration. That is handled in HU-CONF-005.
- The team must define whether the language configuration applies only to the web interface or also to mobile notifications and emails.

**Responsible service(s):** Configuration service / Internationalization service, proposed.  
**Endpoint(s) implemented:**  
- `GET /api/v1/config/language` — proposed, returns the user's current language.  
- `PUT /api/v1/config/language` — proposed, updates the user's language.  
- `GET /api/v1/config/available-languages` — proposed, returns the list of available languages.  

**Events generated:**  
- `LanguageUpdated`  
- `LanguageChangeFailed`  

**Required permissions:**  
- Authenticated user (any role).  
- Configuration is personal and applies only to the authenticated user's interface.

---

## Definition of Done (DoD)

> This HU can only be closed when it meets the team's full DoD.

**Additional checks specific to this HU:**

- [ ] Language configuration module is accessible to authenticated users.
- [ ] Available languages are displayed correctly.
- [ ] Successful language change tested.
- [ ] Interface texts are displayed in the selected language.
- [ ] Confirmation message is shown in the new language.
- [ ] Unauthenticated access attempt tested.
- [ ] Cancel operation retains previous language.
- [ ] Default language is applied for new users.
- [ ] Error and validation messages are translated.
- [ ] Date and time formatting adapts to the selected locale.
- [ ] Audit event is generated.
- [ ] Response time is validated under normal conditions and remains under 5 seconds.
- [ ] Traceability to RF 4.2 is documented.

---

## Estimation and priority

| Field | Value |
|---|---|
| Story Points | 3 |
| Priority | Medium |
| Target sprint | Sprint 3 |
| Dependencies | HU-IAM-001: User login |

---

# HU-CONF-005: Update color palettes

## Story

**As** an administrator  
**I want** to configure and add color palettes for the application interface  
**So that** the institution's visual identity is reflected in the system and the user experience is customized according to institutional criteria.

---

## Acceptance criteria

> Format: "Given [context/initial state], when [user action], then [expected and verifiable result]."

