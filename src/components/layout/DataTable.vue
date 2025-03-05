<template>
    <div class="data-table">
      <div class="header">
        <div class="title-search">
          <h2 class="table-title">{{ title }}</h2>
          <div class="search-bar">
            <input type="text" placeholder="Поиск" v-model="searchTerm" />
          </div>
        </div>
      </div>
      <div class="table-wrapper">
        <div class="scrollable-table">
          <table>
            <thead>
              <tr>
                <th v-for="header in headers" :key="header">{{ header }}</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="entry in filteredEntries" :key="entry.id">
                <td v-for="(value, key) in entry" :key="key">
                  <span v-if="isImageKey(key, value)">
                    <a :href="value" target="_blank" rel="noopener noreferrer" style="color: blue; text-decoration: underline;">
                      Открыть фото
                    </a>
                  </span>
                  <span v-else>
                    {{ value }}
                  </span>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "DataTable",
    props: {
      title: {
        type: String,
        default: "ВСЕ ДАННЫЕ"
      },
      headers: {
        type: Array,
        required: true
      },
      entries: {
        type: Array,
        required: true
      }
    },
    data() {
      return {
        activeTab: null,
        searchTerm: ""
      };
    },
    computed: {
      filteredEntries() {
        return this.entries.filter(entry =>
          Object.values(entry).some(value =>
            value.toString().toLowerCase().includes(this.searchTerm.toLowerCase())
          )
        );
      }
    },
    methods: {
      isImageKey(key, value) {
        return typeof value === 'string' && value.includes('http') && (key.includes('photo') || key.includes('image'));
      }
    }
  };
  </script>
  
  <style scoped>
  .data-table {
    width: 95%;
    max-width: 1400px;
    border: 1px solid #000;
    padding: 20px;
    margin: 20px auto;
    overflow-x: auto;
  }
  
  .header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 10px;
    font-family: 'SF Pro Display', sans-serif;
    font-weight: 500;
  }
  
  .table-title {
    text-transform: uppercase;
  }
  
  .title-search {
    display: flex;
    align-items: center;
    gap: 20px;
  }
  
  .header h2 {
    font-size: 18px;
    font-family: 'SF Pro Display', sans-serif;
    font-weight: 500;
  }
  
  .search-bar input {
    height: 28px;
    border-radius: 0;
    padding: 0 10px;
    border: 1px solid #000;
    font-size: 14px;
  }
  
  .button-group {
    display: flex;
    gap: 10px;
  }
  
  .button-group button {
    padding: 5px 15px;
    border: 1px solid #000;
    background: none;
    cursor: pointer;
    font-size: 14px;
  }
  
  .button-group button.active {
    background-color: #000;
    color: #fff;
  }
  
  .table-wrapper {
    overflow-x: auto;
    position: relative;
  }
  
  table {
    width: 100%;
    border-collapse: collapse;
  }
  
  .scrollable-table {
    max-height: 60vh; 
    overflow-y: auto;  
  }
  
  .scrollable-table table {
    width: 100%;
    border-collapse: collapse;
  }
  
  th, td {
    min-width: 150px;
    padding: 20px;
    text-align: left;
    border-bottom: 1px solid #ccc; 
    white-space: nowrap;
  }
  
  th {
    position: sticky;
    top: 0;
    background-color: #fff;
    z-index: 1;
    color: #CCCCCC;
    font-family: 'SF Pro Display', sans-serif;
    font-weight: 400;
    text-transform: uppercase;
  }
  
  @media screen and (min-width: 1600px) {
    .data-table {
      max-width: 90%;
    }
  }
  
  @media screen and (min-width: 1920px) {
    .data-table {
      max-width: 80%;
    }
  }

  @media screen and (max-width: 768px) {
  .data-table {
    width: 100%;
    max-width: 100%;
    padding: 10px;
    margin: 10px 0;
  }

  .title-search {
    flex-direction: column;
    gap: 10px;
    align-items: flex-start;
  }

  .search-bar input {
    width: 100%;
    font-size: 12px;
    padding: 5px;
  }

  .scrollable-table {
    overflow-x: auto;
    max-width: 100%;
  }

  table {
    font-size: 12px;
    min-width: 600px;
  }

  th, td {
    padding: 10px;
    min-width: 100px;
  }
}

  </style>
