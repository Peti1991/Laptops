<script  lang="ts">
  import { onMount } from 'svelte';

  import { getLaptops } from './api/index';
  import LoadingMask from './components/LoadingMask.svelte';
  import Laptop from './components/Laptop.svelte'
  import { type LaptopType } from './api/index';
  

  let searchValue = ""
  let isLoading = true
  let isSorted = false

  let laptops: LaptopType[] = []
  let searchedLaptops: LaptopType[] = []
  $: searchedLaptops = searchValue ? laptops.filter( laptop => laptop.name.includes(searchValue)) : laptops

  onMount (async () => {
    isLoading = true
    const response = await getLaptops()
    isLoading = false

    if (!response.success) {
      return alert(`Could not load messages (${response.status})`)
    }

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

