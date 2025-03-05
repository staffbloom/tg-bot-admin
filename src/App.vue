<template>
  <div class="varieties">
    <div class="content">
      <div class="table-container">
        <DataTable :title="title" :headers="varietyHeaders" :entries="filteredVarieties" />
      </div>
    </div>
  </div>
</template>

<script>
import DataTable from '@/components/layout/DataTable.vue';

export default {
  name: "Varieties",
  components: {
    DataTable
  },
  data() {
    return {
      varieties: [],
      varietyHeaders: [
        'ID Заказа',
        'Даты',
        'Ценовой диапазон',
        'Город',
        'Улица и дом',
        'Квартира',
        'Этаж',
        'Подъезд',
        'Телефон',
        'Дата создания',
        'Время создания'
      ],
      title: 'Список заказов',
      searchQuery: ''
    };
  },
  mounted() {
    this.fetchOrders();
  },
  methods: {
    async fetchOrders() {
      try {
        const token = localStorage.getItem('authToken');
        if (!token) {
          throw new Error('No auth token found. Please log in first.');
        }

        const response = await fetch('https://bloom-backend-production.up.railway.app/orders', {
          method: 'GET',
          headers: {
            'Authorization': `Bearer ${token}`
          }
        });

        if (!response.ok) {
          if (response.status === 401) {
            throw new Error('Unauthorized: Please check your credentials');
          } else {
            throw new Error(`HTTP error! status: ${response.status}`);
          }
        }

        const data = await response.json();

        if (Array.isArray(data.orders)) {
          this.varieties = data.orders.map(order => ({
            order_id: order.order_id,
            dates: order.dates.join(', '),
            price_range: order.price_range,
            city: order.city,
            street_building: order.street_building,
            apartment: order.apartment || '-',
            floor: order.floor || '-',
            entrance: order.entrance || '-',
            phone: order.phone,
            created_date: order.created_date,  
            created_time: order.created_time
          }));
        } else {
          console.error('Expected array data, but received:', data);
        }

      } catch (error) {
        console.error('Error fetching orders:', error);
      }
    }
  },
  computed: {
    filteredVarieties() {
      const query = this.searchQuery.toLowerCase();
      return this.varieties.filter(item =>
        Object.values(item)
          .join(' ')
          .toLowerCase()
          .includes(query)
      );
    }
  }
};
</script>

<style scoped>

#app {
  display: flex;
}

@font-face {
  font-family: 'SF Pro Display';
  font-weight: 400; 
  font-style: normal;
  src: url('@/assets/fonts/SF-Pro-Display-Light.otf') format('opentype');
}

@font-face {
  font-family: 'SF Pro Display';  
  font-weight: 600;
  font-style: normal;
  src: url('@/assets/fonts/SF-Pro-Display-Semibold.otf') format('opentype');
}

@font-face {
  font-family: 'SF Pro Display';  
  font-weight: 500;
  font-style: normal;
  src: url('@/assets/fonts/SF-Pro-Display-Medium.otf') format('opentype');
}

body {
  font-family: 'SF Pro Display', sans-serif;
  margin: 0;
  padding: 0;
}

.varieties {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  font-family: 'SF Pro Display';
}

.content {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
}

</style>
