# Contribution 1: Improve the UX of user creation for secondary user stores from the console app.

**Contribution Number:** 1 
**Student:** Vismay Igur  
**Issue:** https://github.com/wso2/product-is/issues/21325
**Status:** Phase II Complete

---

## Why I Chose This Issue

I chose this issue because it focuses on a real UX problem where a small design flaw can cause users to make mistakes, like accidentally adding a user to the wrong user store. I thought this was interesting because the issue is not just about fixing a bug, but about improving the user experience so the application behaves more intuitively. Since user creation and identity management are important parts of many applications, making this flow clearer and safer feels like a meaningful contribution.

This issue also matches my skills and learning goals because it involves frontend/UI work, user flows, and understanding how application state or context should be handled when creating a user. I want to improve my ability to work in a larger open-source codebase and learn how production applications organize frontend logic around forms, modals, and user management features. By contributing to this issue, I hope to gain more experience with fixing UX problems, reading existing code, and making a change that helps prevent real users from making avoidable mistakes.

---

## Understanding the Issue

### Problem Description

The user-store context is not fully preserved in the add-user flow. When an admin is working in a secondary user store and creates a new user, the UI does not consistently keep that selected store after the flow completes.

### Expected Behavior

If the admin starts the add-user flow while viewing users in a secondary user store, the UI should keep that same user-store context throughout the flow and return to the users page with the same secondary store still selected.

### Current Behavior

The issue appears to be partially fixed because the add-user flow can open with the correct secondary user store selected. However, after the user is created, the users page resets back to the Primary user store instead of staying on the previously selected secondary store.

### Affected Components

The affected code is most likely in the Console frontend, specifically the users management flow in the identity-apps repo. The main areas involved are:

the users list page and its selected user-store filter state
the add-user flow or modal
the success navigation or redirect after user creation
---

## Reproduction Process

### Environment Setup

I first tried to run the project locally from product-is, but ran into a few setup issues. I did not have Maven installed, so instead of building from source, I used the prebuilt WSO2 Identity Server distribution zip. After that, I hit Java setup problems: JAVA_HOME was not configured correctly at first, and then I found that the downloaded server required JDK 21 or higher while my machine only had JDK 18. After updating Java and setting the correct JAVA_HOME, I was able to run the server and access the Console.

Another challenge was figuring out where user-store management lived in the current UI, since it was not under a separate top-level User Stores menu. In this version, it was under User Attributes & Stores -> User Stores.

I also ran into a separate issue while trying to create and use a secondary database user store for a full end-to-end repro. Even after updating the saved regex values for the store, the create-user form continued rejecting clearly valid usernames like user1234. Because of that, I could not cleanly complete the full create-user flow for the secondary store, and I treated that as a separate issue from the one I am planning to fix.

### Steps to Reproduce

1. Run the WSO2 Identity Server distribution locally and open the Console.
2. Create a secondary user store such as SEC.
3. Go to User Management -> Users and switch the selected user store to the secondary store.
4. Open the add-user flow.
5. Based on the issue discussion, the current partially fixed behavior is that the correct secondary store is selected when the flow opens.
6. The remaining problem is that after user creation, the selected user-store context resets back to Primary instead of staying on the previously selected secondary store.

### Reproduction Evidence
 I found two separate issues during reproduction:
the original issue appears to be only partially fixed, since the remaining problem is the reset back to Primary after user creation
there is also a different issue in the secondary database user-store flow where the create-user form rejects valid usernames even when the store’s saved regex values look correct.

---

## Solution Approach

### Analysis

The issue seems to be in the Console frontend, not the backend. From the current behavior, it looks like the selected secondary user store is picked up correctly when opening the add-user flow, but after the user is created, the page goes back to the users list in the default Primary store instead of staying in the previously selected store. So the problem is likely in the navigation or page state after user creation, where the selected user-store context is not being preserved.


### Proposed Solution

The way I'm planning on approaching this is to update the add-user flow so that when a user is created from a secondary user store context, that selected store is kept after the flow finishes. In other words, instead of returning to the users page with the default Primary store selected, it should return with the same user store that was selected before opening the modal.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** 
When an admin creates a user while working in a secondary user store context, the UI does not fully preserve that context. Even if the add-user flow opens with the correct secondary store selected, after the user is created the users page resets back to the Primary user store instead of staying on the previously selected secondary store.

**Match:** 
A similar pattern likely already exists elsewhere in the Console where filter state or selected tab state is preserved through navigation using URL parameters or route state. I will look for an existing feature in identity-apps that keeps current list context after a create/edit action and reuse that same approach instead of introducing a new state pattern.

**Plan:** [Step-by-step implementation plan]
1. Locate the users list component and identify where the selected user-store filter is stored.
2. Trace how the add-user flow is launched from that page.
3. Trace the success path after user creation and identify where navigation resets to the generic users page.
4.Update the flow so the selected store is preserved in route/query state.
5. Make the users page initialize from that preserved state before falling back to Primary.
6. Add or update tests to cover secondary-store context preservation.

**Implement:** 
Implementation will be done in the identity-apps frontend codebase on a dedicated branch. I will keep the fix scoped only to preserving user-store context after user creation and will not mix in the separate secondary-store validation issue.

**Review:** 
Before submitting, I will review the project’s contribution guidelines and ensure the change is scoped, documented clearly, and does not include unrelated fixes. I will also make sure the commit and PR description clearly state that this fixes the remaining user-store context reset after add-user completion.

**Evaluate:** 
The fix is successful if:

- the add-user flow opened from SEC returns the admin to the users list still scoped to SEC
- the same behavior works consistently for any selected secondary user store
- the primary-store flow still behaves correctly
- no unrelated user-management navigation behavior regresses

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
