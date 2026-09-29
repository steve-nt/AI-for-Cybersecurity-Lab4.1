# Review lab implementation

> Review lab implementation (ipynb file). check according to lab report in reports folder and lab assignment in pdf in the root folder

<details><summary>8 previous messages</summary>

> I’ll compare the notebook against both the assignment requirements and the submitted report, then report concrete gaps with file/cell references and severity. I’m using the PDF skill for reliable assignment extraction; if the report is a Word document, I’ll use the document-review workflow too.
>
> <details><summary>Ran 2 commands</summary>
>
> <details><summary>Explored a file</summary>
>
> - Read `C:/Users/Owner/.codex/plugins/cache/openai-primary-runtime/pdf/26.909.11814/skills/pdf/SKILL.md`
>
> </details>
>
> - Ran `rg --files -g '*.ipynb' -g '*.pdf' -g '*.docx' -g '*.md' -g '*.txt' -g '*.html' -g '*.tex' -g 'AGENTS.md'`
>
> - Ran `$env:PATH='C:\Users\Owner\.cache\codex-runtimes\codex-primary-runtime\dependencies\bin\override;C:\Users\Owner\.cache\codex-runtimes\codex-primary-runtime\dependencies\bin\fallback;'+$env:PATH
> pdfinfo '4.1 Lab_ Explaining_Phishing_Detectors.pdf'
> pdfinfo 'report\Lab4_1_Report.pdf'
> Get-Item 'lab4_1_explaining_phishing_detectors.ipynb','report\Lab4_1_Report.md','4.1 Lab_ Explaining_Phishing_Detectors.txt' | Select-Object FullName,Length,LastWriteTime`
>
> </details>
>
> I found one main notebook, the 13-page assignment PDF, and a five-page final report (plus matching Markdown/Word sources). I’m now mapping each graded requirement to the notebook evidence and checking whether the report’s claims are reproducible from the executed outputs.
>
> <details><summary>Ran 6 commands</summary>
>
> - Ran `$i=0; Get-Content -LiteralPath '4.1 Lab_ Explaining_Phishing_Detectors.txt' | ForEach-Object { $i++; '{0,4}: {1}' -f $i, $_ }
> $i=0; Get-Content -LiteralPath 'report\Lab4_1_Report.md' | ForEach-Object { $i++; '{0,4}: {1}' -f $i, $_ }`
> - Ran `& 'C:\Users\Owner\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' -c "import json, pathlib; p=pathlib.Path('lab4_1_explaining_phishing_detectors.ipynb'); n=json.loads(p.read_text(encoding='utf-8')); print('nbformat',n['nbformat'],n['nbformat_minor'],'cells',len(n['cells'])); print('kernel',n.get('metadata',{}).get('kernelspec')); print('language',n.get('metadata',{}).get('language_info',{}).get('version'));\nfor i,c in enumerate(n['cells']):\n s=''.join(c.get('source',[])); first=next((x.strip() for x in s.splitlines() if x.strip()),''); outs=c.get('outputs',[]); errs=[o.get('ename','')+': '+o.get('evalue','') for o in outs if o.get('output_type')=='error']; print(f'{i:02d} {c[\"cell_type\"]:8} exec={str(c.get(\"execution_count\")):>4} outputs={len(outs):2} lines={len(s.splitlines()):3} | {first[:120]}'); [print('   ERROR',e) for e in errs]"`
> - Ran ``$nb = Get-Content -LiteralPath 'lab4_1_explaining_phishing_detectors.ipynb' -Raw | ConvertFrom-Json
> "nbformat $($nb.nbformat).$($nb.nbformat_minor) cells $($nb.cells.Count)"
> "kernel $($nb.metadata.kernelspec.name) / $($nb.metadata.kernelspec.display_name)"
> for ($i=0; $i -lt $nb.cells.Count; $i++) {
>   $c=$nb.cells[$i]; $src=($c.source -join ''); $first=(($src -split "`n") | Where-Object { $_.Trim() } | Select-Object -First 1).Trim(); $outs=@($c.outputs)
>   '{0:D2} {1,-8} exec={2,4} outputs={3,2} lines={4,3} | {5}' -f $i,$c.cell_type,$c.execution_count,$outs.Count,($src -split "`n").Count,$first.Substring(0,[Math]::Min(120,$first.Length))
>   foreach($o in $outs){ if($o.output_type -eq 'error'){ "   ERROR $($o.ename): $($o.evalue)" } }
> }``
> - Ran `$lines=Get-Content -LiteralPath '4.1 Lab_ Explaining_Phishing_Detectors.txt'
> foreach($range in @(@(349,520),@(521,700),@(701,869))){
>   for($n=$range[0];$n -le [Math]::Min($range[1],$lines.Count);$n++){ '{0,4}: {1}' -f $n,$lines[$n-1] }
> }`
> - Ran `New-Item -ItemType Directory -Force -Path 'tmp\pdfs\assignment','tmp\pdfs\report' | Out-Null
> $env:PATH='C:\Users\Owner\.cache\codex-runtimes\codex-primary-runtime\dependencies\bin\override;C:\Users\Owner\.cache\codex-runtimes\codex-primary-runtime\dependencies\bin\fallback;'+$env:PATH
> pdftoppm -png -r 110 '4.1 Lab_ Explaining_Phishing_Detectors.pdf' 'tmp\pdfs\assignment\page'
> pdftoppm -png -r 140 'report\Lab4_1_Report.pdf' 'tmp\pdfs\report\page'
> Get-ChildItem 'tmp\pdfs\assignment','tmp\pdfs\report' -Filter '*.png' | Select-Object FullName,Length`
> - Ran `& 'C:\Users\Owner\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' -c 'from PIL import Image,ImageOps,ImageDraw; from pathlib import Path; files=sorted(Path("tmp/pdfs/assignment").glob("*.png")); imgs=[Image.open(p).convert("RGB") for p in files]; thumbs=[]; [(im.thumbnail((360,510)),thumbs.append(ImageOps.expand(im,border=2,fill="black"))) for im in imgs]; sheet=Image.new("RGB",(4*364,4*514),"white"); [(sheet.paste(im,((i%4)*364,(i//4)*514)),ImageDraw.Draw(sheet).text(((i%4)*364+8,(i//4)*514+8),str(i+1),fill="red",stroke_width=2,stroke_fill="white")) for i,im in enumerate(thumbs)]; sheet.save("tmp/pdfs/assignment-contact.png")'
> & 'C:\Users\Owner\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' -c 'from PIL import Image,ImageOps,ImageDraw; from pathlib import Path; files=sorted(Path("tmp/pdfs/report").glob("*.png")); imgs=[Image.open(p).convert("RGB") for p in files]; thumbs=[]; [(im.thumbnail((500,707)),thumbs.append(ImageOps.expand(im,border=2,fill="black"))) for im in imgs]; sheet=Image.new("RGB",(3*504,2*711),"white"); [(sheet.paste(im,((i%3)*504,(i//3)*711)),ImageDraw.Draw(sheet).text(((i%3)*504+8,(i//3)*711+8),str(i+1),fill="red",stroke_width=2,stroke_fill="white")) for i,im in enumerate(thumbs)]; sheet.save("tmp/pdfs/report-contact.png")'
> Get-Item 'tmp\pdfs\assignment-contact.png','tmp\pdfs\report-contact.png' | Select-Object FullName,Length`
>
> </details>
>
> Viewed 2 images
>
> <details><summary>Ran 7 commands</summary>
>
> - Ran `$i=0; Get-Content -LiteralPath 'README.md' | ForEach-Object { $i++; '{0,4}: {1}' -f $i, $_ }
> $i=0; Get-Content -LiteralPath 'requirements.txt' | ForEach-Object { $i++; '{0,4}: {1}' -f $i, $_ }
> git status --short`
> - Ran `$nb = Get-Content -LiteralPath 'lab4_1_explaining_phishing_detectors.ipynb' -Raw | ConvertFrom-Json
> foreach($i in 2,4,6,8,10,12,14,16,18,20,22){
>   $c=$nb.cells[$i]; "===== CELL $i exec=$($c.execution_count) ====="; ($c.source -join ''); '----- outputs -----'
>   foreach($o in @($c.outputs)){
>     if($o.output_type -eq 'stream'){ $o.text -join '' }
>     elseif($o.output_type -eq 'execute_result' -or $o.output_type -eq 'display_data'){
>       if($o.data.'text/plain'){ $o.data.'text/plain' -join '' }
>       if($o.data.'text/html'){ '[HTML output]' }
>       if($o.data.'image/png'){ '[PNG output]' }
>     }
>     elseif($o.output_type -eq 'error'){ "$($o.ename): $($o.evalue)" }
>   }
> }`
> - Ran `$nb = Get-Content -LiteralPath 'lab4_1_explaining_phishing_detectors.ipynb' -Raw | ConvertFrom-Json
> foreach($i in 25,28,31,33,36,39,40,41,44,47){
>   $c=$nb.cells[$i]; "===== CELL $i exec=$($c.execution_count) ====="; ($c.source -join ''); '----- outputs -----'
>   foreach($o in @($c.outputs)){
>     if($o.output_type -eq 'stream'){ $o.text -join '' }
>     elseif($o.output_type -eq 'execute_result' -or $o.output_type -eq 'display_data'){
>       if($o.data.'text/plain'){ $o.data.'text/plain' -join '' }
>       if($o.data.'text/html'){ '[HTML output]' }
>       if($o.data.'image/png'){ '[PNG output]' }
>     }
>     elseif($o.output_type -eq 'error'){ "$($o.ename): $($o.evalue)" }
>   }
> }`
> - Ran `$nb = Get-Content -LiteralPath 'lab4_1_explaining_phishing_detectors.ipynb' -Raw | ConvertFrom-Json
> foreach($i in 50,53,55,56,59,62,63,65,68,71,74,77,80){
>   $c=$nb.cells[$i]; "===== CELL $i exec=$($c.execution_count) ====="; ($c.source -join ''); '----- outputs -----'
>   foreach($o in @($c.outputs)){
>     if($o.output_type -eq 'stream'){ $o.text -join '' }
>     elseif($o.output_type -eq 'execute_result' -or $o.output_type -eq 'display_data'){
>       if($o.data.'text/plain'){ $o.data.'text/plain' -join '' }
>       if($o.data.'text/html'){ '[HTML output]' }
>       if($o.data.'image/png'){ '[PNG output]' }
>     }
>     elseif($o.output_type -eq 'error'){ "$($o.ename): $($o.evalue)" }
>   }
> }`
> - Ran `$env:PATH='C:\Users\Owner\.cache\codex-runtimes\codex-primary-runtime\dependencies\bin\override;C:\Users\Owner\.cache\codex-runtimes\codex-primary-runtime\dependencies\bin\fallback;'+$env:PATH
> pdftotext -layout 'report\Lab4_1_Report.pdf' - | Select-String -Pattern '\{\{|\[\[|nan|inf|Traceback|Error' -CaseSensitive
> Get-ChildItem -LiteralPath 'results\tables' -File | Select-Object Name,Length | Sort-Object Name
> Get-ChildItem -LiteralPath 'results\figures' -File | Select-Object Name,Length | Sort-Object Name`
> - Ran `& 'C:\Users\Owner\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' -c 'from pypdf import PdfReader; import re; p=PdfReader("report/Lab4_1_Report.pdf"); ts=[x.extract_text() or "" for x in p.pages]; print("pages",len(ts)); print("words/page",[len(re.findall(r"\b\w+\b",t)) for t in ts]); print("unresolved",[x for x in ("{{","}}","[[","]]","Traceback") if x in "\n".join(ts)])'
> & 'C:\Users\Owner\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' -c 'from pypdf import PdfReader; import re; p=PdfReader("4.1 Lab_ Explaining_Phishing_Detectors.pdf"); print("assignment pages",len(p.pages),"text chars",sum(len(x.extract_text() or "") for x in p.pages))'`
> - Ran ``rg -n -F 'ranked = [f for f in shap_top.index' 'lab4_1_explaining_phishing_detectors.ipynb'
> rg -n -F 'explainer(X_test.iloc[:500])' 'lab4_1_explaining_phishing_detectors.ipynb'
> rg -n -F 'LogisticRegression(max_iter=1000)' 'lab4_1_explaining_phishing_detectors.ipynb'
> rg -n -F '[[table B4_top10_lists' 'report\Lab4_1_Report.md'
> rg -n -F '## Appendix: code' 'report\Lab4_1_Report.md'
> rg -n -F 'The notebook `lab4_1_explaining_phishing_detectors.ipynb` runs from top to bottom' 'report\Lab4_1_Report.md'
> rg -n -F 'tree_top.sort_values(ascending=False).index[:10]' 'lab4_1_explaining_phishing_detectors.ipynb'
> rg -n -F 'execution_count": 34' 'lab4_1_explaining_phishing_detectors.ipynb'``
>
> </details>
>
> The core implementation is strong: all 34 code cells have sequential execution counts, no stored errors, and the saved outputs match the report’s headline tables. I’ve found one clear report-compliance gap (the assignment asks for the full top-10 comparison, while the PDF prints only five rows) and I’m checking two methodological/reproducibility risks before assigning an overall rubric assessment.
>
> <details><summary>Ran 3 commands</summary>
>
> - Ran `Get-ChildItem -Force | Select-Object Name,Mode,Length
> Get-Command jupyter -ErrorAction SilentlyContinue | Select-Object Source
> if (Test-Path '.venv\Scripts\python.exe') { & '.venv\Scripts\python.exe' --version; & '.venv\Scripts\python.exe' -c "import sklearn,shap,lime,numpy,pandas; print('deps ok',sklearn.__version__,shap.__version__,numpy.__version__,pandas.__version__)" }`
> - Ran `& 'C:\Users\Owner\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' -c "import sklearn,shap,lime,numpy,pandas,nbconvert; print('deps ok',sklearn.__version__,shap.__version__,numpy.__version__,pandas.__version__,nbconvert.__version__)"`
> - Ran `$target=(Resolve-Path -LiteralPath 'tmp\pdfs').Path
> $workspace=(Resolve-Path -LiteralPath '.').Path
> "target=$target"
> "workspace=$workspace"
> if (-not $target.StartsWith($workspace + [IO.Path]::DirectorySeparatorChar, [StringComparison]::OrdinalIgnoreCase)) { throw 'Refusing cleanup outside workspace' }
> Remove-Item -LiteralPath $target -Recurse -Force
> if ((Test-Path -LiteralPath 'tmp') -and -not (Get-ChildItem -LiteralPath 'tmp' -Force | Select-Object -First 1)) { Remove-Item -LiteralPath 'tmp' -Force }
> "cleanup complete"`
>
> </details>
</details>

