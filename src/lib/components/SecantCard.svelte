<script lang="ts">
    import { create, all} from 'mathjs'
    import Card from './Card.svelte';
    const math = create(all)

    export let rawExpr = '3*x + sin(x) - e^x'
    $: parsedExpr = math.parse(rawExpr)
    $: f = parsedExpr.compile()

    export let x0 = 0
    export let x1 = 1
    let isInitialGuess = true
    let xn = 0

    let clicksToConverge = 0
</script>

<Card title="📏 Secant:">
    <input autocomplete="off" bind:value={rawExpr} placeholder="Expression" />
    <div class="flex justify-between items-center gap-2">
        <div class="text-white">Guesses:</div>
        <input autocomplete="off" type="number" bind:value={x0} placeholder="First Guess" class="grow max-w-28 max-sm:max-w-22" />
        <input autocomplete="off" type="number" bind:value={x1} placeholder="Second Guess" class="grow max-w-28 max-sm:max-w-22" />
    </div>

    <div class="flex gap-2 justify-between">
        <button on:click={() => {
            const fAtx0 = f.evaluate({ x: x0 })
            const fAtx1 = f.evaluate({ x: x1 })

            if (fAtx0 == fAtx1) {
                if (fAtx0 == 0) {
                    alert(`Exact root found at x = ${x0}`)
                    return
                }
                alert("Values converged, try different guesses")
                xn = x1
                return
            }

            if (isInitialGuess) {
                if ((math.abs(fAtx1) < math.abs(fAtx0))) [x0, x1] = [x1, x0]
                isInitialGuess = false
            }

            xn = x1 - fAtx1 * (x1 - x0) / (fAtx1 - fAtx0)
            x0 = x1
            x1 = xn

            clicksToConverge++

        }}>Evaluate</button>
        <input autocomplete="off" readonly type="number" bind:value={xn} placeholder="Next Approximation" class="grow max-sm:max-w-36" />
    </div>

    <div class="text-white text-right">Clicks to Converge: {clicksToConverge}</div>
</Card>