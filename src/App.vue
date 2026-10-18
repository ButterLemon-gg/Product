<template>
  <div class="container">
    <h1>แบบฝึกหัดที่ 17: โปรแกรมแสดงข้อมูลสินค้าและผู้ใช้ (Vue.js)</h1>

    <div v-if="loading" class="loading">
      ⏳ กำลังโหลดข้อมูลจาก API...
    </div>

    <div v-else>
      <!-- ส่วนที่ 1: แสดงสินค้าเป็นคอลัมน์ (Cards) -->
      <section class="section">
        <h2>1. ข้อมูลสินค้า - แสดงผลแบบคอลัมน์ (Grid/Cards)</h2>
        <div class="product-grid">
          <div v-for="product in products" :key="product.id" class="card">
            <img :src="product.image" :alt="product.title" class="card-img" />
            <div class="card-category">{{ product.category }}</div>
            <div class="card-title">{{ product.title }}</div>
            <div class="card-price">${{ product.price }}</div>
          </div>
        </div>
      </section>

      <!-- ส่วนที่ 2: แสดงสินค้าแบบตาราง (Table) -->
      <section class="section">
        <h2>2. ข้อมูลสินค้า - แสดงผลแบบตาราง (Table)</h2>
        <div class="table-container">
          <table class="styled-table">
            <thead>
              <tr>
                <th style="width: 60px;">ID</th>
                <th style="width: 80px;">รูปภาพ</th>
                <th>ชื่อสินค้า</th>
                <th>หมวดหมู่</th>
                <th style="width: 100px;">ราคา</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="product in products" :key="'table-' + product.id">
                <td>{{ product.id }}</td>
                <td><img :src="product.image" class="table-img" /></td>
                <td>{{ product.title }}</td>
                <td>{{ product.category }}</td>
                <td><strong>${{ product.price }}</strong></td>
              </tr>
            </tbody>
          </table>
        </div>
      </section>

      <!-- ส่วนที่ 3: แสดงข้อมูลผู้ใช้แบบตาราง (Table) -->
      <section class="section">
        <h2>3. ข้อมูลผู้ใช้ - แสดงผลแบบตาราง (Table)</h2>
        <div class="table-container">
          <table class="styled-table">
            <thead>
              <tr>
                <th style="width: 60px;">ID</th>
                <th>ชื่อ - นามสกุล</th>
                <th>Username</th>
                <th>อีเมล</th>
                <th>เบอร์โทรศัพท์</th>
                <th>เว็บไซต์</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="user in users" :key="user.id">
                <td>{{ user.id }}</td>
                <td><strong>{{ user.name }}</strong></td>
                <td>{{ user.username }}</td>
                <td>{{ user.email }}</td>
                <td>{{ user.phone }}</td>
                <td>{{ user.website }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </section>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const products = ref([])
const users = ref([])
const loading = ref(true)

// ฟังก์ชันดึงข้อมูลจาก API
const fetchData = async () => {
  try {
    // 1. ดึงข้อมูลสินค้าจาก FakeStore API
    const resProducts = await fetch('https://fakestoreapi.com/products?limit=8')
    products.value = await resProducts.json()

    // 2. ดึงข้อมูลผู้ใช้จาก JSONPlaceholder API
    const resUsers = await fetch('https://jsonplaceholder.typicode.com/users')
    users.value = await resUsers.json()
  } catch (error) {
    console.error('เกิดข้อผิดพลาดในการดึงข้อมูล:', error)
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchData()
})
</script>

<style scoped>
.container {
  max-width: 1200px;
  margin: 0 auto;
  background: #ffffff;
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 4px 15px rgba(0,0,0,0.08);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 30px;
  border-bottom: 3px solid #42b883;
  padding-bottom: 10px;
}

.section {
  margin-bottom: 40px;
}

h2 {
  color: #35495e;
  background-color: #e8f5e9;
  padding: 10px 15px;
  border-left: 5px solid #42b883;
  border-radius: 4px;
  font-size: 1.2rem;
}

/* 1. Style แสดงสินค้าแบบคอลัมน์ (Grid/Cards) */
.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
  gap: 20px;
  margin-top: 20px;
}

.card {
  border: 1px solid #e1e8ed;
  border-radius: 8px;
  padding: 15px;
  text-align: center;
  background: #fff;
  transition: transform 0.2s;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 5px 15px rgba(0,0,0,0.1);
}

.card-img {
  width: 100%;
  height: 160px;
  object-fit: contain;
  margin-bottom: 10px;
}

.card-title {
  font-size: 0.95rem;
  font-weight: bold;
  height: 40px;
  overflow: hidden;
  margin: 10px 0;
  color: #2c3e50;
}

.card-price {
  font-size: 1.1rem;
  color: #e74c3c;
  font-weight: bold;
}

.card-category {
  font-size: 0.8rem;
  color: #7f8c8d;
  text-transform: uppercase;
}

/* 2 & 3. Style แสดงข้อมูลแบบตาราง (Table) */
.table-container {
  overflow-x: auto;
  margin-top: 15px;
}

.styled-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.9rem;
}

.styled-table thead tr {
  background-color: #42b883;
  color: #ffffff;
  text-align: left;
}

.styled-table th, 
.styled-table td {
  padding: 12px 15px;
  border: 1px solid #e1e8ed;
}

.styled-table tbody tr:nth-of-type(even) {
  background-color: #f8f9fa;
}

.styled-table tbody tr:hover {
  background-color: #f1f3f5;
}

.table-img {
  width: 45px;
  height: 45px;
  object-fit: contain;
}

.loading {
  text-align: center;
  font-size: 1.1rem;
  color: #666;
  padding: 20px;
}
</style>