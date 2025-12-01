<template>
  <div class="header-info">
    <div class="info-item date-info">
      <span class="date">{{ currentDate }}</span>
      <span class="weekday">{{ currentWeekday }}</span>
    </div>
    <div class="divider"></div>
    <div class="info-item weather-info">
      <span class="temp">{{ weather.temp }}°C</span>
      <span class="condition">{{ weather.condition }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HeaderInfo',
  data() {
    return {
      currentDate: '',
      currentWeekday: '',
      weather: {
        temp: '--',
        condition: '加载中...',
      },
    };
  },
  mounted() {
    this.updateDateTime();
    this.fetchWeather();
    setInterval(this.updateDateTime, 60000);
  },
  methods: {
    updateDateTime() {
      const now = new Date();
      const year = now.getFullYear();
      const month = String(now.getMonth() + 1).padStart(2, '0');
      const day = String(now.getDate()).padStart(2, '0');
      this.currentDate = `${year}年${month}月${day}日`;
      
      const weekdays = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
      this.currentWeekday = weekdays[now.getDay()];
    },
    async fetchWeather() {
      try {
        const response = await fetch('https://wttr.in/?format=j1');
        const data = await response.json();
        const current = data.current_condition[0];
        this.weather.temp = current.temp_C;
        this.weather.condition = this.translateWeather(current.weatherDesc[0].value);
      } catch (error) {
        console.error('Failed to fetch weather:', error);
        this.weather.condition = '晴';
      }
    },
    translateWeather(desc) {
      const translations = {
        'Clear': '晴',
        'Sunny': '晴',
        'Partly cloudy': '多云',
        'Cloudy': '阴',
        'Overcast': '阴',
        'Mist': '薄雾',
        'Fog': '雾',
        'Light rain': '小雨',
        'Moderate rain': '中雨',
        'Heavy rain': '大雨',
        'Light snow': '小雪',
        'Moderate snow': '中雪',
        'Heavy snow': '大雪',
      };
      return translations[desc] || desc;
    },
  },
};
</script>
<template>
  <div class="header-info">
    <div class="info-item date-info">
      <span class="date">{{ currentDate }}</span>
      <span class="weekday">{{ currentWeekday }}</span>
    </div>
    <div class="divider"></div>
    <div class="info-item weather-info">
      <span class="temp">{{ weather.temp }}°C</span>
      <span class="condition">{{ weather.condition }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HeaderInfo',
  data() {
    return {
      currentDate: '',
      currentWeekday: '',
      weather: {
        temp: '--',
        condition: '加载中...',
      },
    };
  },
  mounted() {
    this.updateDateTime();
    this.fetchWeather();
    setInterval(this.updateDateTime, 60000);
  },
  methods: {
    updateDateTime() {
      const now = new Date();
      const year = now.getFullYear();
      const month = String(now.getMonth() + 1).padStart(2, '0');
      const day = String(now.getDate()).padStart(2, '0');
      this.currentDate = `${year}年${month}月${day}日`;
      
      const weekdays = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
      this.currentWeekday = weekdays[now.getDay()];
    },
    async fetchWeather() {
      try {
        const response = await fetch('https://wttr.in/?format=j1');
        const data = await response.json();
        const current = data.current_condition[0];
        this.weather.temp = current.temp_C;
        this.weather.condition = this.translateWeather(current.weatherDesc[0].value);
      } catch (error) {
        console.error('Failed to fetch weather:', error);
        this.weather.condition = '晴';
      }
    },
    translateWeather(desc) {
      const translations = {
        'Clear': '晴',
        'Sunny': '晴',
        'Partly cloudy': '多云',
        'Cloudy': '阴',
        'Overcast': '阴',
        'Mist': '薄雾',
        'Fog': '雾',
        'Light rain': '小雨',
        'Moderate rain': '中雨',
        'Heavy rain': '大雨',
        'Light snow': '小雪',
        'Moderate snow': '中雪',
        'Heavy snow': '大雪',
      };
      return translations[desc] || desc;
    },
  },
};
</script>

<style lang="scss" scoped>
.header-info {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 0.8rem 1.5rem;
  background: rgba(255, 255, 255, 0.05);
  border-radius: var(--curve-factor);
  border: 1px solid rgba(255, 255, 255, 0.1);
  white-space: nowrap;
  min-height: 48px;

    .info-item {
      display: flex;
      flex-direction: column;
      gap: 0.2rem;
      
      span {
        color: var(--settings-text-color);
        line-height: 1.3;
      }
      
      .date, .temp {
        font-size: 1.2rem;
        font-weight: 600;
      }
      
      .weekday, .condition {
        font-size: 0.9rem;
        opacity: 0.75;
      }
    }

    .divider {
      width: 1px;
      height: 2.5rem;
      background: rgba(255, 255, 255, 0.2);
    }

    @media (max-width: 768px) {
      padding: 0.6rem 1rem;
      gap: 0.8rem;
      min-height: 42px;
      
      .info-item {
        .date, .temp {
          font-size: 1.05rem;
        }
        .weekday, .condition {
          font-size: 0.8rem;
        }
      }
      
      .divider {
        height: 2rem;
      }
    }

    @media (max-width: 480px) {
      gap: 0.6rem;
      padding: 0.5rem 0.8rem;
      min-height: 38px;
      
      .info-item {
        gap: 0.1rem;
        
        .date, .temp {
          font-size: 0.95rem;
        }
        .weekday, .condition {
          font-size: 0.75rem;
        }
      }
    }
}
</style>
