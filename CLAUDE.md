# CLAUDE.md - Outils Psychométriques Numériques

## Project Overview

This repository contains a collection of **French-language clinical psychometric assessment tools** designed for professional medical/psychiatric practice. All tools are **client-side only** web applications with no backend dependencies, ensuring data privacy and security.

### Key Characteristics
- **Language**: French (fr)
- **Target Users**: Mental health professionals (psychiatrists, psychologists)
- **Data Security**: All processing happens client-side; no data leaves the browser
- **Tech Stack**: Pure HTML, vanilla JavaScript, TailwindCSS (CDN), Lucide Icons
- **Output**: Print-friendly PDF generation via browser print dialog

---

## Repository Structure

```
/outils/
├── index.html              # Landing page with tool catalog
├── diva2.html             # DIVA 2.0 - ADHD structured interview
├── asrs.html              # ASRS v1.1 - Adult ADHD Self-Report Scale
├── raads.html             # RAADS-14 - Autism/Asperger screening
├── madrs.html             # MADRS - Depression rating scale
├── antidepresseurs.html   # Antidepressants reference guide
├── benzo.html             # Benzodiazepines pharmacology reference
└── CLAUDE.md              # This file
```

---

## Assessment Tools Catalog

### 1. **TDAH & Neurodevelopment** (ADHD & Neurodevelopmental)

#### DIVA 2.0 (`diva2.html`)
- **Full Name**: Diagnostic Interview for ADHD in Adults
- **Type**: Structured clinical interview
- **Color Theme**: Blue (`blue-500/600`)
- **Features**: Childhood vs. adulthood symptom distinction
- **Icon**: `brain-circuit`

#### ASRS v1.1 (`asrs.html`)
- **Full Name**: Adult ADHD Self-Report Scale
- **Type**: Self-report screening questionnaire
- **Color Theme**: Sky blue (`sky-500/600`)
- **Features**:
  - Part A (6 questions) - Screening threshold ≥4
  - Part B (12 questions) - Additional symptoms
  - Shaded cells indicate clinically significant responses
- **Icon**: `zap`
- **Scoring**: Grid-based with 5-point scale (Jamais → Très souvent)

#### RAADS-14 (`raads.html`)
- **Full Name**: Ritvo Autism Asperger Diagnostic Scale - 14 items
- **Type**: Autism spectrum screening
- **Color Theme**: Rose (`rose-500/600`)
- **Features**:
  - 14 questions across 3 domains
  - Mentalisation (7 items)
  - Social Anxiety (4 items)
  - Sensory Reactivity (3 items)
  - Threshold: Total > 14 suggests autism spectrum
- **Icon**: `puzzle`
- **Scoring**: 0-3 scale based on temporal occurrence

### 2. **Mood & Pharmacology** (Humeur & Pharmacologie)

#### MADRS (`madrs.html`)
- **Full Name**: Montgomery-Åsberg Depression Rating Scale
- **Type**: Clinician-rated depression severity scale
- **Color Theme**: Indigo (`indigo-500/600`)
- **Features**:
  - 10 items rated 0-6 (even scores anchored, odd scores interpolated)
  - Total score /60
  - Severity levels: Normal (0-6), Mild (7-19), Moderate (20-34), Severe (>34)
  - Item 10 (suicidal ideation) requires immediate clinical attention
- **Icon**: `bar-chart-2`
- **Scoring**: Fine-grained 0-6 slider per item with anchor descriptions

#### Antidépresseurs (`antidepresseurs.html`)
- **Type**: Reference tool (not an assessment)
- **Color Theme**: Teal (`teal-500/600`)
- **Content**: Visual memo of antidepressant profiles and side effects
- **Icon**: `pill`

#### Benzodiazépines (`benzo.html`)
- **Type**: Pharmacology reference table
- **Color Theme**: Violet (`violet-500/600`)
- **Features**:
  - Comprehensive BZD half-life database
  - Searchable table (molecule name, brand, notes)
  - Categories: Short (<10h), Intermediate (10-24h), Long (>24h)
  - Legal prescription limits (anxiolytics: 12 weeks, hypnotics: 4 weeks)
