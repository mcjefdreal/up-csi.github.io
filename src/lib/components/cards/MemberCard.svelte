<script lang="ts">
    import { fade } from 'svelte/transition';

    import type { Member } from '$lib/models/member';

    import MemberCardTag from '$lib/components/cards/MemberCardTag.svelte';

    interface Props {
        member: Member;
        color: string;
    }

    const { member, color }: Props = $props();
    const { name, src } = $derived(member);
    let isOverlayVisible = $state(false);
</script>

<!-- TODO: Prefer using `hover:` modifiers in native CSS. -->
<div
    class="grid-rows-[1fr auto] relative grid grid-cols-1 content-start items-start overflow-hidden *:m-0 md:aspect-square md:grid-rows-1 md:rounded-2xl md:shadow-lg"
    role="img"
    onmouseenter={() => (isOverlayVisible = true)}
    onmouseleave={() => (isOverlayVisible = false)}
>
    <enhanced:img
        {src}
        alt={name}
        height="300px"
        loading="lazy"
        class="col-start-1 row-start-1 m-0 aspect-square h-56 w-full rounded-2xl object-cover md:h-full"
    />
    {#if isOverlayVisible}
        <div
            class="absolute z-10 col-start-1 row-start-1 hidden h-full w-full flex-col justify-end gap-2 bg-csi-black/70 p-4 text-csi-white md:flex"
            transition:fade={{ duration: 75 }}
        >
            <MemberCardTag {member} />
        </div>
    {:else}
        <div
            class="absolute z-10 col-start-1 row-start-1 hidden h-full w-full flex-col justify-end p-1 md:flex"
            in:fade={{ duration: 75 }}
        >
            <div class="h-fit w-fit rounded-full px-3 py-1 {color}">
                <p class="m-0 text-sm text-csi-black">{name}</p>
            </div>
        </div>
    {/if}

    <div class="col-start-1 row-start-2 flex flex-col gap-2 md:hidden">
        <MemberCardTag {member} />
    </div>
</div>
