<script lang="ts">
    import { browser } from '$app/environment';
    import { createAccordion, melt } from '@melt-ui/svelte';
    import { slide } from 'svelte/transition';

    export let items: Array<{
        question: string;
        answer: string;
    }> = [
        {
            question: 'What is the Appwrite Education Program?',
            answer: "If you're a student with the GitHub Student Developer Pack, you can use Appwrite Pro for free while in school to help you build your next project."
        },
        {
            question: 'Who is eligible to apply?',
            answer: 'Any student enrolled in the GitHub Student Developer Pack can apply for free and receive Appwrite Pro until graduation.'
        },
        {
            question: 'How do I apply?',
            answer: "If you're already enrolled in the GitHub Student Developer Pack, click the 'Sign up' button on this page and fill in your details. If you're not enrolled with GitHub Education yet, first apply for the GitHub Student Developer Pack, then come back and sign up to Appwrite Cloud here."
        },
        {
            question: 'What happens after I sign up?',
            answer: 'Appwrite Cloud will automatically verify your GitHub Student Developer Pack membership and apply Appwrite Pro to your account. You can then start using Appwrite right away.'
        },
        {
            question: "I'm already an Appwrite user. Can I still apply?",
            answer: 'This program is only open to new users.'
        },
        {
            question: 'How long do the Appwrite Education program benefits last?',
            answer: 'Your access to Appwrite Pro is valid until you finish your studies and graduate from the GitHub Student Developer Pack.'
        },
        {
            question: 'Do Cloud credits include any add-ons?',
            answer: 'No, Cloud credits do not cover any add-ons.'
        }
    ];

    const {
        elements: { root, heading, content, item, trigger },
        helpers: { isSelected }
    } = createAccordion({
        defaultValue: '0',
        multiple: false,
        forceVisible: true
    });
</script>

<div class="container grid justify-between pt-20 md:grid-cols-2">
    <h2 class="text-primary mt-10 text-5xl">FAQ</h2>
    <ul class="collapsible w-full divide-y divide-white/5" use:melt={$root} id="faq">
        {#each items as faqItem, index (index)}
            <li class="collapsible-item">
                <!-- Progressive Enhancement for kbd navigation & animations -->
                {#if browser}
                    <div
                        class="collapsible-wrapper py-2"
                        use:melt={$item(`${index}`)}
                        {...{ open: $isSelected(`${index}`) ? true : undefined }}
                    >
                        <h3 use:melt={$heading({ level: 3 })}>
                            <button
                                class="flex w-full items-center justify-between gap-2.5 py-6 text-left"
                                use:melt={$trigger(`${index}`)}
                            >
                                <span class="web-label web-u-color-text-primary">
                                    {faqItem.question}
                                </span>
                                <div
                                    class="icon web-u-color-text-primary self-start transition-transform"
                                    class:rotate-180={$isSelected(`${index}`)}
                                >
                                    <span class="icon-cheveron-down" aria-hidden="true" />
                                </div>
                            </button>
                        </h3>

                        {#if $isSelected(`${index}`)}
                            <div
                                class="collapsible-content"
                                use:melt={$content(`${index}`)}
                                transition:slide
                            >
                                <p class="web-main-body-400">
                                    <!-- eslint-disable-next-line svelte/no-at-html-tags -->
                                    {@html faqItem.answer}
                                </p>
                            </div>
                        {/if}
                    </div>
                {:else}
                    <details class="collapsible-wrapper" open={index === 0}>
                        <summary class="collapsible-button">
                            <span class="web-label web-u-color-text-primary">
                                {faqItem.question}
                            </span>
                            <div class="icon web-u-color-text-primary">
                                <span class="icon-cheveron-down" aria-hidden="true" />
                            </div>
                        </summary>

                        <div class="collapsible-content">
                            <p class="web-main-body-400">
                                <!-- eslint-disable-next-line svelte/no-at-html-tags -->
                                {@html faqItem.answer}
                            </p>
                        </div>
                    </details>
                {/if}
            </li>
        {/each}
    </ul>
</div>
