<template>
    <div :class="['sidebar']">
      <div class="logo">
        <img src="@/assets/img/logo.png" alt="Blooooom Logo" />
      </div>
    </div>
  </template>
  
  <script>
  import { Icon } from '@iconify/vue';
  
  export default {
    name: 'Sidebar',
    components: {
      Icon,
    },
    data() {
      return {
        isCollapsed: false,
        ordersExpanded: false,
        bouquetExpanded: false,
        flowersExpanded: false,
        storeExpanded: false,
        additionalItemsExpanded: false,
        companyExpanded: false,
      };
    },
    methods: {
      toggleSidebar() {
        this.isCollapsed = !this.isCollapsed;
        document.documentElement.style.setProperty(
          '--sidebar-width',
          this.isCollapsed ? '80px' : '250px'
        );
      },
      toggleOrders() {
        this.ordersExpanded = !this.ordersExpanded;
      },
      toggleBouquet() {
        this.bouquetExpanded = !this.bouquetExpanded;
      },
      toggleFlowers() {
        this.flowersExpanded = !this.flowersExpanded;
      },
      toggleStore() {
        this.storeExpanded = !this.storeExpanded;
      },
      toggleAdditionalItems() {
        this.additionalItemsExpanded = !this.additionalItemsExpanded;
      },
      toggleCompany() {
        this.companyExpanded = !this.companyExpanded;
      },
      closeSubmenus() {
        this.ordersExpanded = false;
        this.bouquetExpanded = false;
        this.flowersExpanded = false;
        this.storeExpanded = false;
        this.additionalItemsExpanded = false;
        this.companyExpanded = false;
      },
      logout() {
        localStorage.removeItem('auth_token');
        localStorage.removeItem('username');
        this.$store.commit('clearUserData');
        this.$router.push('/login');
      },
    },
    watch: {
      $route(to) {
        if (to.path.startsWith('/orders')) {
          this.ordersExpanded = true;
        } else if (to.path.startsWith('/bouquet')) {
          this.bouquetExpanded = true;
        } else if (to.path.startsWith('/flowers')) {
          this.flowersExpanded = true;
        } else if (to.path.startsWith('/store')) {
          this.storeExpanded = true;
        } else if (to.path.startsWith('/additional-items')) {
          this.additionalItemsExpanded = true;
        } else if (to.path.startsWith('/company')) {
          this.companyExpanded = true;
        }
      },
    },
  };
  </script>
  
  
  
  
  <style scoped>

  .menu-item {
    font-family: 'SF Pro Display';
  }

  span {
    font-family: 'SF Pro Display';
  }

  .sidebar {
    position: fixed;
    width: 250px;
    background-color: #fff;
    border-right: 1px solid #000;
    height: 100vh;
    display: flex;
    flex-direction: column;
    padding-top: 20px;
    transition: width 0.3s;
    overflow-y: auto;
  }
  
  .sidebar.collapsed {
    width: 80px;
  }
  
  .logo {
    text-align: center;
    margin-bottom: 30px;
    cursor: pointer;
  }
  
  .logo img {
    max-width: 150px;
    transition: max-width 0.3s;
  }
  
  .sidebar.collapsed .logo img {
    max-width: 50px;
  }
  
  .menu-list {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  
  .menu-list > li {
    margin: 10px 0;
  }
  
  .menu-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px 20px;
    cursor: pointer;
    margin-right: 10px;
    margin-left: 10px;
  }
  
  .menu-list a {
    display: flex;
    align-items: center;
    padding: 10px 20px;
    text-decoration: none;
    color: #000;
    transition: background-color 0.3s, color 0.3s;
    margin-right: 10px;
    margin-left: 10px;
  }
  
  .menu-list a:hover,
  .menu-list .router-link-active {
    background-color: #000;
    color: #fff;
  }
  
  .icon {
    margin-right: 15px;
    transition: margin 0.3s;
  }
  
  .sidebar.collapsed .icon {
    margin-right: 0;
  }
  
  .toggle-icon {
    margin-left: auto;
    cursor: pointer;
    transition: transform 0.3s;
  }
  
  .menu-list span {
    font-size: 16px;
  }
  
  .sidebar.collapsed .menu-list span {
    display: none;
  }
  
  .submenu {
    list-style: none;
    padding-left: 40px;
    margin: 0;
  }
  
  .submenu li {
    margin: 5px 0;
  }
  
  .sidebar::-webkit-scrollbar {
    width: 6px;
  }
  
  .sidebar::-webkit-scrollbar-thumb {
    background-color: rgba(0, 0, 0, 0.3);
    border-radius: 3px;
  }
  
  .sidebar::-webkit-scrollbar-thumb:hover {
    background-color: rgba(0, 0, 0, 0.5);
  }
  
  @media (max-width: 768px) {
    .sidebar {
      width: 80px;
      overflow-y: auto;
    }
  
    .sidebar.collapsed {
      width: 80px;
    }
  
    .sidebar.collapsed .menu-list span {
      display: none;
    }
  
    .sidebar.collapsed .logo img {
      max-width: 50px;
    }
  
    .icon {
      margin-right: 0;
    }
  }
  </style>
  