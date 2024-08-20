<script lang="ts">
	let competenceName: RegExpMatchArray | null = $state([""]);

	// Select the node that will be observed for mutations
	const targetNode = document.body;

	// Options for the observer (which mutations to observe)
	const config = { attributes: false, childList: true, subtree: true };

  function handleAddCompetenceButtonMouseOver(e: MouseEvent) {
		const r = "Lägg till (.+) som en kompetens";
		if (e.target instanceof HTMLElement && e.target.ariaLabel) {
			competenceName = e.target.ariaLabel.match(r);
		}
	}

	// Callback function to execute when mutations are observed
	function callback(mutationList: MutationRecord[], observerInstance: MutationObserver) {
		const addCompetenceListButtons = document.getElementsByClassName("job-details-skill-match-status-list__add-skill") as HTMLCollectionOf<HTMLButtonElement>;
		if (addCompetenceListButtons.length) {
			for (let button of addCompetenceListButtons) {
				button.addEventListener("mouseover", handleAddCompetenceButtonMouseOver);
			}
		}

		const competenceNameInput = document.querySelector("input[placeholder='Kompetens (t.ex. Projektledning)']") as HTMLInputElement;
		if (competenceNameInput && competenceName) {
			competenceNameInput.value = competenceName[1];
		}

		// ? potentially slow with many DOM elements
		// for (const mutation of mutationList) {
		// 	if (mutation.type === "childList") {
		// 		const addCompetenceListButtons = document.querySelectorAll<HTMLButtonElement>(".job-details-skill-match-status-list__add-skill");
		// 		if (addCompetenceListButtons.length) {
		// 			addCompetenceListButtons.forEach((button) => {
		// 				button.addEventListener("mouseover", handleAddCompetenceButtonMouseOver);
		// 				// button.addEventListener("click", handleAddCompetenceButtonPress);
		// 			});
		// 		}

		// 		const competenceNameInput = document.querySelector("input[placeholder='Kompetens (t.ex. Projektledning)']") as HTMLInputElement;
		// 		if (competenceNameInput && competenceName) {
		// 			competenceNameInput.value = competenceName[1];
		// 		}

		// 		// console.log("A child node has been added or removed.");
		// 	} else if (mutation.type === "attributes") {
		// 		// console.log(`The ${mutation.attributeName} attribute was modified.`);
		// 	}
		// }
	}

	// Create an observer instance linked to the callback function
	const observer = new MutationObserver(callback);

	// Start observing the target node for configured mutations
	observer.observe(targetNode, config);

	// Later, you can stop observing
	// observer.disconnect();
</script>

<!-- <main class="flex flex-col items-center w-full">
	<div class="size-48 w-96 h-60 left-0 border-orange-400 border-solid rounded-xl border-2">
		<h6 class="text-xl p-2">Hello!</h6>
		<p class="p-2">If you can see this the userscript is working properly.</p>
		<Counter />
    </div>
</main> -->

<style>
</style>
