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
        if (d >= 3) describes = true;
        if (s >= 6) supports = true;
        if (e >= 3) explains = true;
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
</script>

<div class="flex justify-center h-screen space-y-5">
    <div class="mt-2 p-4 w-1/2 card bg-base-200/50 text-center space-y-2 block h-fit">
        <h1 class="text-3xl font-bold">
            Huffman History DBQ Rubric Checklist!
        </h1>
        <p>Checklist with the DBQ rubric so I can distract myself from actually self-grading my DBQ.</p>

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

        <table class="table">
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
                    <td><input type="number" bind:value={documents[val].num} /></td>
                    <td><input type="checkbox" class="checkbox" class:checkbox-error={d < 3} class:checkbox-success={d >= 3} bind:checked={documents[val].describes} /></td>
                    <td><input type="checkbox" class="checkbox" class:checkbox-error={s < 6} class:checkbox-success={s >= 6} bind:checked={documents[val].supports} /></td>
                    <td><input type="checkbox" class="checkbox" class:checkbox-error={e < 3} class:checkbox-success={e >= 3} bind:checked={documents[val].explains} /></td>
                </tr>
            {/each}

            <tr>
                <td></td>
                <td>
                    {#if describes}
                        <p class="text-success">{d}/3</p>
                    {:else}
                        <p class="text-error">{d}/3</p>
                    {/if}
                </td>
                <td>
                    {#if supports}
                        <p class="text-success">{s}/6</p>
                    {:else}
                        <p class="text-error">{s}/6</p>
                    {/if}
                </td>
                <td>
                    {#if explains}
                        <p class="text-success">{e}/3</p>
                    {:else}
                        <p class="text-error">{e}/3</p>
                    {/if}
                </td>
            </tr>

            </tbody>
        </table>

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

        <hr>

        <h2 class="text-xl font-bold">
            Summary
        </h2>

        <p>Points: {points}/7</p>
        <p>Grade: {grade}/50</p>

        <button on:click={reset} class="btn btn-primary">Reset</button>
    </div>
</div>


<style lang="postcss">
    :global(html) {
        background-color: theme(colors.gray.100);
    }
</style>
