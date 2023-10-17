<script lang="ts">
    let context = false;
    let thesis = false;
    let documents = [
        {num: "", describes: false, supports: false, explains: false},
        {num: "", describes: false, supports: false, explains: false},
        {num: "", describes: false, supports: false, explains: false},
        {num: "", describes: false, supports: false, explains: false},
        {num: "", describes: false, supports: false, explains: false},
        {num: "", describes: false, supports: false, explains: false},
        {num: "", describes: false, supports: false, explains: false},
    ];
    let outside_evidence = false;
    let complexity = false;

    let describes = false;
    let supports = false;
    let explains = false;

    let d = 0;
    let s = 0;
    let e = 0;

    let huff_rubric = {d: 3, s: 6, e: 3};
    let ap_rubric = {d: 3, s: 4, e: 2};
    let rubric = huff_rubric;

    function updateDSE() {
        describes = false;
        supports = false;
        explains = false;
        d = 0;
        s = 0;
        e = 0;
        for (let i = 0; i < documents.length; i++) {
            let doc = documents[i];
            if (doc.describes) d += 1;
            if (doc.supports) s += 1;
            if (doc.explains) e += 1;
        }
        if (d >= rubric.d) describes = true;
        if (s >= rubric.s) supports = true;
        if (e >= rubric.e) explains = true;
        console.log(documents);
    }

    $: documents, updateDSE();

    let points = 0;
    let points_to_grade = [25, 30, 34, 37, 40, 44, 47, 50];

    let grade = 25;

    function updateScore() {
        console.log("score update");
        points = 0;
        if (context) points += 1;
        if (thesis) points += 1;
        if (outside_evidence) points += 1;
        if (describes) points += 1;
        if (supports) points += 1;
        if (explains) points += 1;
        if (complexity) points += 1;
        grade = points_to_grade[points];
    }

    $: context, thesis, outside_evidence, describes, supports, explains, complexity, updateScore();

    $: console.log(context);

    function reset() {
        window.location.reload();
    }

    let new_or_old = "Huff's Rubric";
    let new_rubric = false;

    function updateRubrics() {
        if (new_rubric) {
            new_or_old = "CollegeBoard Rubric (New Rubric)";
            rubric = ap_rubric;
        } else {
            new_or_old = "Huff's Rubric";
            rubric = huff_rubric;
        }
        updateDSE();
        updateScore();
        complexity = false; // reset complexity scores because the rubric entirely changes here
        new_rubric_complexity_criteria = new_rubric_complexity_criteria_base; // see above
    }

    let new_rubric_complexity_criteria_base = [
        {crit:"Exploring nuance by analyzing multiple themes or perspectives",ach:false},
        {crit:"Explaining both similarity and difference, both continuity and change, both causes and effects, multiple causes or effects, multiple similarities or differences, or multiple continuities or changes",ach:false},
        {crit:"Explaining relevant and insightful connections within and across periods or geographical areas",ach:false},
        {crit:"Effectively uses ALL SEVEN documents to support an argument",ach:false},
        {crit:"Effective HAPPY analysis for at least FOUR documents",ach:false},
        {crit:"Uses documents and outside evidence to demonstrate sophisticated understanding",ach:false}
    ];
    let new_rubric_complexity_criteria = new_rubric_complexity_criteria_base;

    function updateNewRubricComplexity() {
        complexity = false;
        for (let i = 0; i < new_rubric_complexity_criteria.length; i++) {
            if (new_rubric_complexity_criteria[i].ach) {
                complexity = true;
            }
        }
        updateDSE();
        updateScore();
    }

    $: new_rubric_complexity_criteria, updateNewRubricComplexity();

    $: new_rubric, updateRubrics();

    let mcq = 50;
    let curve = 0;
</script>

<svelte:head>
    <title>DBQ Checklist!</title>
</svelte:head>

