<template>
	<div>
		<h1 class="title--composant">
			<span
				:class="new_letter ? 'letters new_letter' : 'letters'"
				v-for="(letter, i) in letters"
				:key="i"
			>
				{{ letter }}
			</span>
		</h1>
	</div>
</template>

<script setup lang="ts">
	const { title } = defineProps<{ title: string }>();
	const letters = useState<string[]>(() => []);
	const new_letter = useState(() => false);

	watch(
		() => title,
		() => {
			CutTitle();
			new_letter.value = true;
		}
	);

	onMounted(() => {
		CutTitle();
	});

	function CutTitle() {
		const exist_letters = document.querySelectorAll(".title--composant span");
		for (const letter of exist_letters) {
			letter.remove();
		}

		const Title = title;

		for (const letter of Title) {
			letters.value.push(letter);
		}
	}
</script>

<style lang="scss">
	// .title--composant {
	// 	overflow: hidden;
	// 	span {
	// 		display: inline-block;

	// 		&.new_letter {
	// 			transform: translateY(-150%);
	// 		}
	// 	}
	// }
</style>
