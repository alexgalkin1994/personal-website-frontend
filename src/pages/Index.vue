<template>
  <Layout>
    <div class="container">
      <!-- <h1 class="text-4xl font-semibold mt-16">{{ $page.strapi.home.title }}</h1> -->
      <div class="title">
        <span class="text write" data-splitting="lines">
          RETRO<br />
          LASER<br />
          WRITE
        </span>
        <span aria-hidden="true" class="text laser" data-splitting="lines">
          RETRO<br />
          LASER<br />
          WRITE
        </span>
      </div>
      <div class="mt-2 w-full md:w-9/12">
        <RichText :data="{ content: $page.strapi.home.bio }" />
      </div>
      <h2
        class="text-2xl uppercase text-gray-600 mb-6 mt-16 tracking-wide font-semibold"
      >
        My latest projects
      </h2>
    </div>
    <!-- List of project preview cards -->
    <div class="flex flex-col gap-12">
      <ProjectCard
        v-for="project in $page.strapi.projects"
        :key="project.id"
        :project="project"
      />
    </div>
  </Layout>
</template>

<page-query>
query {
  strapi {
    # Get homepage data
    home {
      title
      bio
      # Metadata for SEO
      seo {
        title
        description
        shareImage {
          id
          url
        }
      }
    }
    # List projects
    projects(sort: "date:desc") {
      title
      slug
      description
      categories {
        id
        title
      }
      coverImage {
        id
        url
      }
    }
  }
}
</page-query>

<script>
import ProjectCard from "~/components/ProjectCard";
import RichText from "~/components/RichText";
import { getStrapiMedia } from "~/utils/medias";
import { getMetaTags } from "~/utils/seo";

export default {
  methods: {
    getStrapiMedia,
  },
  components: {
    ProjectCard,
    RichText,
  },
  metaInfo() {
    const { title, description, shareImage } = this.$page.strapi.home.seo;
    const image = getStrapiMedia(shareImage.url);
    return {
      title,
      meta: getMetaTags(title, description, image),
    };
  },
};
</script>

<style>
img {
  width: 100%;
  height: auto;
}

.title {
  position: relative;
  text-align: center;
  transform: translateZ(0);
  transform-style: preserve-3d;
}

.title .text {
  font-family: sans-serif;
  font-weight: 400;
  font-size: calc(20vw / var(--word-total) );
  line-height: 1.0;
}

.title .write .word {
  color: hsl(0, 0%, 80%);
  text-shadow: 0 0 0.1em currentColor;
  transform-style: preserve-3d;
  animation: write linear both;
}

.title .laser {
  position: absolute;
  top: 0;
  left: 0;
  /* To avoid the blur getting masked by the clip-path we had to duplicate the element */
  filter: blur(4px) contrast(10);
  pointer-events: none;
}

.title .laser .word {
  display: inline-block;
}

.title .laser .word {
  color: hsl(0, 100%, 75%);
  text-shadow: 0 0 0.1em currentColor;
  transform: translateZ(5px);
  animation: laser linear both;
}

.title .write .word,
.title .laser .word {
  animation-duration: 4s;
  animation-delay: calc(0.3s + var(--word-index) * 160ms);
  animation-iteration-count: infinite;
}

.title:hover .word {
  animation-play-state: paused;
}

/* .title .text .word,
.title .text .word::before {
  animation-play-state: paused;
  animation-delay: -0.4s;
} */

@keyframes write {
  from, 30% { clip-path: polygon(-20% 0%, -15% 0%, -15% 100%, -20% 100%) }
  70%, to { clip-path: polygon(-15% 0%, 120% 0%, 120% 100%, -15% 100%) }
}

@keyframes laser {
  from, 30% { clip-path: polygon(-20% 0%, -15% 0%, -15% 100%, -20% 100%) }
  70%, to { clip-path: polygon(115% 0%, 120% 0%, 120% 100%, 115% 100%) }
}


</style>
