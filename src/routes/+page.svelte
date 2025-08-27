<script lang="ts">
    import { create, all } from 'mathjs'
    const math = create(all)

    let rawExpr = '3*x^2 + sin(x) - exp(x)'
    let lBd = 0
    let uBd = 1
    
    $: parsedExpr = math.parse(rawExpr)
    $: f = parsedExpr.compile()
</script>

<main class="flex flex-col items-center justify-center min-h-screen p-4 gap-8">
    <card class="flex flex-col gap-4 p-4 w-96 bg-neutral-800 border border-black rounded-lg shadow-md">
        <input bind:value={rawExpr} placeholder="Expression" class="bg-gray-100 border border-gray-400 text-gray-900 text-sm rounded p-2.5" />
        <div class="flex justify-between items-center">
            <div class="text-white">Bounds:</div>
            <input type="number" bind:value={lBd} placeholder="Lower Bound" class="bg-gray-100 border border-gray-400 text-gray-900 text-sm rounded p-2.5 w-32" />
            <input type="number" bind:value={uBd} placeholder="Upper Bound" class="bg-gray-100 border border-gray-400 text-gray-900 text-sm rounded p-2.5 w-32" />
        </div>

        <button class="focus:outline-none text-black bg-lime-500 rounded-lg text-sm px-5 py-2.5" on:click={() => {
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

        }}>Evaluate at Midpoint</button>
    </card>

</main>