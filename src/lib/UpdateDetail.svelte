<script>
    import { createEventDispatcher, onMount } from 'svelte';
    import DateTimeFormat from './DateTimeFormat.svelte';
  
    const dispatch = createEventDispatcher();
  
    function goBack() {
       console.log("goBack function triggered");
      dispatch('goBack'); 
    }

    export let selectedUpdate;

    let newComment = '';
    let comments = [];
    let loading = false;

    async function getUpdateComments() {
        try {
        const response = await fetch(`/api/projects/singleProject/${selectedUpdate.project_id}/projectUpdates/${selectedUpdate.id}/comments`, {
            method: 'GET',
            headers: {
            'Content-Type': 'application/json'
            }
        });

        if (!response.ok) {
            throw new Error(response.statusText);
            }

            const data = await response.json();
            comments = data.comments;
        
        
        } catch (error) {
            error = e.message;
            alert(error);
        }finally{
            loading = false;
        }
    }

    async function addUpdateComment() {
        loading = true;
        try {
        const response = await fetch(`/api/projects/singleProject/${selectedUpdate.project_id}/projectUpdates/${selectedUpdate.id}/comments/store`, {
            method: 'POST',
            headers: {
            'Content-Type': 'application/json'
            },
            body: JSON.stringify({ body: newComment }),
        });

        if (!response.ok) {
            throw new Error(response.statusText);
        }

        newComment = '';

        await getUpdateComments();

        alert('Comment added successfully');
        
        
        } catch (error) {
            error = e.message;
            alert(error);
        }finally{
            loading = false;
        }
    }

    onMount(async () => {
        await getUpdateComments();
    })


  </script>


