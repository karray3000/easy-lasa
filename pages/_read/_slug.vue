<style scoped>
.chapter:first-letter {
  color: #903;
  float: left;
  font-family: Georgia;
  font-size: 4rem;
  line-height: 2.5rem;
  padding-top: 0.75rem;
  padding-bottom: 0.25rem;
  padding-right: 0.5rem;
  padding-left: 0.25rem;
}

.paragraph {
  /* line spacing */
  line-height: 1.3rem;
  text-align: justify;
  text-justify: inter-word;
  padding-bottom: 0.7rem;
}

.sentence {
  cursor: pointer;
  transition: 300ms;
}
.sentence:hover {
  background: #eea849; /* fallback for old browsers */
}

.modal-card {
  background-color: while;
  /* width: 60%; */
}

.divider {
  margin-top: 0.5rem;
  height: 0.5rem;
  position: relative;
  width: 100%;
  background: radial-gradient(
    ellipse farthest-side at top center,
    rgba(0, 0, 0, 0.08),
    transparent
  );
}
.divider:before {
  content: '';
  display: block;
  position: absolute;
  top: 0px;
  right: 0;
  left: 0;
  height: 0.5rem;
  background: linear-gradient(
    to right,
    transparent,
    rgba(0, 0, 0, 0.02),
    rgba(0, 0, 0, 0.02),
    transparent
  );
}
</style>

<template>
  <section class="section">
    <h2 class="title is-3">
      {{ title }}
    </h2>
    <h3 class="subtitle is-6">
      Author: {{ author }} / Translator: {{ translator }}
    </h3>
    <div>
      <div
        class="chapter"
        v-for="(chapter, c_index) in data"
        v-bind:key="c_index"
      >
        <div
          class="paragraph"
          v-for="(paragraph, p_index) in chapter"
          v-bind:key="p_index"
        >
          <span
            v-for="(sentencePair, s_index) in paragraph"
            v-bind:key="s_index"
          >
            <span v-if="s_index > 0"> </span>
            <span
              class="sentence"
              @click="
                showModal = true
                firstSentence = sentencePair[0]
                secondSentence = sentencePair[1]
              "
              >{{ sentencePair[0] }}</span
            >
          </span>
        </div>
      </div>
    </div>
    <b-modal v-model="showModal" has-modal-card scroll="keep" @close="showModal = false">
      <div
        class="modal-card"
        v-bind:style="{
          backgroundColor: 'white',
          padding: '1rem',
          borderRadius: '1rem',
        }"
      >
        <p>
          {{ firstSentence }}
        </p>
        <div class="divider" />
        <p>
          {{ secondSentence }}
        </p>
      </div>
    </b-modal>
  </section>
</template>

<script lang="ts">
export default {
  data() {
    return {
      showModal: false,
      firstSentence: undefined,
      secondSentence: undefined
    }
  },
  async asyncData({ params }: { params: { [key: string]: string } }) {
    const slug = params.slug

    const MY_JSON = await import(`../../static/${slug}.json`)

    return {
      title: MY_JSON.title,
      author: MY_JSON.author,
      translator: MY_JSON.translator,
      data: MY_JSON.data,
    }
  },
}
</script>
