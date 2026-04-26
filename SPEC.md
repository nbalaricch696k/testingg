# Snysters Extractor Pro - Specification

## 1. Project Overview
- **Project Name**: Snysters Extractor Pro
- **Type**: Single-page web application (Client-side only)
- **Core Functionality**: Extract valid email addresses from pasted text, filter unwanted domains, de-duplicate, and export results
- **Target Users**: Marketers, recruiters, data analysts who need to extract emails from text sources

## 2. UI/UX Specification

### Layout Structure
- **Header**: Logo/Title area with tagline
- **Main Content**: Two-column layout on desktop
  - Left: Input textarea (50% width)
  - Right: Output textarea (50% width)
- **Control Panel**: Between input/output with action buttons and filters
- **Footer**: Minimal with version info

### Responsive Breakpoints
- **Desktop**: > 1024px - Two-column side-by-side
- **Tablet**: 768px - 1024px - Stacked columns, reduced padding
- **Mobile**: < 768px - Single column, full-width elements

### Visual Design

#### Color Palette
- **Primary**: #1a1a2e (Deep navy background)
- **Secondary**: #16213e (Card/panel background)
- **Accent**: #0f3460 (Borders, subtle elements)
- **Highlight**: #e94560 (Primary action buttons, CTAs)
- **Success**: #00d9ff (Cyan accent for success states)
- **Text Primary**: #ffffff
- **Text Secondary**: #a0a0b0
- **Input Background**: #0d1b2a

#### Typography
- **Font Family**: 'Outfit' (Google Fonts) - Modern geometric sans-serif
- **Headings**: 700 weight
  - H1: 2.5rem (Project title)
  - H2: 1.5rem (Section titles)
  - H3: 1.1rem (Labels)
- **Body**: 400 weight, 1rem
- **Monospace**: 'JetBrains Mono' for email output

#### Spacing System
- **Base unit**: 8px
- **Container padding**: 32px (desktop), 16px (mobile)
- **Card padding**: 24px
- **Element gaps**: 16px
- **Button padding**: 12px 24px

#### Visual Effects
- **Card shadows**: 0 8px 32px rgba(0, 0, 0, 0.3)
- **Glow effects**: 0 0 20px rgba(233, 69, 96, 0.3) on primary buttons
- **Border radius**: 12px (cards), 8px (buttons/inputs)
- **Transitions**: all 0.3s ease

### Components

#### Header
- Project title "Snysters Extractor Pro" with gradient text effect
- Subtitle: "Smart Browser-Based Email Extractor"
- Animated icon (envelope/extract symbol)

#### Input Section
- Label: "Paste Your Text Here"
- Large textarea (min-height: 400px)
- Placeholder text with example
- Character count in corner

#### Output Section
- Label: "Extracted Emails"
- Read-only textarea
- Monospace font for clarity
- Placeholder when empty

#### Control Panel
- **Filter Toggles**:
  - [x] Remove .edu domains
  - [x] Remove .info domains
  - [x] Remove duplicates (checked by default)
  - [ ] Sort alphabetically
  - [ ] Group every 50 emails

- **Separator Options** (radio buttons):
  - Comma (,)
  - New Line (\n)
  - Pipe (|)

- **Action Buttons**:
  - Extract (Primary - highlighted)
  - Reset (Secondary)
  - Copy (Secondary)
  - Download ▼ (Dropdown: .txt, .csv)

#### Statistics Panel (Before & After)
- Three counter cards in a row:
  - Total Found: [number] (pill badge)
  - Filtered: [number] (with red accent)
  - Final Clean List: [number] (with cyan accent)

#### Button States
- **Default**: Solid background
- **Hover**: Brightness increase, slight scale (1.02)
- **Active**: Scale down (0.98)
- **Disabled**: 50% opacity, no pointer events

## 3. Functionality Specification

### Core Features

#### Email Extraction
- Use regex pattern: `/[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}/g`
- Match emails regardless of case
- Handle emails embedded in text

#### Filtering
- **Domain Filter**: Remove emails ending with:
  - `.edu` (any subdomain)
  - `.info` (any subdomain)
- Filter is automatic (always on) based on requirements

#### De-duplication
- Case-insensitive comparison
- Preserve first occurrence
- Update duplicate count

#### Output Formatting
- **Separators**:
  - Comma: `email1@example.com,email2@example.com`
  - New Line: each email on new line
  - Pipe: `email1@example.com|email2@example.com`
- **Sorting**: Optional alphabetical (A-Z)
- **Grouping**: Add `\n--- Break ---\n` every 50 emails when enabled

#### Statistics
- **Total Found**: Count of all regex matches
- **Filtered**: Count of .edu + .info domains removed
- **Final**: Unique valid emails after all processing

### User Interactions

#### Extract Button
1. Get input text
2. Run regex extraction
3. Apply filters (.edu, .info)
4. Remove duplicates
5. Apply formatting (separator, sort, group)
6. Display in output
7. Update counters

#### Reset Button
1. Clear input textarea
2. Clear output textarea
3. Reset all counters to 0
4. Uncheck optional toggles (keep required filters)

#### Copy Button
1. Get output text
2. Write to clipboard using navigator.clipboard
3. Show success toast notification

#### Download Button
1. Get output text
2. Create blob (txt or csv)
3. Trigger download with filename: `extracted_emails_YYYYMMDD_HHMMSS.ext`

### Edge Cases
- Empty input: Show message "Please paste text to extract emails"
- No emails found: Show message "No email addresses found in the input"
- All emails filtered: Show message "All emails were filtered out (.edu/.info domains)"
- Very large input: Process in chunks to prevent UI freeze

## 4. Acceptance Criteria

### Visual Checkpoints
- [ ] Dark theme with navy/cyan color scheme renders correctly
- [ ] Two-column layout on desktop, stacked on mobile
- [ ] All buttons have hover/active states
- [ ] Counters display with proper styling
- [ ] Responsive design works at all breakpoints

### Functional Checkpoints
- [ ] Regex extracts all valid email formats
- [ ] .edu and .info domains are automatically removed
- [ ] Duplicates are removed (case-insensitive)
- [ ] Separator options work correctly
- [ ] Sort toggle alphabetizes output
- [ ] Group toggle adds breaks every 50 emails
- [ ] Copy writes to clipboard
- [ ] Download creates proper file
- [ ] Counters show accurate Before & After stats

### Test Scenarios
1. Input: "test@test.com, test@test.com, admin@edu.com" → Output: "test@test.com" (1 final)
2. Input: "a@b.com\nc@d.com\ne@info.org" → Output: "a@b.com\nc@d.com" (2 final, 1 filtered)
3. Input: "First@Example.com, first@example.com" → Output: "First@Example.com" (1 final, deduped)