<div class="flex justify-center h-screen space-y-5">
    <div class="mt-2 w-3/4 p-4 card bg-base-200/50 text-center space-y-2 block h-fit">
        <h1 class="text-3xl font-bold">
            Huffman History DBQ Rubric Checklist!
        </h1>
        <h3 class="text-xl font-bold">
            (and test score calculator)
        </h3>
        <p>Checklist with the DBQ rubrics so you can grade yourself on DBQs you write. Supports both the old rubric (Huff's rubric) and the new rubric (used for AP Exams). Also includes a test score calculator so you can figure out what your total test score will be.</p>

        <i>To use the new rubric (the one for the AP exam), scroll to the bottom of the page</i>
        <br>
        <i>Version 1.3.0 - by Tyler/@coredoesdev</i>

        <hr>

        <h2 class="text-xl font-bold">
            Contextualization
        </h2>
        <b>Describes a broader historical context relevant to the prompt</b>
        <p>The response must relate the topic of the prompt to broader historical events, developments, or processes that occur before, during, or continue after the time frame of the question. This point is not awarded for merely a phrase or a reference.</p>
        <div class="form-control text-center">
            <label class="label cursor-pointer text-right justify-end">
                <span class="label-text float-right mr-3">Does the DBQ meet this criteria?</span>
                <input class:checkbox-error={!context} class:checkbox-success={context} type="checkbox" bind:checked={context} class="checkbox float-left" />
            </label>
        </div>

        <hr>

        <h2 class="text-xl font-bold">
            Thesis/Claim
        </h2>
        <b>Responds to the prompt with a historically defensible thesis/claim that establishes a line of reasoning.</b>
        <p>The thesis must make a claim that responds to the prompt, rather than merely restating or rephrasing the prompt. The thesis must consist of one or more sentences located in one place, either the introduction or the conclusion.</p>
        <div class="form-control text-center">
            <label class="label cursor-pointer text-right justify-end">
                <span class="label-text float-right mr-3">Does the DBQ meet this criteria?</span>
                <input class:checkbox-error={!thesis} class:checkbox-success={thesis} type="checkbox" bind:checked={thesis} class="checkbox float-left" />
            </label>
        </div>

        <hr>

        <h2 class="text-xl font-bold">
            Documents
        </h2>

        <table class="table table-fixed">
            <thead>
            <tr>
                <th>Doc #</th>
                <th>Describes</th>
                <th>Supports</th>
                <th>Explains</th>
            </tr>
            </thead>
            <tbody>

            {#each [0, 1, 2, 3, 4, 5, 6] as val}
                <tr>
                    <td><input type="number" class="input w-full p-2" bind:value={documents[val].num} /></td>
                    <td><input type="checkbox" class="checkbox" class:checkbox-error={!describes} class:checkbox-success={describes} bind:checked={documents[val].describes} /></td>
                    <td><input type="checkbox" class="checkbox" class:checkbox-error={!supports} class:checkbox-success={supports} bind:checked={documents[val].supports} /></td>
                    <td><input type="checkbox" class="checkbox" class:checkbox-error={!explains} class:checkbox-success={explains} bind:checked={documents[val].explains} /></td>
                </tr>
            {/each}

            <tr>
                <td>
                    <i>Rubric: {new_or_old}</i>
                </td>
                <td>
                    {#if describes}
                        <p class="text-success">{d}/{rubric.d}</p>
                    {:else}
                        <p class="text-error">{d}/{rubric.d}</p>
                    {/if}
                </td>
                <td>
                    {#if supports}
                        <p class="text-success">{s}/{rubric.s}</p>
                    {:else}
                        <p class="text-error">{s}/{rubric.s}</p>
                    {/if}
                </td>
                <td>
                    {#if explains}
                        <p class="text-success">{e}/{rubric.e}</p>
                    {:else}
                        <p class="text-error">{e}/{rubric.e}</p>
                    {/if}
                </td>
            </tr>

            </tbody>
        </table>

        <p>
            <b>Describes:</b> Accurately describes the content of the document. Quotes are insufficient to earn this point.
        </p>
        <p>
            <b>Supports:</b> Supports an argument in response to the prompt. Documents should meet and exceed the standard set for the description point.
        </p>
        <p>
            <b>Explains:</b> Explains how or why the documentś point of view, purpose, historical situation, and/or audience is relevant to an argument (does it use HAPPY?)
        </p>

        <hr>

        <h2 class="text-xl font-bold">
            Outside Evidence
        </h2>
        <b>Uses at least one piece of outside historical evidence (beyond that found in the documents) relevant to the argument about the prompt.</b>
        <p>The response must describe the evidence and use more than just a phrase or reference. This additional piece of evidence must be different from the evidence used to earn the contextualization point.</p>
        <div class="form-control text-center">
            <label class="label cursor-pointer text-right justify-end">
                <span class="label-text float-right mr-3">Does the DBQ meet this criteria?</span>
                <input class:checkbox-error={!outside_evidence} class:checkbox-success={outside_evidence} type="checkbox" bind:checked={outside_evidence} class="checkbox float-left" />
            </label>
        </div>

        <hr>

        <h2 class="text-xl font-bold">
            Complexity
        </h2>

        {#if new_rubric}
            <b>Demonstrates a complex understanding of the historical development that is the focus of the prompt through sophisticated argumentation and/or effective use of evidence.</b>
            <table class="table">
                <thead>
                <tr>
                    <th>Criteria</th>
                    <th></th>
                </tr>
                </thead>
                <tbody>
                <tr>
                    {#each [0, 1, 2, 3, 4, 5] as val}
                        <tr>
                            <td>{new_rubric_complexity_criteria[val].crit}</td>
                            <td><input type="checkbox" class="checkbox" class:checkbox-error={!complexity} class:checkbox-success={complexity} bind:checked={new_rubric_complexity_criteria[val].ach} /></td>
                        </tr>
                    {/each}
                </tr>
                </tbody>
            </table>
        {:else}
            <b>Demonstrates a complex understanding of the historical development that is the focus of the prompt, using evidence to corroborate, qualify, or modify an argument that addresses the question.</b>
            <p>The response must demonstrate a complex understanding, which must be part of the argument and not merely a phrase or reference. This <i>could</i> include:</p>
            <ul class="list-disc">
                <li class="list-item">Explaining nuance by analyzing multiple variables</li>
                <li class="list-item">Explaining both similarity and difference, both continuity and change, or multiple causes, or both causes and effects</li>
                <li class="list-item">Explaining relevant and insightful connections within and across periods</li>
                <li class="list-item">Confirming the validity of an argument by corroborating multiple perspectives across themes</li>
                <li class="list-item">Qualifying or modifying an argument by considering diverse or alternate views or evidence</li>
            </ul>
            <div class="form-control text-center">
                <label class="label cursor-pointer text-right justify-end">
                    <span class="label-text float-right mr-3">Does the DBQ meet this criteria?</span>
                    <input class:checkbox-error={!complexity} class:checkbox-success={complexity} type="checkbox" bind:checked={complexity} class="checkbox float-left" />
                </label>
            </div>
        {/if}

        <hr>

        <h2 class="text-xl font-bold">
            Summary
        </h2>

        <p>Points: {points}/7</p>
        <p>Grade: {grade}/50 ({(grade/50)*100}%)</p>

        <div class="form-control w-full max-w-xs">
            <label class="label">
                <span class="label-text">MCQ score (/50)</span>
            </label>
            <input type="number" bind:value={mcq} placeholder="45" class="input input-bordered w-full max-w-xs" />
        </div>

        <div class="form-control w-full max-w-xs">
            <label class="label">
                <span class="label-text">Curve (points)</span>
            </label>
            <input type="number" bind:value={curve} placeholder="0" class="input input-bordered w-full max-w-xs" />
        </div>

        <p>Test Grade (including MCQ): {grade + mcq}/100</p>
        <p>Test Grade (MCQ+Curve): {grade+mcq+curve}/100</p>

        <button on:click={reset} class="btn btn-primary">Reset</button>

        <div class="justify-center form-control w-1/3">
            <label class="label cursor-pointer">
                <span class="label-text">{new_or_old}</span>
                <input type="checkbox" class="toggle" bind:checked={new_rubric} />
            </label>
        </div>

        <i>Switching rubrics will reset scores for the complexity point. All other scores will remain unaffected.</i>

        <div class="collapse collapse-arrow bg-base-200">
            <input type="checkbox" name="changelog" />
            <div class="collapse-title text-xl font-medium">
                Changelog
            </div>
            <div class="collapse-content">
                <h3 class="text-lg">1.3.0</h3>
                <p>Adds description of criteria for Describes, Supports, and Explains in the Document section (thanks Charlie Bragg for the idea!)</p>
                <h3 class="text-lg">1.2.2</h3>
                <p>Add the changelog</p>
                <h3 class="text-lg">1.2.1</h3>
                <p>Add new rubric, and the test score calculator (thanks Lucas Shah for the idea!)</p>
                <h3 class="text-lg">1.2.0</h3>
                <p>Improve support for mobile devices and add new rubric</p>
                <h3 class="text-lg">1.1.0</h3>
                <p>URL changed to dbq.coredoes.dev instead of old pages.dev url</p>
                <h3 class="text-lg">1.0.0</h3>
                <p>Initial release</p>
            </div>
        </div>
    </div>
</div>


<style lang="postcss">
    :global(html) {
        background-color: theme(colors.gray.100);
    }
</style>
