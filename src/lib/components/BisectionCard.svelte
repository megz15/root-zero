<script lang="ts">
    import { create, all} from 'mathjs'
    const math = create(all)

    export let rawExpr = '3*x + sin(x) - e^x'
    $: parsedExpr = math.parse(rawExpr)
    $: f = parsedExpr.compile()

    export let lBd = 0
    export let uBd = 1

    let clicksToConverge = 0
</script>

<card class="relative flex flex-col gap-4 p-4 pt-7 w-96 bg-neutral-800 border border-black rounded-lg shadow-md">
    <div class="absolute -top-2 bg-neutral-950 text-white px-2 rounded-lg border border-gray-400">✂️ Bisection Method:</div>
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

        clicksToConverge++

    }}>Evaluate at Midpoint & Renew Bounds</button>

    <div class="text-white text-right">Clicks to Converge: {clicksToConverge}</div>
</card>