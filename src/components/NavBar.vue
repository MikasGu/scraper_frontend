<template>
  <nav>
    <div class="nav-container">
      <div class="nav-item logo-container">
        <span class="logo-text">VACATION SCRAPER</span>
      </div>
      <div class="nav-item ip-display">
        {{ userIp }}
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'NavBar',
  data() {
    return {
      userIp: 'Loading...',
    };
  },
  created() {
    this.fetchUserIp();
  },
  methods: {
    async fetchUserIp() {
      try {
        const response = await fetch('https://api64.ipify.org?format=json');
        const data = await response.json();
        this.userIp = data.ip;
      } catch (error) {
        console.error('Error fetching IP:', error);
        this.userIp = 'Error fetching IP';
      }
    },
  },
};
</script>

<style scoped>
nav {
  background: #ffffff1a;
  padding: 10px 0;
}

.nav-container {
  display: flex;
  justify-content: space-between;
  max-width: 800px;
  margin: 0 auto;
}

.nav-item {
  text-decoration: none;
  font-weight: bold;
  padding: 5px;
  color: #fff;
  text-transform: uppercase;
}

.ip-display {
  text-align: right;
  color: #fff1f1be;
  margin-top: 5px;
}

.logo-container {
  display: flex;
  align-items: center;
}

.logo-text {
  font-family: 'Arial', sans-serif;
  font-size: 24px;
  color: #fff;
  text-transform: uppercase;
  letter-spacing: 2px;
  margin-right: 10px;
}

</style>
