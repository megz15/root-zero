<script lang="ts">
    import { create, all} from 'mathjs'
    const math = create(all)

    export let rawExpr = '3*x + sin(x) - e^x'
    $: parsedExpr = math.parse(rawExpr)
    $: f = parsedExpr.compile()

    export let lBd = 0
    export let uBd = 1
</script>

<card class="relative flex flex-col gap-4 p-4 pt-6 w-96 bg-neutral-800 border border-black rounded-lg shadow-md">
    <div class="absolute -top-2 text-sm bg-neutral-950 text-white px-2 rounded-lg border border-gray-400">📏✂️ Regula Falsi Method:</div>
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

    }}>Evaluate & Renew Bounds</button>
</card>