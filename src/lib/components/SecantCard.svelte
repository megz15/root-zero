<script lang="ts">
    import { create, all} from 'mathjs'
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

<card class="relative flex flex-col gap-4 p-4 pt-7 w-96 bg-neutral-800 border border-black rounded-lg shadow-md">
    <div class="absolute -top-2 bg-neutral-950 text-white px-2 rounded-lg border border-gray-400">📏 Secant Method:</div>
    <input bind:value={rawExpr} placeholder="Expression" class="bg-gray-100 border border-gray-400 text-gray-900 text-sm rounded p-2.5" />
    <div class="flex justify-between items-center">
        <div class="text-white">Initial Guesses:</div>
        <input type="number" bind:value={x0} placeholder="First Guess" class="bg-gray-100 border border-gray-400 text-gray-900 text-sm rounded p-2.5 w-20" />
        <input type="number" bind:value={x1} placeholder="Second Guess" class="bg-gray-100 border border-gray-400 text-gray-900 text-sm rounded p-2.5 w-20" />
    </div>

    <div class="flex gap-2">
        <button class="focus:outline-none text-black bg-lime-500 rounded-lg text-sm px-5 py-2.5" on:click={() => {
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
        <input readonly type="number" bind:value={xn} placeholder="Next Approximation" class="bg-gray-100 border border-gray-400 text-gray-900 text-sm rounded p-2.5 grow" />
    </div>

    <div class="text-white text-right">Clicks to Converge: {clicksToConverge}</div>
</card>