- **Icon**: `hourglass`
- **Special**: Includes active metabolite warnings and accumulation risks

---

## Technical Architecture

### Technology Stack
```javascript
// Core Dependencies (CDN)
- TailwindCSS v3.x (via cdn.tailwindcss.com)
- Lucide Icons (via unpkg.com/lucide@latest)
- Vanilla JavaScript (ES6+)
- No build process required
```

### Common Code Patterns

#### 1. **Page Structure Template**
Every assessment tool follows this structure:
```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>[Tool Name]</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @media print { /* Print styles */ }
    </style>
</head>
<body class="bg-slate-50 font-sans min-h-screen">
    <!-- No-print header (sticky toolbar) -->
    <!-- Main content (printable) -->
    <!-- JavaScript logic -->
    <script>
        lucide.createIcons(); // Initialize icons
    </script>
</body>
</html>
```

#### 2. **Print Optimization**
All tools include print media queries:
```css
@media print {
    .no-print { display: none !important; }
    body { background: white; -webkit-print-color-adjust: exact; }
    .shadow-xl { box-shadow: none !important; }
    input, textarea { border: none !important; }
}
```

#### 3. **State Management Pattern**
```javascript
// Example from ASRS
let answers = {}; // { questionId: { val: number, isSignificant: boolean } }

function select(qId, val, threshold, isSignificant) {
    answers[qId] = { val, isSignificant };
    updateUI();
    calculate();
}

function calculate() {
    // Compute scores based on answers object
    // Update DOM elements
}

function resetForm() {
    if(confirm('Effacer ?')) {
        answers = {};
        // Reset UI
    }
}
```

#### 4. **Scoring Display Pattern**
- **Live Score Display**: Floating score bubble (no-print) or inline score boxes
- **Color-Coded Severity**: Green/Blue/Orange/Red gradients
- **Threshold Indicators**: Visual cues for clinical significance

#### 5. **Navigation Pattern**
```html
<!-- Back button (always top-left) -->
<a href="index.html" class="p-2 hover:bg-slate-100 rounded-full">
    <i data-lucide="arrow-left"></i>
</a>
```

---

## Color Scheme & Branding

### Tool-Specific Colors
| Tool Category | Primary Color | Hex/Tailwind |
|--------------|---------------|--------------|
| DIVA 2.0 | Blue | `blue-600` |
| ASRS | Sky Blue | `sky-600` |
| RAADS-14 | Rose | `rose-600` |
| MADRS | Indigo | `indigo-600` |
| Antidépresseurs | Teal | `teal-600` |
| Benzodiazépines | Violet | `violet-600` |

### Severity Color Coding
- **Normal/Low**: Green (`green-500`)
- **Mild/Moderate**: Blue/Orange (`blue-500`, `orange-500`)
- **Severe/High**: Red (`red-600`)

---

## Development Guidelines

### Adding a New Assessment Tool

1. **Copy an existing template** that matches the assessment type:
   - For questionnaires → Use `asrs.html` or `raads.html`
   - For clinical scales → Use `madrs.html`
   - For reference tables → Use `benzo.html`

2. **Update metadata**:
   ```html
   <title>New Tool Name</title>
   <!-- Update header -->
   <h1>New Tool Name</h1>
   ```

3. **Choose a color theme**:
   - Pick an unused color from Tailwind palette
   - Apply consistently to: header bar, icon background, score displays, buttons

4. **Implement scoring logic**:
   ```javascript
   const questions = [ /* question objects */ ];
   let scores = {};

   function calculate() {
       // Your scoring algorithm
   }
   ```

5. **Add to index.html**:
   ```html
   <a href="newtool.html" class="group block bg-white rounded-xl...">
       <!-- Card content -->
   </a>
   ```

6. **Test print layout**:
   - Ensure `.no-print` elements are hidden
   - Verify page breaks work correctly
   - Check that colors print properly (webkit-print-color-adjust)

### Code Style Conventions

#### HTML
- Use semantic HTML5 elements
- Prefer Tailwind utility classes over custom CSS
- Keep inline styles minimal (only for print/special cases)
- Use data attributes for dynamic content: `id="score-${questionId}"`

