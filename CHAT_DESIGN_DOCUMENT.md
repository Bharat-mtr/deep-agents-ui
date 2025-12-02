# Chat Interface Design Document

## Overview

This document describes the design specifications for the Chromatic Agent chat interface. The chat interface is a conversational UI that enables users to interact with an AI agent, view tool calls, manage sub-agents, and handle human-in-the-loop (HITL) interruptions.

## Design System

### Color Palette

#### Light Theme
- **Primary**: `#FF7644` (Orange)
- **User Message Accent**: `#FF7644` (Orange)
- **Background**: `#F7F5F3` (Light Beige)
- **Surface**: `#FFFFFF` (White)
- **Surface Light**: `#FFF7F0` (Light Cream)
- **Surface Beige**: `#FAEED7` (Beige)
- **Border**: `rgba(0, 0, 0, 0.1)` (10% Black)
- **Border Light**: `rgba(0, 0, 0, 0.05)` (5% Black)
- **Text Primary**: `#000000` (Black)
- **Text Secondary**: `rgba(0, 0, 0, 0.6)` (60% Black)
- **Text Tertiary**: `rgba(0, 0, 0, 0.7)` (70% Black)
- **Success**: `#10b981` (Green)
- **Warning**: `#FFC16F` (Yellow)
- **Error**: `#FF564E` (Red)
- **Interactive Hover**: Light background on hover states

#### Dark Theme (Available)
- **Primary**: `#2dd4bf` (Cyan)
- **Background**: `#0f0f0f` (Near Black)
- **Surface**: `#1a1a1a` (Dark Gray)
- **Border**: `#2d2d2d` (Medium Gray)
- **Text Primary**: `#f3f4f6` (Light Gray)
- **Text Secondary**: `#9ca3af` (Medium Gray)

### Typography

- **Font Family**: `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif`
- **Monospace Font**: `"SF Mono", Monaco, "Cascadia Code", "Roboto Mono", Consolas, "Courier New", monospace`
- **Font Sizes**:
  - XS: `0.75rem` (12px)
  - SM: `0.875rem` (14px)
  - Base: `1rem` (16px)
  - LG: `1.125rem` (18px)
  - XL: `1.25rem` (20px)
  - 2XL: `1.5rem` (24px)
  - 3XL: `1.875rem` (30px)
- **Font Weights**: Medium (500), Semibold (600)
- **Line Heights**: Tight (1.25), Normal (1.5), Relaxed (1.75)

### Spacing Scale

- **XS**: `0.25rem` (4px)
- **SM**: `0.5rem` (8px)
- **MD**: `1rem` (16px)
- **LG**: `1.5rem` (24px)
- **XL**: `2rem` (32px)
- **2XL**: `3rem` (48px)

### Border Radius

- **SM**: `0.25rem` (4px)
- **MD**: `0.375rem` (6px)
- **LG**: `0.5rem` (8px)
- **Full**: `9999px` (Circular)

### Transitions

- **Base**: `200ms ease`

## Layout Structure

### Container Hierarchy

```
ChatInterface (Full Height Container)
├── Header (Fixed Height: 48px)
│   ├── Header Left
│   │   ├── Logo (20px × auto)
│   │   └── Title ("chromatic agent")
│   └── Header Right
│       ├── New Thread Button (32px × 32px)
│       └── History Button (32px × 32px)
├── Content (Flex: 1, Scrollable)
│   ├── ThreadHistorySidebar (Optional, 320px width, Slide-in)
│   └── MessagesContainer (Flex: 1)
│       ├── Empty State (Centered)
│       ├── Thread Loading State (Centered)
│       └── MessagesList (Max-width: 900px, Centered)
│           └── ChatMessage[] (Margin-bottom: 24px)
└── InputForm (Fixed Bottom, Max-width: 900px, Centered)
    ├── Image Preview Container (Optional)
    └── Input Row
        ├── Attach Button (32px × 32px)
        ├── Textarea (Flex: 1, Auto-height)
        └── Send/Stop Button (32px × 32px)
```

### Key Layout Constraints

- **Chat Max Width**: `900px` (centered)
- **Header Height**: `48px`
- **Sidebar Width**: `320px` (when open)
- **Message Spacing**: `24px` between messages
- **Input Padding**: `24px` vertical, `32px` horizontal

