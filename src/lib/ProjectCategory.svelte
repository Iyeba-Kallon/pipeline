<!-- <script>
  const categories = [
    { name: 'All projects', active: true },
    { name: 'Design', active: false },
    { name: 'Engineering', active: false },
    { name: 'Art', active: false },
    { name: 'Development', active: false }
  ];
</script>

<nav class="flex flex-wrap items-center self-start gap-4 mt-8 text-sm leading-none text-lime-800 max-md:mt-6 max-md:max-w-full" aria-label="Project Categories">
  {#each categories as category}
    <button
      class="px-10 py-3 text-xl my-auto whitespace-nowrap border-lime-800 border-solid border-2 rounded-full transition-colors duration-300 hover:bg-lime-200 {category.active ? 'bg-lime-300' : ''}"
      aria-current={category.active ? 'page' : undefined}
    >
      {category.name}
    </button>
  {/each}
</nav> -->

<script>
  import { onMount } from "svelte";
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();
  
  let loading = true;

  let categories = [];

  async function fetchAllCategories() {
    try {
      const response = await fetch('/api/categories', {
        method: 'GET',
        headers: {
            'Content-Type': 'application/json'
        }
      });

      if (!response.ok) {
        throw new Error(response.statusText);
      }

      const data = await response.json();

      categories = [
        { id: 0, title: "All Projects", active: true },
        ...data.categories.map(category => ({ ...category, active: false }))
      ];
    } catch (error) {
      
    }
  }

  onMount(() => {
    fetchAllCategories();
  })

  function setActiveCategory(selectedCategory) {

    if (selectedCategory.id === 0) {
      setAllProjects();
      return;
    }

    categories = categories.map(category => ({
      ...category,
      active: category.title === selectedCategory.title
    }));

    dispatch('categorySelected', selectedCategory);

  }

  function setAllProjects(){
    categories = categories.map(category => ({ ...category, active: false }));
    dispatch('categorySelected', '');
  }


</script>

<nav
  class="flex flex-wrap items-center max-w-full gap-4 pb-4 mt-8 overflow-x-auto text-sm leading-none text-lime-800"
  style="scroll-snap-type: x mandatory;"
  aria-label="Project Categories"
>

  {#each categories as category}
    <button
      class="px-10 py-3 text-xl my-auto whitespace-nowrap border-lime-800 border-solid border-2 rounded-full transition-colors duration-300 hover:bg-lime-200 {category.active ? 'bg-lime-300' : ''}"
      aria-current={category.active ? 'page' : undefined}
      on:click={() => setActiveCategory(category)}
      style="scroll-snap-align: start;"
    >
      {category.title}
    </button>
  {/each}
</nav>
<style>
  nav {
    scrollbar-width: thin;
    margin-bottom: 5px;
    scrollbar-color: #a3e635 #f3f4f6; /* Adjust scrollbar colors for Firefox */
  }

  /* Customize scrollbar for WebKit browsers */
  nav::-webkit-scrollbar {
    height: 8px;
  }

  nav::-webkit-scrollbar-track {
    background: #f3f4f6;
  }

  nav::-webkit-scrollbar-thumb {
    background-color: #a3e635;
    border-radius: 10px;
  }
</style>
