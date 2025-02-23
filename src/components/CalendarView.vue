<template>
  <div class="container">
    <div class="title">
      <div class="thisYear">
        <span>TWO THOUSAND</span>
        <span>AND</span>
        <span>{{ yearSpell }}</span>
        </div>
      <div class="quote">Time is, time was, and time is past.</div>
    </div>
    <div class="calendar">
      <div v-for="(item, index) in yearDays" :key="index"
        :class="[isMonthHeader(index) ? 'is-month' : 'is-day', isDayPassed(index) ? 'dayPassed' : 'baseStyle']">
        {{ item }}
      </div>
    </div>
    <div class="footer">
      <a href="https://github.com/RhettTien/Elekoo-Calendar" target="_blank" style="text-decoration: none;color: black;">
        <svg width="18px" height="18px" viewBox="0 0 20 20" version="1.1">
          <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
            <g id="Dribbble-Light-Preview" transform="translate(-140.000000, -7559.000000)" fill="#000000">
              <g id="icons" transform="translate(56.000000, 160.000000)">
                <path
                  d="M94,7399 C99.523,7399 104,7403.59 104,7409.253 C104,7413.782 101.138,7417.624 97.167,7418.981 C96.66,7419.082 96.48,7418.762 96.48,7418.489 C96.48,7418.151 96.492,7417.047 96.492,7415.675 C96.492,7414.719 96.172,7414.095 95.813,7413.777 C98.04,7413.523 100.38,7412.656 100.38,7408.718 C100.38,7407.598 99.992,7406.684 99.35,7405.966 C99.454,7405.707 99.797,7404.664 99.252,7403.252 C99.252,7403.252 98.414,7402.977 96.505,7404.303 C95.706,7404.076 94.85,7403.962 94,7403.958 C93.15,7403.962 92.295,7404.076 91.497,7404.303 C89.586,7402.977 88.746,7403.252 88.746,7403.252 C88.203,7404.664 88.546,7405.707 88.649,7405.966 C88.01,7406.684 87.619,7407.598 87.619,7408.718 C87.619,7412.646 89.954,7413.526 92.175,7413.785 C91.889,7414.041 91.63,7414.493 91.54,7415.156 C90.97,7415.418 89.522,7415.871 88.63,7414.304 C88.63,7414.304 88.101,7413.319 87.097,7413.247 C87.097,7413.247 86.122,7413.234 87.029,7413.87 C87.029,7413.87 87.684,7414.185 88.139,7415.37 C88.139,7415.37 88.726,7417.2 91.508,7416.58 C91.513,7417.437 91.522,7418.245 91.522,7418.489 C91.522,7418.76 91.338,7419.077 90.839,7418.982 C86.865,7417.627 84,7413.783 84,7409.253 C84,7403.59 88.478,7399 94,7399"
                  id="github-[#142]">
                </path>
              </g>
            </g>
          </g>
        </svg>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      yearSpell: null,
      yearDays: this.generateYearDays()
    };
  },
  created() {
    const { toWords } = require('number-to-words');
    const year = new Date().getFullYear() % 100;
    this.yearSpell = toWords(year).toUpperCase().replace(",", "");
  },
  methods: {
    generateYearDays() {
      // 获取当前年份
      const year = new Date().getFullYear();
      const daysArray = [];
      for (let month = 0; month < 12; month++) {
        // 添加月份的首字母
        const monthName = new Date(year, month).toLocaleString('en-US', { month: 'long' });
        daysArray.push(monthName.charAt(0));
        // 添加该月的所有天数
        const daysInMonth = new Date(year, month + 1, 0).getDate();
        for (let day = 1; day <= daysInMonth; day++) {
          daysArray.push(day.toString());
        }
      }
      return daysArray;
    },
    isMonthHeader(index) {
      // 计算每个月首字母的索引
      let monthStartIndex = 0;
      for (let month = 0; month < 12; month++) {
        if (index === monthStartIndex) {
          return true;
        }
        // 跳过当前月份的所有天数
        monthStartIndex += 1 + new Date(new Date().getFullYear(), month + 1, 0).getDate();
      }
      return false;
    },
    isDayPassed(dayOfYear) {
      const now = new Date();
      const year = now.getFullYear();
      // 获取月份的数字
      const monthNumber = now.getMonth();
      // 创建当年1月1日的日期对象
      const targetDate = new Date(year, 0, 1);
      // 调整为传入的天数对应的日期
      targetDate.setDate(dayOfYear - monthNumber);
      // 设置时间为当天的最后一刻（23:59:59.999）
      targetDate.setHours(23, 59, 59, 999);
      return targetDate < now;
    },
    numberToEnglish(num) {
      return new Intl.NumberFormat('en-US', {
        style: 'spellout'
      }).format(num);
    }
  }
};
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  margin: auto 16px;
}

.title {
  margin: auto 36px;
  padding-left: 10px;
}

.thisYear {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  font-size: 60px;
  font-weight: bold;
  color: bisque;
  text-align: left;
}

.quote {
  font-size: 36px;
  font-weight: bold;
  text-align: left;
  margin: 20px auto;
}

.calendar {
  display: flex;
  flex-wrap: wrap;
  gap: auto;
  font-family: Arial, sans-serif;
  margin: auto 36px;
}

.baseStyle {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 10px;
  width: 38px;
  height: 38px;
  border: 1px solid #dbdbdb;
  border-radius: 50%;
  text-align: center;
  box-sizing: border-box;
  background-color: #f9f9f9;
}

.dayPassed {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 10px;
  width: 38px;
  height: 38px;
  color: #000000;
  border: 1px solid #000000;
  border-radius: 50%;
  text-align: center;
  box-sizing: border-box;
  background-color: #000000 !important;
}

.is-month {
  font-weight: bold;
}

.footer {
  position: fixed;
  margin: auto;
  right: 30px;
}

@media only screen and (max-width: 768px) {

  .container,
  .title,
  .calendar {
    margin: auto 6px;
  }

  .thisYear {
    font-size: 32px;
  }

  .quote {
    font-size: 16px;
    margin: 10px auto;
  }

  .baseStyle,
  .dayPassed {
    margin: 8px;
    width: 32px;
    height: 32px;
  }
}
</style>