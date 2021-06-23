<template>
  <div class="tag-input">
    <input
      type="text"
      v-model="newTag"
      @keydown.enter="addTag(newTag)"
      @keydown.prevent.tab="addTag(newTag)"
      @keydown.delete="newTag.length || removeTag(tags.length - 1)"
      :style="{ 'padding-left': `${paddingLeft}px` }"
    />
    <ul class="tags" ref="tagsUl">
      <li v-for="(tag, index) in tags" :key="tag" class="tag">
        {{ tag }}
        <button class="delete" @click="removeTag(index)">x</button>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, watch, nextTick, onMounted } from "vue";
export default {
  props: {
    modelValue: { type: Array, default: () => [] },
  },
  setup(props, { emit }) {
    const tags = ref(props.modelValue);
    const newTag = ref("");
    const paddingLeft = ref(10);
    const tagsUl = ref(null);
    const addTag = (tag) => {
      //check empty
      if (tag === "") {
        console.error("tag is empty.");
        return;
      }
      // duplicate
      if (tags.value.some((it) => it === tag)) {
        console.error("tag is duplicate.");
        return;
      }
      tags.value.push(tag);
      newTag.value = "";
    };
    const removeTag = (index) => {
      tags.value.splice(index, 1);
    };
    const onTagsChange = () => {
      // set left padding
      const extraCushion = 15;
      paddingLeft.value = tagsUl.value.clientWidth + extraCushion;
      // scroll tags ul to end
      tagsUl.value.scrollTo(tagsUl.value.scrollWidth, 0);
      emit("update:modelValue", tags.value);
    };
    watch(tags, () => nextTick(onTagsChange), { deep: true });
    onMounted(onTagsChange);

    return {
      tags,
      newTag,
      addTag,
      removeTag,
      paddingLeft,
      tagsUl,
    };
  },
};
</script>

<style scoped>
.tag-input {
  position: relative;
}
ul {
  list-style: none;
  display: flex;
  align-items: center;
  gap: 7px;
  margin: 0;
  padding: 0;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 10px;
  max-width: 75%;
  overflow-x: auto;
  /* overflow: hidden; */
}

.tag {
  background: rgb(250, 104, 104);
  padding: 5px;
  border-radius: 4px;
  color: white;
  white-space: nowrap;
  transition: 0.1s ease background;
}
input {
  width: 100%;
  padding: 10px;
}
.delete {
  color: white;
  background: none;
  outline: none;
  border: none;
  cursor: pointer;
}
</style>