## Component Specifications

### 1. ChatInterface (Main Container)

**Purpose**: Main wrapper component that orchestrates the entire chat experience.

**Layout**:
- Full height flex container
- Background: `var(--color-background)`
- Flex direction: Column

**Key Features**:
- Header with logo and action buttons
- Scrollable message area
- Fixed input form at bottom
- Optional thread history sidebar

**States**:
- Empty state (no messages)
- Loading thread state
- Active conversation state
- Loading message state

---

### 2. Header

**Dimensions**: 
- Height: `48px`
- Padding: `16px 24px` (vertical, horizontal)

**Left Section**:
- Logo: `20px` width, auto height
- Title: Base font size, medium weight
- Gap between logo and title: `8px`

**Right Section**:
- Two icon buttons: `32px × 32px`
- Gap between buttons: `4px`
- Ghost variant buttons
- Hover: Light background color
- Disabled: 40% opacity

**Buttons**:
- **New Thread** (SquarePen icon): Disabled when no messages
- **History** (History icon): Toggles thread history sidebar

---

### 3. MessagesContainer

**Layout**:
- Flex: 1 (takes remaining space)
- Overflow: Hidden
- Position: Relative

**MessagesList**:
- Flex: 1
- Overflow-y: Auto
- Padding: `24px 32px`
- Max-width: `900px`
- Margin: Auto (centered)
- Custom scrollbar styling

**Scrollbar**:
- Width: `10px`
- Track: Transparent
- Thumb: Border color, rounded
- Thumb hover: Tertiary text color

---

### 4. ChatMessage

**Layout**:
- Flex container, row direction
- Gap: `16px`
- Margin-bottom: `24px`
- Width: 100%
- Max-width: 100%

#### User Messages

**Visual Style**:
- No avatar shown
- Content: Left-aligned
- Border-left: `3px solid` primary color
- Padding-left: `16px`
- Text: Plain text, pre-wrap
- Background: Transparent

**Content Structure**:
- Text content (if present)
- Images (if present) - displayed below text

#### Assistant Messages

**Visual Style**:
- Avatar: `28px × 28px` circle (shown/hidden based on previous message)
- Avatar background: Surface light color
- Avatar icon: `14px × 14px` logo
- Content: Markdown rendered
- Background: Transparent
- No border

**Content Structure** (in order):
1. Text content (Markdown rendered)
2. Images (if present)
3. Tool calls (if present)
4. Sub-agent indicators (if present)

**Avatar Visibility**:
- Show avatar if message type differs from previous message
- Hide avatar if same type as previous (maintains spacing)

---

### 5. Message Content Components

#### MarkdownContent

**Purpose**: Renders assistant message content with markdown support.

**Features**:
- Full markdown support (GFM)
- Syntax highlighting for code blocks (Prism, oneDark theme)
- Inline code styling
- Link styling (external, opens in new tab)
- Blockquote styling
- List styling (ordered and unordered)
- Table wrapper with horizontal scroll

**Code Blocks**:
- Dark theme (oneDark)
- Language detection
- Pre-wrapped for proper formatting

#### Image Display

**User Images**:
- Max-width: `80px`
- Max-height: `80px`
- Border-radius: `4px`
- Object-fit: Cover
- Border: 1px solid border color
- Box-shadow: Subtle shadow
- Display: Flex row, wrap, gap: `4px`

**Assistant Images**:
- Max-width: 100%
- Max-height: `400px`
- Border-radius: `4px`
- Object-fit: Contain

---

### 6. ToolCallBox

**Purpose**: Displays tool call information with expand/collapse functionality.

**Layout**:
- Container with header button
- Expandable content section

**Header**:
- Clickable button (ghost variant)
- Left-aligned content:
  - Chevron icon (right when collapsed, down when expanded)
  - Status icon (pending/running/completed/error)
  - Tool name
- Disabled if no content to show

**Status Icons**:
- **Pending**: Clock icon
- **Running**: Loader icon (animated)
- **Completed**: CheckCircle icon (green)
- **Error**: AlertCircle icon (red)

**Content** (when expanded):
- Arguments section (if present)
  - Section title: "Arguments"
  - JSON formatted code block
