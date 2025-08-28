<script lang="ts">
    import { create, all} from 'mathjs'
    import Card from './Card.svelte';
    const math = create(all)

    export let rawExpr = '(e^x - sin(x)) / 3'
    $: parsedExpr = math.parse(rawExpr)
    $: f = parsedExpr.compile()

    export let x0 = 0
    let xn = 0

    let clicksToConverge = 0
</script>

<Card title="🎯 Fixed Point Iterations:">
    <input autocomplete="off" bind:value={rawExpr} placeholder="Expression" />
    <div class="flex justify-between items-center gap-2">
        <div class="text-white">Guess:</div>
        <input autocomplete="off" type="number" bind:value={x0} placeholder="First Guess" class="grow" />
    </div>

    <div class="flex gap-2 justify-between">
        <button on:click={() => {
            const fAtx0 = f.evaluate({ x: x0 })

            if (fAtx0 == x0) {
                alert(`Exact root found at x = ${x0}`)
                return
            }

            xn = fAtx0
            x0 = xn

            clicksToConverge++

        }}>Evaluate</button>
        <input autocomplete="off" readonly type="number" bind:value={xn} placeholder="Next Approximation" class="grow max-sm:max-w-36" />
    </div>

    <div class="text-white text-right">Clicks to Converge: {clicksToConverge}</div>
</Card>