# CarbonSheet JSON Schema & Content Specification

This document provides a comprehensive guide for AI coding agents and human authors on how to structure, validate, and maintain cheat sheet content files in **CarbonSheet**.

---

## 1. Overview ("Documentation as Code")

All cheat sheet data in CarbonSheet is managed as **structured JSON files** located in `src/content/sheets/` (e.g., `kotlin.json`, `typescript.json`). 

Each file represents a single reference sheet and is strictly validated at runtime and build time using **Zod** schemas defined in [`src/types/sheet.ts`](file:///Users/elnoxvie/Developments/github/stitch_devsheet_hub/src/types/sheet.ts).

---

## 2. Schema Hierarchy & Structure

```
SheetMetadata (Root Document)
├── aliases: string[] (Search synonyms e.g. ["hashmap", "dict"])
├── relatedSheets: string[] (Related sheet slugs e.g. ["typescript", "docker"])
├── prerequisites: { title, slug }[] (Prerequisite sheet links)
├── githubEditUrl: string (Edit on GitHub submission link)
└── sections: SheetTopic[] (Outer H2 Header Block with blue vertical bar)
    └── subtopics: SheetSubtopic[] (Inner Content Card)
        ├── table: { title, headers, rows } (Structured comparison table)
        ├── diagram: { type, content } (Architecture flowchart string)
        ├── tabs: CodeTab[] (Quick Reference / Detailed Example)
        │   ├── annotations: CodeAnnotation[] (Syntax token callouts)
        │   ├── evaluations: CodeEvaluation[] (Evaluated output "input » output")
        │   └── playground: { type, url, buttonLabel } (⚡ Run in Playground button)
        ├── callouts: CodeCallout[] (Titled Gotchas & Tips: 💡, ⚡, 📌, 🟢)
        └── tags: string[] (Card footer chips: ["math", "bigdecimal"])
```

---

## 3. Zod Definitions ([`src/types/sheet.ts`](file:///Users/elnoxvie/Developments/github/stitch_devsheet_hub/src/types/sheet.ts))

```typescript
import { z } from "zod";

// Code Token Annotations
export const CodeAnnotationSchema = z.object({
  id: z.string(),
  targetToken: z.string(),
  text: z.string(),
  position: z.enum(["top", "bottom", "right", "left"]).optional(),
});

// External Playground Integration
export const PlaygroundSchema = z.object({
  type: z.enum(["external_link", "kotlin", "typescript", "stackblitz", "codesandbox", "custom_embed"]),
  url: z.string().optional(),
  buttonLabel: z.string().optional(),
});

// Type Evaluation Markers (input » output)
export const CodeEvaluationSchema = z.object({
  input: z.string(),
  output: z.string(),
});

// Code Snippet Block
export const CodeBlockSchema = z.object({
  language: z.string(),
  code: z.string(),
  annotations: z.array(CodeAnnotationSchema).optional(),
  evaluations: z.array(CodeEvaluationSchema).optional(),
  playground: PlaygroundSchema.optional(),
});

// Code Tab (e.g., "Quick Reference", "Detailed Example")
export const CodeTabSchema = z.object({
  label: z.string(),
  language: z.string(),
  code: z.string(),
  annotations: z.array(CodeAnnotationSchema).optional(),
  evaluations: z.array(CodeEvaluationSchema).optional(),
  playground: PlaygroundSchema.optional(),
});

// Gotchas & Tip Callouts with optional title
export const CodeCalloutSchema = z.object({
  title: z.string().optional(),
  type: z.enum(["tip", "warning", "note", "danger", "syntax", "example"]).optional(),
  icon: z.string(),
  text: z.string(),
  color: z.enum(["green", "amber", "blue", "red", "purple", "default"]).optional(),
});

// Mermaid / Visual Diagram
export const DiagramSchema = z.object({
  type: z.enum(["mermaid", "svg"]),
  content: z.string(),
});

// Structured Comparison Table
export const TableSchema = z.object({
  title: z.string().optional(),
  headers: z.array(z.string()),
  rows: z.array(z.array(z.string())),
});

// Subtopic Card (Inner Content Container)
export const SheetSubtopicSchema = z.object({
  id: z.string(),
  title: z.string(),
  description: z.string().optional(),
  table: TableSchema.optional(),
  diagram: DiagramSchema.optional(),
  tabs: z.array(CodeTabSchema).optional(),
  codeBlocks: z.array(CodeBlockSchema).optional(),
  callouts: z.array(CodeCalloutSchema).optional(),
  tags: z.array(z.string()).optional(),
  isKnown: z.boolean().optional(),
});

// Outer Topic Section (Major H2 Header Block with vertical accent bar)
export const SheetTopicSchema = z.object({
  id: z.string(),
  title: z.string(),
  description: z.string().optional(),
  isKnown: z.boolean().optional(),
  codeBlocks: z.array(CodeBlockSchema).optional(),
  subtopics: z.array(SheetSubtopicSchema).optional(),
});

// Root Sheet Metadata Document
export const SheetMetadataSchema = z.object({
  id: z.string(),
  title: z.string(),
  slug: z.string(),
  category: z.string(),
  technology: z.string(),
  version: z.string(),
  difficulty: z.enum(["BEGINNER", "INTERMEDIATE", "ADVANCED"]),
  readTimeMinutes: z.number(),
  description: z.string(),
  tags: z.array(z.string()),
  updatedAt: z.string(),
  aliases: z.array(z.string()).optional(),
  relatedSheets: z.array(z.string()).optional(),
  prerequisites: z.array(
    z.object({
      title: z.string(),
      slug: z.string(),
    })
  ).optional(),
  githubEditUrl: z.string().optional(),
  sections: z.array(SheetTopicSchema),
});
```

---

## 4. Authoring Guidelines for Coding Agents

1. **Comparison Tables (`table`)**:
   - Add structured comparison tables to subtopics for quick side-by-side technical reference (e.g. `val vs var`, `const vs let`, HTTP codes, Modifier reference tables).
   ```json
   "table": {
     "title": "Variable Scope & Mutability Comparison",
     "headers": ["Keyword", "Scope", "Reassignable", "Hoisted"],
     "rows": [
       ["const", "Block Scope", "❌ No", "❌ No"],
       ["let", "Block Scope", "✅ Yes", "❌ No"],
       ["var", "Function Scope", "✅ Yes", "✅ Yes"]
     ]
   }
   ```

2. **Search Synonyms (`aliases`)**:
   - Provide alternative search terms in `aliases` (e.g. `["hashmap", "dict", "assoc array"]`) to make `⌘K` command-palette search instant and intuitive.

3. **External Playgrounds (`playground`)**:
   - Add `playground: { type: "kotlin", url: "https://play.kotlinlang.org/", buttonLabel: "Run on Kotlin Playground" }` to enable live code execution links.

4. **Callout Gotchas (`callouts`)**:
   - Add `title` to callouts for clear gotcha headers (e.g. `title: "Scale Check Gotcha"`, `color: "amber"`).

5. **Related Sheets (`relatedSheets`)**:
   - Add related sheet slugs in `relatedSheets` to render recommendations at the bottom of sheet detail pages.

6. **Validation Commands**:
   - Run `npx tsc --noEmit` to verify TypeScript type safety.
   - Run `npm run build` to verify Next.js static page generation.