- Result section (if present)
  - Section title: "Result"
  - Formatted code block (string or JSON)

**Styling**:
- Code blocks: Monospace font, formatted JSON
- Section titles: Small font, medium weight
- Sections: Margin-top spacing

---

### 7. SubAgentIndicator

**Purpose**: Clickable indicator for sub-agent tasks.

**Layout**:
- Button element (full width)
- Hover: Background color change

**Content**:
- Header row:
  - Status icon (left)
  - Sub-agent name (right)
- Description: Sub-agent input/description text

**Status Icons**:
- **Pending**: Clock icon
- **Active**: Loader icon (animated)
- **Completed**: CheckCircle icon (green)
- **Error**: AlertCircle icon (red)

**Styling**:
- Padding: Medium spacing
- Border-radius: Small
- Hover: Background color change
- Cursor: Pointer

---

### 8. HITLPanel (Human-in-the-Loop)

**Purpose**: Displays interruption state and allows approve/reject actions.

**Layout**:
- Container with content and actions
- Positioned below last AI message
- Max-width: `900px`, centered

**Content Section**:
- Tool info:
  - Tool name: "Calling **[tool name]**"
  - Arguments: Formatted JSON (if present)
  - No parameters message (if no args)

**Actions Section**:
- Reject button (outline variant, red)
  - X icon
  - "Reject" text
- Approve button (primary variant, green)
  - Check icon (or loading spinner)
  - "Approve" text (or "Processing...")

**States**:
- Loading: Spinner in approve button, both buttons disabled
- Ready: Both buttons enabled

**Styling**:
- Arguments: Monospace, formatted
- Buttons: Small size, icon + text
- Gap between buttons: Small spacing

---

### 9. InputForm

**Layout**:
- Fixed at bottom
- Padding: `24px 32px`
- Max-width: `900px`, centered
- Background: Background color
- Flex-shrink: 0

#### Image Preview Container

**Layout** (when images present):
- Flex row, wrap
- Gap: `8px`
- Padding: `8px`
- Background: Surface light color
- Border-radius: `4px`

**Image Preview**:
- Size: `80px × 80px`
- Border-radius: `4px`
- Overflow: Hidden
- Position: Relative

**Remove Button**:
- Position: Absolute, top-right (`4px` from edges)
- Size: `20px × 20px`
- Background: `rgba(0, 0, 0, 0.7)`
- Border-radius: 50%
- Opacity: 0 (visible on hover)
- Hover: Red background, opacity 1
- Icon: X, `14px`

#### Input Row

**Layout**:
- Flex row
- Gap: `4px`
- Align-items: Center
- Background: Surface color
- Border-radius: `4px`
- Padding: `4px`
- Transition: Background color on focus

**Focus State**:
- Background: Surface light color

**Components**:
1. **Attach Button** (32px × 32px)
   - Ghost variant
   - Paperclip icon, `20px`
   - Color: Secondary text
   - Hover: Interactive hover background, primary text

2. **Textarea** (Flex: 1)
   - Border: None
   - Background: Transparent
   - Padding: `8px 16px`
   - Font-size: Base
   - Color: Primary text
   - Resize: None
   - Min-height: `20px`
   - Max-height: `200px`
   - Overflow-y: Auto
   - Line-height: Relaxed
   - Placeholder: Tertiary text color
   - Auto-height: Adjusts to content
   - Custom scrollbar (6px width)

3. **Send/Stop Button** (32px × 32px)
   - **Send Button**:
     - Primary gradient background
     - White text
     - Send icon, `16px`
     - Disabled: 50% opacity (when no input and no images)
   - **Stop Button**:
     - Error color background
     - White text
     - Square icon, `16px`
     - Hover: 90% opacity

---

### 10. Empty State

**Layout**:
- Centered vertically and horizontally
- Max-width: `900px`
- Padding: `48px`
- Text-align: Center

**Content**:
- Logo icon: `48px` width, 50% opacity
- Heading: Large font size, medium weight, secondary text color
- Margin-bottom: `8px` between icon and heading

---

### 11. Loading States

#### Thread Loading State

**Layout**:
- Position: Absolute, centered
- Margin-top: `100px`
- Full width and height
- Background: Background color
- Z-index: 10

