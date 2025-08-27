<script lang="ts">
    import BisectionCard from '$lib/components/BisectionCard.svelte';
    import SecantCard from '$lib/components/SecantCard.svelte';

    let rawExpr = '3*x + sin(x) - e^x'

    // Bisection
    let bisection_lBd = 0
    let bisection_uBd = 1

    // Secant
    let secant_initGuess0 = 0
    let secant_initGuess1 = 1
</script>

<main class="flex flex-col items-center justify-center min-h-screen p-4 gap-4">
    <BisectionCard rawExpr={rawExpr} lBd={bisection_lBd} uBd={bisection_uBd} />
    <SecantCard rawExpr={rawExpr} x0={secant_initGuess0} x1={secant_initGuess1} />
</main>