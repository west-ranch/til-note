<template>
  <div class="editor">
    <div class="memoListWrapper">
      <div class="memoBtns">
        <button class="addMemoBtn" @click="addMemo">メモの追加</button>
        <button class="saveMemosBtn" @click="saveMemos">メモの保存</button>
        <button class="deleteMemoBtn" v-if="memos.length > 1" @click="deleteMemo">選択中のメモの削除</button>
      </div>
      <div
        class="memoList"
        v-for="(memo, index) in memos"
        :key="index"
        @click="selectMemo(index)"
        :data-selected="index == selectedIndex"
      >
        <p class="memoTitle">{{ displayTitle(memo.markdown) }}</p>
      </div>
    </div>
    <textarea class="markdown" v-model="memos[selectedIndex].markdown" ref="markdown"></textarea>
    <div class="previewWrapper" ref="preview">
      <p class="previewTitle">Preview Area</p>
      <div class="preview markdown-body" v-html="preview()"></div>
    </div>
  </div>
</template>

<script>
import marked from "marked";
export default {
  name: "editor",
  props: ["user"],
  data() {
    return {
      memos: [
        {
          markdown: ""
        }
      ],
      selectedIndex: 0
    };
  },
  created: function() {
    firebase
      .firestore()
      .collection("memos")
      .doc(this.user.uid)
      .get()
      .then(doc => {
        if (doc.exists && doc.data().memos) {
          this.memos = doc.data().memos;
        }
      });
  },
  mounted: function() {
    document.onkeydown = e => {
      if (e.key == "s" && (e.metaKey || e.ctrlKey)) {
        this.saveMemos();
        return false;
      }
    };
  },
  beforeDestroy: function() {
    document.onkeydown = null;
  },
  methods: {
    logout: function() {
      firebase.auth().signOut();
    },
    addMemo: function() {
      this.memos.push({
        markdown: "無題のメモ"
      });
    },
    saveMemos: function() {
      firebase
        .firestore()
        .collection("memos")
        .doc(this.user.uid)
        .set({ memos: this.memos });
    },
    deleteMemo: function() {
      if (confirm("本当に削除しますか？")) {
        this.memos.splice(this.selectedIndex, 1);
        if (this.selectedIndex > 0) {
          this.selectedIndex--;
        }
      }
    },
    selectMemo: function(index) {
      this.selectedIndex = index;
    },
    preview: function() {
      return marked(this.memos[this.selectedIndex].markdown);
    },
    displayTitle: function(text) {
      return text.split(/\n/)[0];
    }
  }
};
</script>

<style lang="scss" scoped>
.editor {
  height: 100%;
}
.memoListWrapper {
  width: 20%;
  padding-bottom: 20px;
}
.memoList {
  padding: 10px;
  box-sizing: border-box;
  text-align: left;
  border-bottom: 1px solid #000;
  &:nth-child(even) {
    background-color: #ccc;
  }
  &[data-selected="true"] {
    background-color: #ccf;
  }
}
.memoTitle {
  height: 1.5em;
  margin: 0;
  white-space: nowrap;
  overflow: hidden;
}
.addMemoBtn {
  margin-top: 20px;
}
.deleteMemoBtn {
  margin: 10px;
}
.markdown {
  border: none;
  border-right: #ccc 1px solid;
  border-left: #ccc 1px solid;
  background-color: #eee;
  box-shadow: inset 0 0 5px 0 rgba(0, 0, 0, 0.1);
  padding: 10px;
  width: 50%;
  resize: none;
}
.previewWrapper {
  text-align: left;
  width: 30%;
}
.previewTitle {
  color: #888;
  padding: 10px;
  font-size: 14px;
  border-bottom: #ddd 1px dotted;
}
.preview {
  padding: 10px;
}
.markdown,
.memoListWrapper,
.previewWrapper {
  overflow: scroll;
  float: left;
  height: 100%;
  box-sizing: border-box;
}
</style>