**Spinner**:
- Size: `50px × 50px`
- Color: Primary color
- Animation: Spin (1s linear infinite)

#### Message Loading State

**Layout**:
- Flex row
- Align-items: Center
- Gap: `8px`
- Padding: `16px 0`
- Color: Secondary text
- Font-size: Small

**Content**:
- Spinner: `16px × 16px`, animated
- Text: "Working..."

#### Image Upload Loading

**Layout**:
- In send button
- Spinner: `16px × 16px`
- Replaces send icon

---

### 12. ThreadHistorySidebar

**Purpose**: Slide-in sidebar for thread history (detailed specs in separate component doc).

**Behavior**:
- Slides in from left when open
- Overlays or pushes content (implementation dependent)
- Width: `320px`
- Toggle via history button in header

---

## Interactions & Behaviors

### Message Sending

1. **Input Validation**:
   - Submit disabled if: No text AND no images
   - Submit enabled if: Text OR images present

2. **Image Upload Flow**:
   - User clicks attach button
   - File picker opens (images only, multiple)
   - Images added to preview container
   - On submit: Images uploaded to Supabase
   - Upload state: Loading spinner in send button
   - After upload: URLs appended to message

3. **Message Submission**:
   - Enter key: Submit (Shift+Enter: New line)
   - Submit button: Submit
   - Textarea cleared after submit
   - Images cleared after submit
   - Textarea height resets

4. **Auto-scroll**:
   - Messages container scrolls to bottom on new message
   - Smooth scroll behavior

### Message Display

1. **Message Processing**:
   - Messages filtered by supported types (human, ai, system, developer, tool)
   - Tool calls extracted from AI messages
   - Tool results matched to tool calls
   - Avatar visibility determined by message type sequence

2. **Tool Call States**:
   - Pending: Tool call created, waiting for result
   - Running: Tool execution in progress
   - Completed: Tool executed successfully
   - Error: Tool execution failed

3. **Sub-agent Detection**:
   - Tool calls with name "task" and subagent_type are treated as sub-agents
   - Displayed as clickable indicators below message
   - Clicking selects sub-agent (triggers parent callback)

### HITL (Human-in-the-Loop)

1. **Interrupt Detection**:
   - Polls stream for interrupt state every 2 seconds
   - Checks stream.interrupt property (primary method)
   - Fallback: Polls thread state API

2. **Interrupt Display**:
   - Shown below last AI message
   - Displays tool name and arguments
   - Shows approve/reject buttons

3. **Interrupt Resolution**:
   - Approve: Resumes with approve decision
   - Reject: Resumes with reject decision
   - Loading state during resolution
   - Interrupt cleared after resolution

### Thread Management

1. **New Thread**:
   - Button disabled when no messages
   - Clicking creates new thread
   - Cancels any ongoing stream
   - Clears current messages

2. **Thread Selection**:
   - Opens thread history sidebar
   - Selecting thread loads its messages
   - Sidebar closes after selection

---

## Responsive Considerations

### Desktop (Default)
- Max-width: `900px` for chat content
- Centered layout
- Full sidebar width when open

### Mobile (Future)
- Consider full-width layout
- Sidebar as overlay/modal
- Adjust input form padding
- Smaller avatar sizes
- Touch-friendly button sizes

---

## Accessibility

### Keyboard Navigation
- Tab through interactive elements
- Enter to submit message
- Shift+Enter for new line
- Escape to close sidebar (if implemented)

### ARIA Labels
- Buttons have descriptive labels
- Sub-agent indicators have aria-labels
- Loading states announced

### Focus Management
- Focus moves to input after message send
- Focus management for sidebar open/close

### Screen Reader Support
- Status icons have appropriate labels
- Tool call states announced
- Loading states announced

---

## Animation & Transitions

### Spinner Animation
```css
@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
```

### Transitions
- Background color changes: `200ms ease`
- Opacity changes: `200ms ease`
- Scroll behavior: Smooth

---

## Component Dependencies

### External Libraries
- `react-markdown`: Markdown rendering
- `remark-gfm`: GitHub Flavored Markdown
- `react-syntax-highlighter`: Code syntax highlighting
- `lucide-react`: Icons
- `@langchain/langgraph-sdk`: Message streaming