Overall, this is strong, likely grade-5 work once the two medium-priority issues below are addressed. I reviewed the complete assignment :codex-file-citation{path="C:\Masters\D7084E-D7041E\Lab4\AI-for-Cybersecurity-Lab4.1\4.1 Lab_ Explaining_Phishing_Detectors.pdf" purpose="source"} and the rendered final report :codex-file-citation{path="C:\Masters\D7084E-D7041E\Lab4\AI-for-Cybersecurity-Lab4.1\report\Lab4_1_Report.pdf" purpose="source"}.

### Findings

1. **[P2] The required top-10 comparison is truncated to five rows.**
   The notebook correctly generates ten features per model, but the report renderer explicitly limits Table 3 to `rows=5` in [Lab4_1_Report.md](<./report/Lab4_1_Report.md:50>). The assignment specifically requests the “top-10 lists (B4).” Show all ten rows, even if placed in an appendix and referenced from the results section.

2. **[P2] BRB feature selection uses test-set explanations.**
   SHAP importance is calculated from the first 500 test samples in [Step B2](<./lab4_1_explaining_phishing_detectors.ipynb:1325>), and that ranking is then used to select the BRB inputs in [Step D1](<./lab4_1_explaining_phishing_detectors.ipynb:2409>). The later validation-set cross-check shows the same features, so the reported result probably does not change, but an after-the-fact check does not remove test leakage. Select `F1` and `F2` from validation SHAP first, then use the test set only for final scoring.

