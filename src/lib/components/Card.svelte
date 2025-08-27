<script lang="ts">
    export let title: string
</script>

<card class="relative flex flex-col gap-4 px-6 pt-6 w-96 max-sm:w-80 h-60 bg-neutral-800 border border-black rounded-lg shadow-md">
    <div class="absolute -top-2 bg-neutral-950 text-white px-2 rounded-lg border border-gray-400">{title}</div>
    <slot/>
</card>