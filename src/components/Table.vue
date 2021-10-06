<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div>
      <div class="formContainer">
        <span class="search">Search:</span>
        <input
          type="search"
          class="textField"
          placeholder="search for book names"
          v-model="search"
        />
      </div>
      <table>
        <tr>
        
          <th>Name</th>
          <th>ISBN</th>
          <th>Authors</th>
          <th>Country</th>
          <th>PageNumber</th>
          <th>Date</th>
        </tr>
        <tr v-for="item in filterNames" :key="item.url">
          <td>{{ item.name }}</td>
          <td>{{ item.isbn }}</td>
          <td>{{ formatAuthor(item.authors) }}</td>
          <td>{{ item.country }}</td>
          <td>{{ item.numberOfPages }}</td>
          <td>{{ formatDate(item.released) }}</td>
        </tr>
      </table>

      <div class="footer">
        <p class="entries">Showing 1 to 10 of 57 entires</p>
        <div class="list">
          <button class="pagination-button" >
            Previous
          </button>
          <!-- <span class="list" v-for="(item, index) in new Array(2)" :key="index">
            <button class="pagination-button" @click="pageNumber = index + 1">
              {{ index + 1 }}
            </button>
          </span> -->
          <button class="pagination-button">
            Next
          </button>
        </div>
      </div>
    </div>
    <slot name="data" :pageNumber="pageNumber" />
  </div>
</template>

<script>
export default {
  name: 'Table',
  props: {
    msg: String,
    data: Array,
  },
  data() {
    return {
      search: '',
      currentPage: 1,
      allPages: 2,
    };
  },
  mounted() {
    this.name();
    console.log(this.search);
  },
  computed: {
    filterNames() {
      return this.data.filter((data) => {
        return data.name.toUpperCase().match(this.search.toUpperCase());
      });
    },
  },
  methods: {
    formatDate(date) {
      var d = new Date(date),
        month = '' + (d.getMonth() + 1),
        day = '' + d.getDate(),
        year = d.getFullYear();

      if (month.length < 2) month = '0' + month;
      if (day.length < 2) day = '0' + day;

      return [year, month, day].join('-');
    },
    formatAuthor(value) {
      return value.toString();
    },
    pagination() {
     fetch(
      'https://www.anapioficeandfire.com/api/books/?page={pageNumber}&pageSize=12'
    )
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
.formContainer {
  display: flex;
  justify-content: flex-end;
  align-content: baseline;
  margin-bottom: 10px;
}
.search {
  font-size: 15px;
  margin-right: 10px;
  margin-top: 5px;
}
.textField {
  height: 30px;
  padding: 10px;
  border-radius: 5px;
}
.footer {
  display: flex;
  justify-content: space-between;
  align-content: center;
}
.list {
  display: flex;
  margin-top: 5px;
  justify-content: flex-end;
}
.entries {
  display: flex;
  justify-content: flex-start;
}
.pagination-button {
  padding: 8px;
  margin: 2px;
  border-radius: 3px;
  font-size: 1em;
  cursor: pointer;
}
</style>
