<script lang="ts">
    import { create, all} from 'mathjs'
    import Card from './Card.svelte'
    const math = create(all)

    export let rawExpr = '2,0,-3,3,-4'

    export let x0 = -2
    let xn = 0
    let clicksToConverge = 0
</script>

<Card title="📯 Horner's:">
    <div class="flex justify-between items-center gap-2">
        <div class="text-white">Polynomial Coeff:</div>
        <input autocomplete="off" bind:value={rawExpr} placeholder="Comma-separated coefficient values" class="grow w-16" />
    </div>

    <div class="flex justify-between items-center gap-2">
        <div class="text-white">Guess:</div>
        <input autocomplete="off" type="number" bind:value={x0} placeholder="Initial Guess" class="grow w-16" />
    </div>

    <div class="flex gap-2 justify-between">
        <button on:click={() => {

            const coeffs = rawExpr.split(",").map(c => Number(c))
            const degree = coeffs.length - 1

            let R = [coeffs[0] ?? 0]
            for (let i = 0; i < degree; i++) {
                R.push(R[i] * x0 + coeffs[i+1])
            }

            const Pn = R.pop() ?? 0

            let Q = [R[0] ?? 0]
            for (let i = 0; i < degree - 1; i++) {
                Q.push(Q[i] * x0 + R[i+1])
            }

            xn = x0 - Pn / Q[degree - 1]
            
            if (x0 == xn) {
                alert(`Exact root found at x = ${x0}`)
                return
            }
            
            x0 = xn
            
            clicksToConverge++

        }}>Evaluate</button>
        <input autocomplete="off" readonly type="number" bind:value={xn} placeholder="Next Approximation" class="grow max-sm:max-w-36" />
    </div>

    <div class="text-white text-right">Clicks to Converge: {clicksToConverge}</div>
</Card>