# Discovery Questions for Vasin

These questions refine production truth. They do not block unrelated engineering work.

## Highest priority

1. **Windows:** Which Windows versions are used on the real Vasin design workstation(s)?
2. **Photoshop:** What exact Photoshop version is installed?
3. **Illustrator:** What exact Illustrator version is installed?
4. **Workstations:** Is V1 intended for one computer or multiple computers?
5. **AI API key:** Will Vasin use its own OpenAI API key, or will we provide/manage the API account?
6. **Canva:** Must Canva be available in V1, or can it follow after the Adobe workflow is working?
7. **Languages:** Should the desktop UI and Astra support English only, or English + Dari/Farsi (or others)?
8. **Internet:** Must any design workflow function when internet is unavailable?

## Print-production questions

For each major product category Vasin actually prints, please provide the current real shop rules rather than estimates:

9. What printer/RIP is used?
10. What file format does the print operator prefer/require (AI, PDF/PDF-X, TIFF, JPEG, PSD/PSB, other)?
11. What resolution/effective-PPI rule is used for:
   - business cards/flyers/brochures;
   - indoor banners;
   - outdoor banners/billboards?
12. What scale convention is used for very large designs (1:1, 1:10, another ratio)?
13. What color mode/ICC profile is expected?
14. What bleed and safe-margin rules are used?
15. Are crop marks, registration marks, cut-contour/spot colors, or special named swatches required?
16. Are fonts kept editable, embedded in PDF, or outlined before final delivery?
17. Which output file is actually sent to the RIP/printer?

## Workflow questions

18. Does Vasin normally create designs mainly in Illustrator, Photoshop, or both?
19. Which 3–5 job types should Astra master first?
20. Should the operator approve Astra's plan before it changes Photoshop/Illustrator?
21. Should final export always require a human click/approval?
22. Do you want Astra to learn from Vasin's previous designs in V1, or after the first production workflow works?
23. Do existing designs/templates need to remain in a central searchable library?
24. Should Astra ever overwrite an existing source file, or always create a new version?

## Useful evidence to collect

- screenshots of Vasin's normal New Document / export settings;
- 2–3 successful historical jobs per priority product;
- an original editable source file plus the actual file sent to print;
- printer/RIP instructions from the production operator;
- a list of installed Adobe versions.

Answers should be copied into OPEN_ITEMS/DECISIONS and relevant PrinterProfiles so they become repository truth.
