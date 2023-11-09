<script lang="ts">
    import z from "zod"
    
    const LaptopSchema = z.object({
    brand: z.string(),
    name: z.string(),
    weight: z.number()
    })

    type Laptop = z.infer<typeof LaptopSchema>

    export let laptop:Laptop
    let isShowingMore = false

    const showMore = () => {
        isShowingMore = !isShowingMore
    }

</script>

<div>
    <p>
        <span class=" text-3xl">{laptop.name}</span>
        <button class=" btn btn-primary btn-xs" on:click={showMore}>
            {isShowingMore ? "Show less" : "Show more"}
        </button>
    </p>
    {#if isShowingMore}
        <p class=" text-3xl">{laptop.brand}</p>
        <p class=" text-3xl">{laptop.weight}</p>
    {/if}

</div>