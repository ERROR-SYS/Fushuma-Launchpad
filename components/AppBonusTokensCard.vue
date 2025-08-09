<script setup lang="ts">
    import { formatNumber } from '~/js/utils';
    import AppSubtitle from '~/components/AppSubtitle.vue';
    import type { IIcoInfoWithKey } from '~/types/Ico';
    import AppFeatureEnabledText from '~/components/AppFeatureEnabledText.vue';
    import { getTokenSymbol } from '~/js/tokens';

    const { icoInfo } = defineProps<{
        icoInfo: IIcoInfoWithKey;
    }>();

    let icoTokenSymbol = await getTokenSymbol(icoInfo.data.icoMint);

    const tokensToPurchase = computed(() => {
        return formatNumber(
            (icoInfo.data.amount / icoInfo.data.icoDecimals) * (icoInfo.data?.bonusActivator / 100 / 100),
        );
    });
</script>

<template>
    <div v-if="icoInfo.data?.bonusReserve" class="bg-white dark:bg-white/10 shadow-sm py-6 px-4">
        <AppSubtitle title="Bonus Tokens" class="mb-3" />

        <AppFeatureEnabledText />

        <div class="grid grid-cols-2 gap-y-5 mt-5">
            <div>
                <p class="text-sm text-black/50 dark:text-white/50">Allocated bonus reserve</p>
                <p class="tracking-tight font-medium mt-1">
                    {{ formatNumber(icoInfo.data.bonusReserve / icoInfo.data.icoDecimals) }} {{icoTokenSymbol}}
                </p>
            </div>

            <div class="text-end">
                <p class="text-sm text-black/50 dark:text-white/50">Additional bonus {{ icoTokenSymbol }}</p>
                <p class="tracking-tight mt-1 font-medium">{{ icoInfo.data?.bonusPercentage / 100 }}%</p>
            </div>

            <div class="col-span-2">
                <p class="text-sm text-black/50 dark:text-white/50">Bonus activator</p>
                <p class="tracking-tight mt-1 font-medium">
                    {{ formatNumber(icoInfo.data?.bonusActivator / 100) }}% of {{ icoTokenSymbol }} ({{ tokensToPurchase }}
                    {{ icoTokenSymbol }})
                </p>
            </div>
        </div>
    </div>
</template>

<style scoped></style>
