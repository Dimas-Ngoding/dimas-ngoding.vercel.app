<template>
	<div class="project mt-5">
		<div class="container mb-5">
			<h1 class="judul text-center" data-aos="fade-down" data-aos-duration="700">
				Project
			</h1>
			<h5 class="text-muted text-center" data-aos="fade-down" data-aos-duration="700">
				Project based on Github Api
			</h5>
			<Loading v-show="showLoading == true" />
			<container-project>
				<div class="row justify-content-center mt-4">
					<div
						class="col-12 col-md-12 col-lg-6 mt-4"
						v-for="({ name, desc, url, lang, crt, upt, topics }, i) of arrRepos"
						:key="i"
						:data-aos="i % 2 == 0 ? 'fade-right' : 'fade-left'"
						data-aos-duration="1000"
					>
						<div class="project-box">
							<h4>
								{{ name }} <span class="lang-gt">({{ lang }})</span>
							</h4>
							<h6 class="text-muted">{{ desc }}</h6>
							<hr />
							<h6 class="text-muted">
								<span class="badge badge-dark">Tags:</span>
								{{ arrayTopicsParse(topics) }}
							</h6>
							<h6 class="text-muted">
								<span class="badge badge-dark">Created:</span>
								{{ dateParse(crt) }}
							</h6>
							<h6 class="text-muted mb-2">
								<span class="badge badge-dark">Last Update:</span>
								{{ dateParse(upt) }}
							</h6>
							<a :href="url" class="btn btn-visit btn-block shadow-none mt-3" target="_blank"
								>Visit</a
							>
						</div>
					</div>
				</div>
			</container-project>
		</div>
		<Footer />
	</div>
</template>

<script>
import CtProject from "../components/Home/CtProject.vue";
import NProgress from "nprogress";
import ApiService from "../utils/ApiService";

export default {
	data: () => ({
		arrRepos: [],
		showLoading: true
	}),
	beforeCreate() {
		document.title = "Project | @dimasandhk";
	},
	created: async function() {
		NProgress.start();

		const data = await ApiService.getRepos();
		this.arrRepos = [...data];

		this.showLoading = false;
		NProgress.done();
	},
	methods: {
		dateParse(str) {
			return `${str.split("T")[0]}, ${str.split("T")[1].split("Z")[0]}`;
		},
		arrayTopicsParse(arr) {
			let res = "";
			if (arr.length > 0) {
				arr.forEach((el, i) => {
					if (arr.length - 1 == i) {
						res += `${this.capitalizeString(el)}`;
					} else {
						res += `${this.capitalizeString(el)}, `;
					}
				});
			} else {
				res = "No Tags";
			}
			return res;
		},
		capitalizeString(str) {
			return `${str.charAt(0).toUpperCase()}${str.slice(1)}`;
		}
	},
	components: {
		"container-project": CtProject
	}
};
</script>

<style lang="scss" scoped>
.project {
	hr {
		background-color: #616166 !important;
	}
	.head-span {
		color: #acacac;
	}
	.lang-gt {
		color: #1affd6;
		font-weight: 600;
	}
	.project-box {
		.badge-dark {
			background-color: #1affd6 !important;
			color: #27272e;
		}
		.btn-visit {
			&:hover {
				border: 1px solid #393940;
			}
			&:active {
				background-color: #393940;
			}
			color: #fff;
			border: 1px solid #393940;
			background-color: #393940;
			font-weight: 700;
		}
		box-shadow: rgba(0, 0, 0, 0.5) 0px 4px 8px 0px;
		height: 100%;
		background-color: #27272e;
		padding: 10px 15px;
		border-radius: 7px;
		border: none;
	}
}
h3,
h1 {
	font-weight: 600;
}
.judul {
	color: #1affd6;
}
</style>
