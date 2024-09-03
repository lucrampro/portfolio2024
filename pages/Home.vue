<template>
	<div class="home">
		<p class="numbers--projects">
			<span>{{ project_number + 1 }}</span> / {{ projects_info.length }}
		</p>
		<div class="container--imgs">
			<div
				:class="`wrapper--img img-${index}`"
				v-for="(_item, index) in 3"
				:key="index"
			>
				<NuxtLink
					:to="{ path: `/projets/${projects_info[project_number].title}` }"
				>
					<img
						:src="getImagePath(`projets/${projects_info[project_number].poster}`)"
						alt=""
					/>
				</NuxtLink>
			</div>

			<div class="wrapper--comandes--desktop">
				<p @click="animeImg('precedent')">precedent</p>
				<p @click="animeImg('suivant')">suivant</p>
			</div>
		</div>
		<div class="wrapper--infos">
			<p class="technos">{{ projects_info[project_number].techno }}</p>
			<Toto :title="projects_info[project_number].title" />
		</div>
		<div class="wrapper--comandes">
			<p @click="animeImg('precedent')">precedent</p>
			<p @click="animeImg('suivant')">suivant</p>
		</div>
	</div>
</template>

<script setup lang="ts">
	import projets from "@/projets.json";
	import gsap from "gsap";

	const projects_info = useState(() => projets);
	const project_number = useState(() => 0);

	function changeImg(value: "suivant" | "precedent") {
		if (value === "suivant") {
			project_number.value == projects_info.value.length - 1
				? (project_number.value = 0)
				: project_number.value++;
		} else {
			project_number.value == 0
				? (project_number.value = projects_info.value.length - 1)
				: project_number.value--;
		}
	}

	function animeImg(value: "suivant" | "precedent") {
		const wrapper_img_left = document.querySelector(".wrapper--img.img-0");
		const wrapper_img_right = document.querySelector(".wrapper--img.img-1");
		const wrapper_img_center = document.querySelector(".wrapper--img.img-2");

		const duration = 1.4;

		gsap
			.timeline()
			.to([wrapper_img_left, wrapper_img_right], {
				duration: duration,
				x: "0%",
				rotate: "0deg",
				scale: 1,
				ease: "expo.inOut",
			})
			.to(
				[
					wrapper_img_center?.querySelector("a img"),
					wrapper_img_right?.querySelector("a img"),
					wrapper_img_left?.querySelector("a img"),
				],
				{
					duration: duration,
					stagger: 0.05,
					top: "200%",
					ease: "expo.inOut",
				},
				"start"
			)
			.to(
				".technos",
				{
					opacity: 0,
					duration: 0.5,
				},
				"start"
			)
			.add(() => animeTitleToTop(), "start")
			.to(
				".numbers--projects span",
				{
					duration: 0.5,
					opacity: 0,
					y: "10px",
				},
				"start"
			)
			.add(() => changeImg(value))
			.set(
				[
					wrapper_img_center?.querySelector("a img"),
					wrapper_img_right?.querySelector("a img"),
					wrapper_img_left?.querySelector("a img"),
				],
				{
					top: "-100%",
				}
			)
			.set(".numbers--projects span", {
				y: "-10px",
			})
			.to(
				[
					wrapper_img_left?.querySelector("a img"),
					wrapper_img_right?.querySelector("a img"),
					wrapper_img_center?.querySelector("a img"),
				],
				{
					duration: duration,
					stagger: 0.05,
					top: "50%",
					ease: "expo.inOut",
				},
				"second"
			)
			.to(
				".numbers--projects span",
				{
					duration: 0.5,
					opacity: 1,
					y: "0px",
				},
				"ssecond"
			)
			.add(() => animeTitleToBottom())
			.to(".technos", {
				duration: duration,
				opacity: 1,
				delay: 0.5,
			})
			.to(
				[wrapper_img_left, wrapper_img_right],
				{
					scale: 0.8,
				},
				"second+=1"
			)
			.to(
				wrapper_img_right,
				{
					x: "-20%",
				},
				"second+=1"
			)
			.to(
				wrapper_img_left,
				{
					x: "20%",
				},
				"second+=1"
			);
	}

	function animeTitleToTop() {
		const duration = 1;
		gsap.timeline().to(".title--composant span", {
			duration: duration,
			stagger: 0.02,
			skewX: "-40deg",
			skewY: "-40deg",
			y: "150%",
			ease: "expo.inOut",
		});
	}

	function animeTitleToBottom() {
		const duration = 0.8;
		gsap
			.timeline()
			.set(".title--composant span", {
				skewX: "60deg",
				skewY: "60deg",
			})
			.to(".title--composant span", {
				duration: duration,
				stagger: 0.02,
				y: "0%",
				skewX: "0deg",
				skewY: "0deg",
				ease: "expo.inOut",
			});
	}

	onMounted(() => {
		const wrapper_img_left = ".wrapper--img.img-0";
		const wrapper_img_right = ".wrapper--img.img-1";
		const wrapper_img_center = ".wrapper--img.img-2";

		setTimeout(() => {
			gsap
				.timeline()
				.set(".home .title--composant span", {
					skewX: "60deg",
					skewY: "60deg",
				})
				.to(
					[wrapper_img_left, wrapper_img_right, wrapper_img_center],
					{
						duration: 0.5,
						scale: 1,
						opacity: 1,
					},
					"start"
				)
				.to(
					wrapper_img_left,
					{
						duration: 0.25,
						scale: 0.8,
						x: "-20%",
					},
					"start+=0.25"
				)
				.to(
					wrapper_img_right,
					{
						duration: 0.25,
						scale: 0.8,
						x: "20%",
					},
					"start+=0.25"
				)
				.to(
					".home h1 span",
					{
						duration: 1,
						stagger: 0.001,
						skewX: 0,
						skewY: 0,
						y: 0,
						ease: "expo.out",
					},
					"start"
				)

				.to(
					".home .technos",
					{
						opacity: 1,
						y: "0px",
					},
					"start+=0.2"
				)
				.to(
					".wrapper--comandes p",
					{
						opacity: 1,
						y: "0px",
					},
					"commands"
				)
				.to(
					".wrapper--comandes--desktop p",
					{
						opacity: 1,
						y: "0px",
					},
					"commands"
				)
				.to(
					".numbers--projects",
					{
						opacity: 1,
					},
					"commands"
				)
				.to(".header", {
					opacity: 1,
				});
		}, 100);
	});
