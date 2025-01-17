<script>
  import ProgressBar from './ProgressBar.svelte';
  import ContributeButton from './ContributeButton.svelte';
  import CategoryTag from './CategoryTag.svelte';
  import DPGRating from './DPGRating.svelte';
  import { amountFormat } from '$lib/utils/amountFormat.js';

  import { onMount } from 'svelte';
  let isOpen = false;
  let imageNumber = String(Math.floor(Math.random() * 17) + 1).padStart(2, '0');

  function toggleDropdown() {
    isOpen = !isOpen;
  }

  function closeDropdown(event) {
    if (event.target.closest('.dropdown-container')) return;
    isOpen = false;
  }

  export let project;

  onMount(() => {
    document.addEventListener('click', closeDropdown);
    return () => {
      document.removeEventListener('click', closeDropdown);
    };
  });
</script>

<div class="flex flex-col overflow-hidden bg-gray-200 rounded-3xl">
  <header class="relative pt-[75%]">
    <a href="/project/{project.id}" class="absolute inset-0 flex items-center justify-center">
      <div class="w-[95%] h-[90%] overflow-hidden rounded-3xl">
        <img
          loading="lazy"
          src={`https://sdgs.un.org/sites/default/files/goals/E_SDG_Icons-${imageNumber}.jpg`}
          alt=""
          class="object-contain w-full h-full"
        />
      </div>
    </a>
  </header>

  <div class="p-4 mt-auto">
    <div class="flex items-center justify-between mb-2">
      <h2 class="text-xl font-semibold text-black">{project.title}</h2>
      <p class="text-xs text-neutral-400">
        <DPGRating rating={project.dpgStatusCount} />
      </p>
    </div>

    <div class="flex gap-2 ml-[-2px] mb-4">
      {#if project.tags.length > 0}
        {#each project.tags as tag}
          <CategoryTag {tag} />
        {/each}
      {:else}
        <p></p>
      {/if}
    </div>

    <div>
      <span class="text-sm font-semibold">${amountFormat(project.current_funding || 0)}</span>
      raised of
      <span class="text-sm font-semibold">${amountFormat(project.funding_goal || 0)}</span>
    </div>
    <ProgressBar progress={project.current_funding} total={project.funding_goal} />
    <ContributeButton {project} />
  </div>
</div>