<div class=" h-full px-[18px] flex-col justify-start items-start inline-flex font-['Inter']">
    <div class="self-stretch h-[89px] pb-12 flex-col justify-start items-start flex">
        <div class="px-[19px] py-[11.50px] bg-white border border-[#d1d1d1] justify-center items-center inline-flex">
            <div class="flex items-center justify-start">
                <div class="flex items-center justify-center w-3 h-3">
                    <div class="relative flex flex-col items-start justify-start w-3 h-3"></div>
                </div>
                <button class="inline-flex flex-col items-start justify-start pl-3" on:click={goBack}>
                    <div class="flex flex-col items-center justify-start">
                        
                        <div class="text-center text-[#222222] text-sm font-normal font-['Inter'] leading-[17.50px] flex gap-2 ml-[-10px]">
                            <svg width="13" height="15" viewBox="0 0 13 13" fill="none" xmlns="http://www.w3.org/2000/svg">
                                <g id="icon--chevron-left">
                                <path id="Vector" d="M3.45996 6.65996L8.65996 11.86L9.65996 10.86L5.45996 6.65996L9.65996 2.45996L8.65996 1.45996" fill="black"/>
                                </g>
                                </svg>

                                All Updates</div>
                    </div>
                </button>
            </div>
        </div>
    </div>
    <div class="flex flex-col items-start self-stretch justify-start gap-6">
        <div class="flex flex-col items-start self-stretch justify-start gap-3">
            <div class="inline-flex items-start self-stretch justify-start">
            </div>
            <div class="self-stretch text-[#282828] text-[32px] font-bold font-['Inter'] leading-10">{selectedUpdate.title}</div>
            <div class="self-stretch h-[62px] pb-5 border-b border-[#dcdedd] flex-col justify-start items-start gap-1 flex">
                <div class="inline-flex items-center justify-start gap-3">
                    <img class="w-[42px] h-[42px] relative rounded-[42px] border border-[#dcdedd]" src="https://via.placeholder.com/42x42" alt="" />
                    <div class="w-[120.07px] flex-col justify-start items-start inline-flex">
                        <div class="inline-flex items-center self-stretch justify-start gap-2">
                            <div class="w-[57.07px] h-6 text-[#282828] text-sm font-normal font-['Inter'] leading-normal">{selectedUpdate.userProfile.name}</div>
                            <div class="px-[5px] py-[0.25px] bg-[#05ce78] rounded-[3px] flex-col justify-center items-start inline-flex">
                                <div class="text-white text-xs font-bold font-['Inter'] leading-[18px]">{selectedUpdate.user_id === selectedUpdate.userProfile.user_id ? 'Creator' : 'Member'}</div>
                            </div>
                        </div>
                        <div class="self-stretch h-[18px] text-[#282828]/50 text-[13px] font-normal font-['Inter'] leading-[18px]"><DateTimeFormat date={selectedUpdate.created_at} /></div>
                    </div>
                </div>
            </div>
        </div>
        <div class="self-stretch pb-2 flex-col justify-start items-start gap-[30px] flex">
            <div class="self-stretch text-[#282828] text-base font-normal font-['Inter'] leading-[29px]">{@html selectedUpdate.body} </div>
           
        </div>
    </div>
    
    <div class="self-stretch pt-[42px] flex-col justify-start items-start gap-[18px] flex">
        {#if comments.length > 0}
        <div class="flex flex-col items-start self-stretch justify-start h-5">
            <div class="self-stretch text-[#282828] text-base font-bold font-['Inter'] leading-tight">Comments ({comments.length})</div>
        </div>

        <div class="flex justify-between w-[97%] gap-4 pt-4 pb-8 pl-4">
            <input
                type="text"
                bind:value={newComment}
                placeholder="Add a comment..."
                class="w-full border-2 border-[#dcdedd] rounded-lg px-4 py-2 text-base text-[#0b383c] focus:outline-none focus:border-[#0b383c] transition-colors duration-200"
            />
            <button
                on:click={addUpdateComment}
                class="w-full md:w-1/4 px-4 py-2 bg-[#0b383c] text-white text-base rounded-lg transition-colors duration-300 focus:outline-none focus:border-[#0b383c]"
                disabled={loading}
            >
               {loading ? 'Submitting...' : 'Comment'}
            </button>
        </div>

        
        <div class="self-stretch h-[385px] px-[13px] pt-[15px] pb-[13px] bg-[#fbfbfa] border border-[#e8e8e8] flex-col justify-start items-start gap-3 flex">
            {#each comments as comment}
            <div class="self-stretch h-[113px] px-[19px] pt-[19px] pb-[13px] bg-white border border-[#e8e8e8] flex-col justify-start items-start gap-[18px] flex">
                <div class="inline-flex items-start self-stretch justify-start">
                    <div class="flex items-start self-stretch justify-start gap-3">
                        <div class="inline-flex flex-col items-start justify-start h-9">
                            <img class="w-9 h-9 relative rounded-[36px] border border-[#dcdedd]" src="https://via.placeholder.com/36x36" alt=""/>
                        </div>
                        <div class="w-[195.45px] flex-col justify-start items-start inline-flex">
                            <div class="inline-flex items-center self-stretch justify-start gap-3">
                                <div class="w-[92.93px] h-[21px] text-[#282828] text-sm font-normal font-['Inter'] leading-[21px]">{comment.userProfile.name}</div>
                            </div>
                            <div class="self-stretch h-[18px] flex-col justify-start items-start flex">
                                <div class="text-[#9b9e9e] text-xs font-normal font-['Inter'] leading-[18px]"><DateTimeFormat date={comment.created_at} /></div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="inline-flex items-start self-stretch justify-center">
                    <div class="inline-flex flex-col items-start self-stretch justify-start grow shrink basis-0">
                        <div class="flex flex-col items-start self-stretch justify-start h-6">
                            <div class="self-stretch text-[#282828] text-sm font-normal font-['Inter'] leading-normal">{comment.body}</div>
                        </div>
                    </div>
                </div>
            </div>
            {/each}
        </div>
        {:else}
        <p>No comments</p>
        {/if}
    </div>

    
</div>