3. **[P3] “random_state=42 everywhere” is not literally true.**
   The three logistic-regression constructions omit `random_state` ([example](<./lab4_1_explaining_phishing_detectors.ipynb:615>)). The current default `lbfgs` solver is effectively deterministic, so this does not invalidate the results, but adding `random_state=42` would align the notebook with the assignment and README claim.

4. **[P3] The page limit is an ambiguous submission risk.**
   The report is five pages: three pages of report plus two pages of code appendix. The main narrative therefore meets the 2–3-page target, and the appendix satisfies the screenshot requirement, but a strict grader could interpret the limit as applying to the whole PDF. The safest option is to confirm appendices are excluded or compress the code evidence.

### What is done well

- All 34 code cells have sequential execution counts and no stored exceptions.
- Correct stratified 60/20/20 split and 342-label budget.
- Both lower lines are present; `y_hidden` is used only for checking pseudo-label accuracy.
- All seven models use the same test set.
- Ablation, LIME stability, SHAP/LIME local comparisons and all nine BRB rules are implemented.
- The report numbers match the notebook outputs and contain no unresolved placeholders.
- The discussion is unusually good: it is honest about unlabelled data, weak LIME fits, attacker-controlled features and the BRB trade-offs.
- The PDF is clean, legible and professionally laid out; AI use and sources are disclosed.

I could not perform a fresh clean execution because the available runtime lacks the project’s ML dependencies. The sequential saved execution, assertions, generated CSVs and matching report provide strong reproducibility evidence, but a final clean “Run All” remains advisable.

- Fix priority findings
- Run final verification
- Create grading checklist
