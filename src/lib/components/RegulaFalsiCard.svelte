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

<Card title="✂️📏 Regula Falsi:">
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

        // Instead of midpoint, we use the secant formula here

        const x2 = uBd - fAtuBd * (uBd - lBd) / (fAtuBd - fAtlBd)
        const fAtx2 = f.evaluate({ x: x2 })

        if (math.sign(fAtx2) == 0) {
            alert(`Exact root found at x = ${x2}`)
            return
        } else if (math.sign(fAtx2 * fAtlBd) == -1) {
            uBd = x2
        } else if (math.sign(fAtx2 * fAtuBd) == -1) {
            lBd = x2
        }

        clicksToConverge++

    }}>Evaluate & Renew Bounds</button>

    <div class="text-white text-right">Clicks to Converge: {clicksToConverge}</div>
</Card>