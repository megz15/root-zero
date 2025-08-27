<script lang="ts">
    import { create, all} from 'mathjs'
    import Card from './Card.svelte';
    const math = create(all)

    export let rawExpr = '3*x + sin(x) - e^x'
    $: parsedExpr = math.parse(rawExpr)
    $: f = parsedExpr.compile()

    export let lBd = 0
    export let uBd = 1

    let clicksToConverge = 0
</script>

<Card title="✂️ Bisection Method:">
    <input autocomplete="off" bind:value={rawExpr} placeholder="Expression" />
    <div class="flex justify-between items-center">
        <div class="text-white">Bounds:</div>
        <input autocomplete="off" type="number" bind:value={lBd} placeholder="Lower Bound" class="max-w-32 max-sm:max-w-24" />
        <input autocomplete="off" type="number" bind:value={uBd} placeholder="Upper Bound" class="max-w-32 max-sm:max-w-24" />
    </div>

    <button on:click={() => {
        const fAtlBd = f.evaluate({ x: lBd })
        const fAtuBd = f.evaluate({ x: uBd })

        if (math.sign(fAtlBd * fAtuBd) != -1) {
            alert("The function must have opposite signs at the bounds (Intermediate Value Theorem)")
            return
        }

        const midPt = (lBd + uBd) / 2
        const fAtMidPt = f.evaluate({ x: midPt })

        if (math.sign(fAtMidPt) == 0) {
            alert(`Exact root found at x = ${midPt}`)
            return
        } else if (math.sign(fAtMidPt * fAtlBd) == -1) {
            uBd = midPt
        } else if (math.sign(fAtMidPt * fAtuBd) == -1) {
            lBd = midPt
        }

        clicksToConverge++

    }}>Evaluate & Renew Bounds</button>

    <div class="text-white text-right">Clicks to Converge: {clicksToConverge}</div>
</Card>