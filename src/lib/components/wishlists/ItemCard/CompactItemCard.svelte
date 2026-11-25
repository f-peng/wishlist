<script lang="ts">
    import type { InternalItemCardProps } from "./ItemCard.svelte";
    import type { MessageFormatter } from "$lib/server/i18n";
    import { formatPrice } from "$lib/price-formatter";
    import ClaimButtons from "./ClaimButtons.svelte";
    import ManageButtons from "./ManageButtons.svelte";
    import ReorderButtons from "./ReorderButtons.svelte";
    import ItemImage from "./components/ItemImage.svelte";

    const { defaultImage: _defaultImage, ...props }: InternalItemCardProps = $props();
</script>

<!-- One row per item: thumbnail, name and price, then the claim / manage buttons -->
<div class="flex flex-wrap items-center gap-x-3 gap-y-1 p-2 print:py-1">
    <ItemImage class="aspect-square size-10 flex-none rounded object-cover" badgeClass="hidden!" item={props.item}>
        {#snippet defaultImage(t: MessageFormatter)}
            {@render _defaultImage(t, "aspect-square size-10 rounded")}
        {/snippet}
    </ItemImage>

    <div class="flex min-w-0 flex-1 basis-40 items-baseline gap-x-2">
        {#if props.item.mostWanted}
            <iconify-icon class="text-primary-500 flex-none" icon="ion:star"></iconify-icon>
        {/if}
        {#if props.item.url}
            <a
                id={`${props.id}-name`}
                class="anchor text-primary-800-200 line-clamp-1 font-bold print:no-underline!"
                data-testid="name"
                href={props.item.url}
                onclick={(e) => e.stopPropagation()}
                rel="noreferrer"
                target="_blank"
            >
                {props.item.name}
            </a>
        {:else}
            <span id={`${props.id}-name`} class="line-clamp-1 font-bold" data-testid="name">
                {props.item.name}
            </span>
        {/if}
        {#if props.item.price || props.item.itemPrice}
            <span class="flex-none text-sm" data-testid="price">{formatPrice(props.item)}</span>
        {/if}
    </div>

    <div class="flex flex-none items-center gap-2 print:hidden">
        {#if props.reorderActions}
            <ReorderButtons {...props} />
        {:else}
            <ClaimButtons isClaimableOrClaimed={() => {}} {...props} />
            <ManageButtons {...props} />
        {/if}
    </div>
</div>