</script>

<style lang="scss">
	.home {
		height: 100vh;
		padding-bottom: 0px !important;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		position: relative;

		@media screen and (min-width: $laptop) {
			flex-direction: column-reverse;
		}

		.container--imgs {
			width: 100%;
			height: 300px;
			position: relative;
			display: flex;
			align-items: center;
			justify-content: center;

			@media screen and (min-width: $tablet) {
				height: 500px;
			}

			.wrapper--comandes--desktop {
				display: flex;
				justify-content: space-between;
				width: 100%;
				max-width: 1230px;
				display: none;

				@media screen and (min-width: $laptop) {
					display: flex;
				}

				p {
					font-family: Title;
					text-transform: uppercase;
					cursor: pointer;
					opacity: 0;
				}
			}

			.wrapper--img {
				position: absolute;
				width: 220px;
				height: 240px;
				left: 50%;
				top: 50%;
				transform: translate(-50%, -50%) scale(0.9);
				opacity: 0;
				overflow: hidden;

				@media screen and (min-width: $tablet) {
					width: 540px;
					height: 480px;
				}

				@media screen and (min-width: $laptop) {
					width: 630px;
					height: 360px;
				}

				&.img-0 {
					// transform: translate(-80%, -50%) scale(0.7);
					opacity: 0.6;
				}

				&.img-1 {
					// transform: translate(-20%, -50%) scale(0.7);
					opacity: 0.6;
				}

				img {
					width: 100%;
					position: absolute;
					top: 50%;
					left: 50%;
					transform: translate(-50%, -50%);
				}
			}
		}

		.wrapper--infos {
			h1 {
				text-transform: uppercase;
				text-align: center;

				span {
					transform: translateY(200%);
				}

				@media screen and (min-width: $laptop) {
					letter-spacing: 5px;
				}
			}

			p {
				text-align: center;
				transform: translateY(-40px);
				opacity: 0;

				@media screen and (min-width: $laptop) {
					font-size: 24px;
				}
			}
		}

		.wrapper--comandes {
			width: 100%;
			display: flex;
			align-items: center;
			justify-content: space-evenly;
			margin-top: 20px;
			padding: 0px 40px;

			@media screen and (min-width: $laptop) {
				display: none;
			}

			p {
				opacity: 0;
				font-family: Title;
				text-transform: uppercase;
				cursor: pointer;
			}
		}

		.numbers--projects {
			opacity: 0;

			> span {
				display: inline-block;
			}
		}
	}
</style>
