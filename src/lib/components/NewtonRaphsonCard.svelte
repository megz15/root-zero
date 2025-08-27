<script lang="ts">
    import { create, all} from 'mathjs'
    const math = create(all)

    export let rawExpr = '3*x + sin(x) - e^x'
    $: parsedExpr = math.parse(rawExpr)
    $: f = parsedExpr.compile()

    $: derivativeOfParsedExpr = math.derivative(parsedExpr, 'x')
    $: fPrime = derivativeOfParsedExpr.compile()

    export let x0 = 0
    let xn = 0
</script>

<card class="relative flex flex-col gap-4 p-4 pt-6 w-96 bg-neutral-800 border border-black rounded-lg shadow-md">
    <div class="absolute -top-2 text-sm bg-neutral-950 text-white px-2 rounded-lg border border-gray-400">🍎 Newton-Raphson Method:</div>
    <input bind:value={rawExpr} placeholder="Expression" class="bg-gray-100 border border-gray-400 text-gray-900 text-sm rounded p-2.5" />
    <div class="flex justify-between items-center">
        <div class="text-white">Initial Guess:</div>
        <input type="number" bind:value={x0} placeholder="First Guess" class="bg-gray-100 border border-gray-400 text-gray-900 text-sm rounded p-2.5" />
    </div>

    <div class="flex gap-2">
        <button class="focus:outline-none text-black bg-lime-500 rounded-lg text-sm px-5 py-2.5" on:click={() => {
            const fAtx0 = f.evaluate({ x: x0 })

            if (fAtx0 == 0) {
                alert(`Exact root found at x = ${x0}`)
                return
            }

            const fPrimeAtx0 = fPrime.evaluate({ x: x0 })
            if (fPrimeAtx0 == 0) {
                alert("Derivative is zero, try different guesses")
                xn = x0
                return
            }

            xn = x0 - fAtx0 / fPrimeAtx0
            x0 = xn

        }}>Evaluate</button>
        <input readonly type="number" bind:value={xn} placeholder="Next Approximation" class="bg-gray-100 border border-gray-400 text-gray-900 text-sm rounded p-2.5 grow" />
    </div>
</card>