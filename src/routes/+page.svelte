<script lang="ts">
    import icon from '$lib/assets/favicon.png'

    import BisectionCard from '$lib/components/BisectionCard.svelte'
    import SecantCard from '$lib/components/SecantCard.svelte'
    import RegulaFalsiCard from '$lib/components/RegulaFalsiCard.svelte'
    import NewtonRaphsonCard from '$lib/components/NewtonRaphsonCard.svelte'
    import FixedPointCard from '$lib/components/FixedPointCard.svelte';
    import ModifiedNewtonCard from '$lib/components/ModifiedNewtonCard.svelte';
    import MullerCard from '$lib/components/MullerCard.svelte';
    import HornerCard from '$lib/components/HornerCard.svelte';

    const rawExpr = '3*x + sin(x) - e^x'

    // Initial Bound Guesses
    const initLBd = 0
    const initUBd = 1

    // Initial Root Guesses
    const initGuess0 = 0
    const initGuess1 = 1
</script>

<main class="p-8 bg-neutral-900 min-h-screen flex flex-col items-center justify-center gap-8">
    <div class="flex gap-4 justify-center">
        <img src={icon} alt="App Icon" class="w-16 h-16 rotate-45" />
        <h1 class="text-4xl font-bold text-white text-center">Root Finder</h1>
        <img src={icon} alt="App Icon" class="w-16 h-16 rotate-315" />
    </div>
    <div class="grid grid-cols-2 max-[850px]:grid-cols-1 gap-4">
        <BisectionCard rawExpr={rawExpr} lBd={initLBd} uBd={initUBd} />
        <SecantCard rawExpr={rawExpr} x0={initGuess0} x1={initGuess1} />
        <RegulaFalsiCard rawExpr={rawExpr} lBd={initLBd} uBd={initUBd} />
        <NewtonRaphsonCard rawExpr={rawExpr} x0={initGuess0} />
        <FixedPointCard x0={initGuess0} />
        <ModifiedNewtonCard rawExpr={rawExpr} x0={initGuess0} />
        <MullerCard rawExpr={rawExpr} />
        <HornerCard />
    </div>
</main>