#### JavaScript
- Use `const` for immutable data structures (question arrays)
- Use `let` for state management (scores object)
- Avoid jQuery or heavy frameworks
- Prefer arrow functions: `array.map(item => ...)`
- Always initialize Lucide: `lucide.createIcons()`

#### CSS/Tailwind
- Mobile-first responsive: `class="col-span-1 md:col-span-2"`
- Consistent spacing: `p-4`, `gap-4`, `space-y-4`
- Shadow hierarchy: `shadow-sm` (cards) → `shadow-xl` (containers)
- Border colors: `border-slate-200` (default), `border-slate-300` (emphasis)

#### Naming Conventions
- **Functions**: camelCase (`calculateTotal`, `renderQuestions`)
- **Variables**: camelCase (`totalScore`, `isSignificant`)
- **Constants**: camelCase for data (`questionsA`, `madrsData`)
- **IDs**: kebab-case (`score-mental`, `display-score-${id}`)
- **Classes**: Tailwind utilities only

### Accessibility Considerations
- Always include `lang="fr"` on `<html>`
- Use semantic form elements (`<input>`, `<textarea>`, `<button>`)
- Ensure keyboard navigation works (onclick handlers on focusable elements)
- Provide clear labels for inputs
- Use sufficient color contrast (tested with Tailwind defaults)

### Browser Compatibility
- Target: Modern evergreen browsers (Chrome, Firefox, Safari, Edge)
- No IE11 support required
- CDN dependencies may fail offline - document this limitation
- Print functionality relies on browser print dialog

---

## Clinical & Legal Notes

### Data Privacy
- **No server-side processing**: All computation happens in-browser
- **No data persistence**: Refreshing the page clears all data
- **No analytics/tracking**: No third-party scripts except CDNs
- **GDPR Compliant**: No personal data leaves the user's device

### Medical Disclaimers
- Tools are for **professional clinical use only**
- Scores are **indicative**, not diagnostic
- Always require clinical judgment and context
- Some tools (e.g., MADRS Item 10) flag immediate safety concerns

### Prescription Regulations (French Context)
- **Benzodiazepines**: Legal limits enforced (12 weeks anxiolytics, 4 weeks hypnotics)
- **Secure Prescriptions**: Some medications require "ordonnance sécurisée"
- **Specialist Initials**: Clonazepam (Rivotril) requires neuro/pediatrician initial prescription

---

## Workflow for AI Assistants

### When asked to modify an existing tool:

1. **Read the file first** using the Read tool
2. **Understand the scoring algorithm** before making changes
3. **Preserve the existing structure**:
   - Keep print styles intact
   - Maintain color scheme consistency
   - Don't change the data format (e.g., question objects)
4. **Test changes mentally**:
   - Will this break the calculate() function?
   - Does the new HTML render correctly?
   - Is the print layout still valid?

### When asked to create a new tool:

1. **Clarify requirements**:
   - What type of assessment? (questionnaire, scale, reference)
   - How many items/questions?
   - What's the scoring method?
   - What are the clinical cutoffs/interpretation rules?

2. **Choose the right template**:
   - Simple questionnaire → `asrs.html`
   - Multi-domain assessment → `raads.html`
   - Fine-grained scale → `madrs.html`
   - Data table → `benzo.html`

3. **Implement systematically**:
   - First: Data structure (questions array)
   - Second: Rendering logic (HTML generation)
   - Third: Interaction handlers (onclick, select)
   - Fourth: Scoring calculation
   - Fifth: Display/interpretation

4. **Add to navigation**:
   - Update `index.html` with new card
   - Use appropriate category (TDAH, Humeur, etc.)
   - Choose a unique icon and color

### When asked to debug:

1. **Check the JavaScript console** for errors
2. **Verify data flow**:
   - Are event handlers firing? (console.log in onclick)
   - Is state updating? (check scores object)
   - Is calculation correct? (console.log totals)
3. **Inspect the DOM**:
   - Are IDs unique?
   - Are classes applied correctly?
   - Is Lucide rendering icons?

### Common Pitfalls to Avoid

