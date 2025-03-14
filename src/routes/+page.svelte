<script>
    import members from "$lib/members.json";

    let votes = $state(
        members
            .map((member) => ({
                vote: "maybe",
                ...member,
            }))
            .sort((a, b) => a.state.localeCompare(b.state))
    );

    let totalVotes = $derived({
        yes: votes.filter((member) => member.vote === "yes").length,
        no: votes.filter((member) => member.vote === "no").length,
        maybe: votes.filter((member) => member.vote === "maybe").length,
    });
</script>

<main>
    <div class="vote-distribution-bar" aria-label="Vote distribution bar">
        {#if totalVotes.yes > 0}
            <div
                class="vote-bar yes"
                style="width: {(totalVotes.yes /
                    (totalVotes.yes + totalVotes.no + totalVotes.maybe)) *
                    100}%;"
                aria-label="{totalVotes.yes} Yes"
            >
                <span>{totalVotes.yes}</span>
            </div>
        {/if}
        {#if totalVotes.maybe > 0}
            <div
                class="vote-bar maybe"
                style="width: {(totalVotes.maybe /
                    (totalVotes.yes + totalVotes.no + totalVotes.maybe)) *
                    100}%;"
                aria-label="{totalVotes.maybe} Maybe"
            >
                <span>{totalVotes.maybe}</span>
            </div>
        {/if}
        {#if totalVotes.no > 0}
            <div
                class="vote-bar no"
                style="width: {(totalVotes.no /
                    (totalVotes.yes + totalVotes.no + totalVotes.maybe)) *
                    100}%;"
                aria-label="{totalVotes.no} No"
            >
                <span>{totalVotes.no}</span>
            </div>
        {/if}
    </div>

    <h1>Members</h1>
    <ul>
        {#each votes as member}
            <li>
                <span>
                    <span
                        style="display: inline-block; width: 1rem; height: 1rem; background-color: {member.party ===
                        'Democratic'
                            ? 'blue'
                            : member.party === 'Republican'
                              ? 'red'
                              : 'yellow'}; margin-right: 0.5rem;"
                    ></span>
                    {member.name} ({member.state})
                </span>
                <span>
                    <button
                        onclick={() => (member.vote = "yes")}
                        style="background-color: {member.vote === 'yes'
                            ? 'lightgreen'
                            : 'white'};"
                    >
                        Yes
                    </button>
                    <button
                        onclick={() => (member.vote = "no")}
                        style="background-color: {member.vote === 'no'
                            ? 'lightcoral'
                            : 'white'};"
                    >
                        No
                    </button>
                    <button
                        onclick={() => (member.vote = "maybe")}
                        style="background-color: {member.vote === 'maybe'
                            ? 'lightyellow'
                            : 'white'};"
                    >
                        Maybe
                    </button>
                </span>
            </li>
        {/each}
    </ul>
</main>

<style>
    ul {
        list-style: none;
        padding: 0;
        display: grid;
        grid-template-columns: 1fr auto;
        max-width: 70ch;

        li {
            grid-column: 1 / -1;
            display: grid;
            grid-template-columns: subgrid;
        }
    }
    button {
        border: 1px solid black;
        padding: 0.5rem;
    }

    .vote-distribution-bar {
        display: flex;
        height: 2rem;
        background-color: #f0f0f0;
        border: 1px solid #ccc;
        border-radius: 4px;
        overflow: hidden;
        margin-bottom: 1rem;
        position: sticky;
        top: 1rem;
    }

    .vote-bar {
        display: flex;
        align-items: center;
        justify-content: center;
        color: white;
        font-weight: bold;
        height: 100%;
    }

    .vote-bar.yes {
        background-color: lightgreen;
    }

    .vote-bar.no {
        background-color: lightcoral;
    }

    .vote-bar.maybe {
        background-color: lightyellow;
        color: black;
    }
</style>
