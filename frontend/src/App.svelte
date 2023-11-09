<script  lang="ts">
  import { onMount } from 'svelte';
  import { makeServer } from './api'
  makeServer({ environment: 'development' })
  import axios from "axios"
  import z from "zod"

  import LoadingMask from './components/LoadingMask.svelte';
  import Laptop from './components/Laptop.svelte'

  const LaptopSchema = z.object({
    brand: z.string(),
    name: z.string(),
    weight: z.number()
  })
  
  type Laptop = z.infer<typeof LaptopSchema>
  

  let searchValue = ""
  let isLoading = true
  let isSorted = false

  let laptops: Laptop[] = []
  let searchedLaptops: Laptop[] = []
  $: searchedLaptops = searchValue ? laptops.filter( laptop => laptop.name.includes(searchValue)) : laptops

  onMount (async () => {
    isLoading = true
    const response = await axios.get("https://demoapi.com/api/laptop")
    isLoading = false

    /* const responseResult = LaptopSchema.safeParse(response)
    if (!responseResult.success) {
      laptops = []
    }else {
      laptops = responseResult.data
    } */

    laptops = response.data
  })
  
  
  const sort = () => {
    searchedLaptops = searchedLaptops.sort((a,b) =>{
      return isSorted ? a.weight - b.weight : b.weight - a.weight
    })
    isSorted = !isSorted
  }

  
</script>

<title>Laptops</title>

<header class=" flex flex-row justify-center gap-8">
  <input type="text" bind:value={searchValue}>
  <button class=" btn btn-primary" on:click={sort}>Sort</button>
</header>

<main class=" mt-20 flex flex-row justify-center ">
  {#if isLoading}
    <LoadingMask />
  {/if}
  <div class=" flex flex-col gap-10">
    {#each searchedLaptops as laptop}
      <Laptop laptop={laptop} />
    {/each}
  </div>
</main>

