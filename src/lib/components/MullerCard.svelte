<script lang="ts">
    import { create, all, derivative, sqrt} from 'mathjs'
    import Card from './Card.svelte';
    const math = create(all)

    export let rawExpr = '3x + sin(x) - e^x'
    $: parsedExpr = math.parse(rawExpr)
    $: f = parsedExpr.compile()

    export let x0 = 0
    export let x1 = 0.25
    export let x2 = 0.5
    let xn = 0

    let clicksToConverge = 0
</script>

<Card title="⚪ Muller's:">
    <input autocomplete="off" bind:value={rawExpr} placeholder="Expression" />
    <div class="flex justify-between items-center gap-2">
        <div class="text-white">Guesses:</div>
        <input autocomplete="off" type="number" bind:value={x0} placeholder="First Guess" class="grow w-16" />
        <input autocomplete="off" type="number" bind:value={x1} placeholder="Second Guess" class="grow w-16" />
        <input autocomplete="off" type="number" bind:value={x2} placeholder="Third Guess" class="grow w-16" />
    </div>

    <div class="flex gap-2 justify-between">
        <button on:click={() => {
            const fAtx0 = f.evaluate({ x: x0 })
            const fAtx1 = f.evaluate({ x: x1 })
            const fAtx2 = f.evaluate({ x: x2 })

            if (fAtx0 == 0) {
                alert(`Exact root found at x = ${x0}`)
                return
            }
            if (fAtx1 == 0) {
                alert(`Exact root found at x = ${x1}`)
                return
            }
            if (fAtx2 == 0) {
                alert(`Exact root found at x = ${x2}`)
                return
            }

            const a = (fAtx2 * (x1 - x0) + fAtx1 * (x0 - x2) + fAtx0 * (x2 - x1)) / ((x2 - x0) * (x2 - x1) * (x1 - x0))
            const b = (fAtx2 - fAtx1)/(x2 - x1) + a * (x2 - x1)
            const c = fAtx2

            xn = x2 - 2*c / (b + math.sign(b) * math.complex(math.sqrt(b * b - 4 * a * c)).re)
            x0 = x1
            x1 = x2
            x2 = xn

            clicksToConverge++

        }}>Evaluate</button>
        <input autocomplete="off" readonly type="number" bind:value={xn} placeholder="Next Approximation" class="grow max-sm:max-w-36" />
    </div>

    <div class="text-white text-right">Clicks to Converge: {clicksToConverge}</div>
</Card>