### Internal Components
- `Button` (from `@/components/ui/button`)
- `ThreadHistorySidebar`
- `HITLPanel`
- `MarkdownContent`
- `ToolCallBox`
- `SubAgentIndicator`

---

## State Management

### Local State
- Input text
- Uploaded images
- Thread history sidebar open/closed
- Tool call expanded/collapsed states

### Props/Context
- Thread ID
- Messages (from `useChat` hook)
- Loading states
- Selected sub-agent
- Interrupt state

### Hooks
- `useChat`: Manages message streaming, sending, HITL
- Custom hooks for file watching, etc.

---

## Error Handling

### Message Coercion Errors
- Filtered out gracefully
- Logged for debugging
- Don't crash the UI

### Image Upload Errors
- Logged to console
- TODO: User-facing error message

### Stream Errors
- Handled via onError callback
- Non-critical errors logged
- UI remains functional

---

## Performance Considerations

### Optimization
- React.memo for message components
- useMemo for expensive computations
- useCallback for event handlers
- Optimistic updates for messages

### Memory Management
- Object URLs revoked when images removed
- Messages filtered to prevent memory leaks

---

## Future Enhancements

### Potential Additions
- Message editing
- Message deletion
- Copy message content
- Export conversation
- Search within conversation
- Message reactions
- Typing indicators
- Read receipts
- Dark mode toggle (colors defined, not implemented)

---

## Design Tokens Summary

### Colors
- Primary: `#FF7644`
- Background: `#F7F5F3`
- Surface: `#FFFFFF`
- Text Primary: `#000000`
- Text Secondary: `rgba(0, 0, 0, 0.6)`

### Spacing
- Base unit: `4px` (0.25rem)
- Scale: 4px, 8px, 16px, 24px, 32px, 48px

### Typography
- Base font: System font stack
- Base size: `16px` (1rem)
- Line height: `1.75` (relaxed)

### Components
- Button size: `32px × 32px` (icon buttons)
- Avatar size: `28px × 28px`
- Border radius: `4px` (standard), `9999px` (circular)

---

## Implementation Notes

### File Structure
```
ChatInterface/
├── ChatInterface.tsx
└── ChatInterface.module.scss

ChatMessage/
├── ChatMessage.tsx
└── ChatMessage.module.scss

ToolCallBox/
├── ToolCallBox.tsx
└── ToolCallBox.module.scss

MarkdownContent/
├── MarkdownContent.tsx
└── MarkdownContent.module.scss

SubAgentIndicator/
├── SubAgentIndicator.tsx
└── SubAgentIndicator.module.scss

HITLPanel/
├── HITLPanel.tsx
└── HITLPanel.module.scss
```

### Styling Approach
- SCSS Modules for component styles
- CSS Variables for theming
- Shared variables file: `_variables.scss`
- BEM-like naming convention

### TypeScript Types
- Defined in `types/types.ts`
- Message types from LangChain SDK
- Custom types for ToolCall, SubAgent, etc.

---

## Visual Reference

### Color Usage
- **Primary Orange** (`#FF7644`): User message border, primary buttons, logo accent
- **Light Beige** (`#F7F5F3`): Main background
- **White** (`#FFFFFF`): Message surfaces, input background
- **Light Cream** (`#FFF7F0`): Hover states, input focus
- **Black** (`#000000`): Primary text
- **60% Black**: Secondary text, icons
- **10% Black**: Borders, dividers

### Typography Hierarchy
- **Title**: Base size, medium weight (header)
- **Body**: Base size, normal weight (messages)
- **Small**: Small size (metadata, tool names)
- **Code**: Monospace (tool arguments, results)

### Spacing Rhythm
- Consistent `8px` base unit
- Messages: `24px` bottom margin
- Sections: `16px` gaps
- Input: `24px` vertical padding

---

## Conclusion

This design document provides comprehensive specifications for implementing the Chromatic Agent chat interface. The design emphasizes clarity, usability, and a clean aesthetic with the signature orange accent color. All components are designed to work together cohesively while maintaining flexibility for future enhancements.

For implementation, refer to the component files and styling modules in the codebase. The design system tokens (colors, spacing, typography) are defined in `_variables.scss` and should be used consistently across all components.