- **Don't add backend dependencies** - this is a client-only project
- **Don't break print layout** - always test @media print changes
- **Don't change question IDs** after deployment - breaks saved references
- **Don't add npm/build steps** - keep it simple (just HTML files)
- **Don't remove Lucide initialization** - `lucide.createIcons()` must run
- **Don't use English text** - all user-facing text must be French
- **Don't skip the confirm() dialog** on reset - prevents accidental data loss
- **Don't add external analytics** - violates privacy-first design

---

## Git Workflow

### Branching Strategy
- Main branch: `main` (production-ready code)
- Feature branches: `claude/[session-id]` (AI-assisted development)
- Always develop on designated feature branch

### Commit Messages
Use clear, descriptive French or English messages:
- ✅ "Create benzo.html"
- ✅ "Update index.html"
- ✅ "Fix MADRS scoring calculation"
- ❌ "WIP"
- ❌ "changes"

### Push Protocol
```bash
# Always use -u flag for feature branches
git push -u origin claude/[branch-name]

# Feature branch must start with 'claude/' and end with session ID
# Otherwise push will fail with 403
```

---

## Testing Checklist

When modifying or creating tools, verify:

### Functional Testing
- [ ] All questions/items render correctly
- [ ] Score calculation is mathematically correct
- [ ] Reset button clears all state
- [ ] Print button opens print dialog
- [ ] Back button returns to index.html
- [ ] Input fields accept text
- [ ] Date fields show date picker
- [ ] All interactive elements respond to clicks

### Visual Testing
- [ ] Color scheme is consistent with tool category
- [ ] Icons render (Lucide loaded correctly)
- [ ] Responsive layout works on mobile (320px+)
- [ ] Tablet layout works (768px+)
- [ ] Desktop layout works (1024px+)
- [ ] Print preview shows clean layout
- [ ] No console errors in DevTools

### Content Validation
- [ ] All text is in French
- [ ] Medical terminology is correct
- [ ] Scoring thresholds match published literature
- [ ] Citations/sources are accurate (if applicable)
- [ ] Patient information fields are present

---

## Future Enhancements (Ideas)

### Potential Features
- **Local Storage**: Save progress without server
- **PDF Export**: Programmatic PDF generation (jsPDF)
- **Multi-language**: Add English translations
- **Dark Mode**: System preference detection
- **Offline Mode**: Service Worker for PWA
- **Export to EHR**: JSON export for integration

### Architecture Improvements
- **Modular CSS**: Extract common print styles to shared file
- **Component Library**: Shared UI components (buttons, cards)
- **Build Process**: Optional Vite/Parcel for optimization
- **Type Safety**: Migrate to TypeScript
- **Testing**: Jest for scoring algorithm unit tests

**Note**: Any enhancements should maintain the core principle of **client-side only, zero backend dependencies**.

---

## Support & Resources

### Clinical Validation Sources
- **ASRS v1.1**: WHO Adult ADHD Self-Report Scale
- **RAADS-14**: Eriksson et al. (2013) validation study
- **MADRS**: Montgomery & Åsberg (1979) original publication
- **DIVA 2.0**: Sandra Kooij et al., Dutch ADHD guidelines

### Technical Documentation
- [TailwindCSS Docs](https://tailwindcss.com/docs)
- [Lucide Icons](https://lucide.dev)
- [MDN Print Styles](https://developer.mozilla.org/en-US/docs/Web/CSS/@media)

### French Medical Regulations
- [HAS - Haute Autorité de Santé](https://www.has-sante.fr)
- [ANSM - Agence Nationale de Sécurité du Médicament](https://ansm.sante.fr)

---

## Contact & Contribution

### For Medical Professionals
This tool is designed for professional clinical use. Questions about scoring interpretation or clinical application should be directed to the original instrument authors or relevant professional guidelines.

### For Developers
When contributing code:
1. Maintain the existing code style
2. Test thoroughly (functional + print layout)
3. Document any new patterns in this CLAUDE.md
4. Keep changes minimal and focused
5. Preserve client-side only architecture

---

## Version History

| Date | Version | Changes |
|------|---------|---------|
| 2024-11 | 1.0 | Initial repository with 7 tools |

---

**Last Updated**: 2025-11-27
**Maintained By**: AI-assisted development via Claude Code
**License**: Not specified (professional medical use)
