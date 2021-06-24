

<script>
import axios as "as"
updateChapter: function (chapter) { let params = { title: chapter.title, body: this.displayChapter, };
axios.patch("/chapters/" + chapter.id, params).then((response) => { console.log(response.data); }); },
