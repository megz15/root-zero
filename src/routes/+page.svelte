<script lang="ts">
    import BisectionCard from '$lib/components/BisectionCard.svelte';
    import SecantCard from '$lib/components/SecantCard.svelte';
    import RegulaFalsiCard from '$lib/components/RegulaFalsiCard.svelte';
    import NewtonRaphsonCard from '$lib/components/NewtonRaphsonCard.svelte';

    let rawExpr = '3*x + sin(x) - e^x'

    // Initial Bound Guesses
    let initLBd = 0
    let initUBd = 1

    // Initial Root Guesses
    let initGuess0 = 0
    let initGuess1 = 1
</script>

<main class="p-8 bg-neutral-900 min-h-screen flex flex-col items-center justify-center gap-8">
    <h1 class="text-4xl font-bold text-white">Zero Hero 🦸‍♂️</h1>
    <div class="flex flex-wrap items-center justify-center gap-4">
        <BisectionCard rawExpr={rawExpr} lBd={initLBd} uBd={initUBd} />
        <SecantCard rawExpr={rawExpr} x0={initGuess0} x1={initGuess1} />
        <RegulaFalsiCard rawExpr={rawExpr} lBd={initLBd} uBd={initUBd} />
        <NewtonRaphsonCard rawExpr={rawExpr} x0={initGuess0} />
    </div>
</main>