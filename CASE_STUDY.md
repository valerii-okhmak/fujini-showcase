# Building Fujini

## Goal

Create a cohesive interface for Discord community operations: composing announcements, coordinating scheduled activity and configuring repeatable automation. Support multiple servers while keeping each server's content, settings and access rules distinct.

## 1. A visual editor for structured Discord messages

**Challenge:** Discord message formats have nested structure and format-specific constraints. A plain text field cannot represent the full editing workflow.

**Approach:** Combine a component tree, block-specific controls and a Discord-style preview. Keep language variants within the draft workflow and configure their destinations separately. Share content contracts across the frontend and backend.

**Tradeoff:** A custom editor gives control over the interaction model but requires ongoing validation against supported Discord message formats. The preview helps users compose content; it does not replace testing the delivered message in Discord.

## 2. Server access that reflects current permissions

**Challenge:** A successful login does not prove that someone still has permission to manage a particular server or feature.

**Approach:** Separate identity from authorization. Check current server membership and roles, then apply server-specific grants for individual users or roles. Keep authorization decisions in the backend.

**Tradeoff:** Current checks introduce a dependency on Discord API availability. Interface visibility is a convenience; backend checks remain responsible for access decisions.

## 3. Background work across two applications

**Challenge:** Dashboard actions and Gateway events arrive through different processes. Stored state must remain useful when events are missed or remote operations fail.

**Approach:** Separate API responsibilities from Gateway processing. Use BullMQ jobs with retries for guild synchronization, and periodic reconciliation for missed changes. Track publication outcomes and use a backend scheduler for due content and branding actions.

**Tradeoff:** Redis and background processing add operational responsibilities. This structure makes work easier to separate and retry, but it is not a claim of exactly-once delivery or unlimited horizontal scalability.

## 4. Coordinating scheduled activity

**Challenge:** Content, branding and collection windows need one understandable view of when changes will happen.

**Approach:** Bring scheduled items into a calendar with a month view and chronological agenda, using the configured server time zone. Link items back to their editors and display execution state.

**Tradeoff:** The interface must keep time-zone presentation consistent with persisted timing and scheduler behavior. Additional real-server validation remains part of release preparation.

## Next milestones

- Continue end-to-end testing on Discord test servers, including permission failures and delivery outcomes.
- Complete payment integration and subscription lifecycle handling.
- Prepare production deployment, monitoring and operational procedures.
- Revisit a public demonstration environment with isolated sample data.

The project is presented as work in development. Screenshots show demonstration data; they are not evidence of production adoption or measured performance.
