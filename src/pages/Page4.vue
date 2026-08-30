<template>
  <div class="calendar-wrapper">
    <div class="calendar">
      <!-- Header -->
      <div class="calendar-header">
        <div class="month-year">
          <span class="month">August</span>
          <span class="year"> 2026</span>
        </div>
      </div>

      <!-- Weekdays -->
      <div class="weekdays">
        <div v-for="day in weekdays" :key="day" class="weekday">
          {{ day }}
        </div>
      </div>

      <!-- Days -->
      <div class="days-grid">
        <div
          v-for="day in calendarDays"
          :key="day.date"
          class="day-cell"
          :class="{ highlighted: day.isHighlighted }"
          :style="day.day === 1 ? { gridColumnStart: firstDayColumn } : {}"
        >
          <span class="day-number">{{ day.day }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from "vue";

const weekdays = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"];
const HIGHLIGHT_DAY = 31;
const MONTH = 7; // August (0-indexed)
const YEAR = 2026;

// Offset for day 1 so it aligns under the correct weekday (Mon=1, Tue=2, ...)
const firstDayColumn = computed(() => {
  const firstDay = new Date(YEAR, MONTH, 1).getDay();
  return firstDay === 0 ? 7 : firstDay;
});

const calendarDays = computed(() => {
  const daysInMonth = new Date(YEAR, MONTH + 1, 0).getDate();
  const days = [];

  for (let i = 1; i <= daysInMonth; i++) {
    const date = new Date(YEAR, MONTH, i);
    days.push({
      day: i,
      date: date.toISOString(),
      isHighlighted: i === HIGHLIGHT_DAY,
    });
  }

  return days;
});
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.calendar-wrapper {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 16px;
  background: #fce4ec;
}

.calendar {
  max-width: 600px;
  width: 100%;
  background: #ffffff;
  border-radius: 24px;
  padding: 24px 20px 28px;
  border: 3px solid #d81b60;
  box-shadow: 0 8px 0 #d81b60;
}

/* Header */
.calendar-header {
  text-align: center;
  margin-bottom: 24px;
  padding-bottom: 16px;
  border-bottom: 3px solid #f8bbd0;
}

.month-year {
  font-size: 32px;
  font-weight: 800;
  letter-spacing: 1px;
  background: linear-gradient(135deg, #d81b60 0%, #f06292 50%, #d81b60 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* Weekdays */
.weekdays {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 4px;
  margin-bottom: 8px;
  padding: 0 2px;
}

.weekday {
  text-align: center;
  font-size: 13px;
  font-weight: 700;
  color: #d81b60;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  padding: 8px 0;
  border-bottom: 2px solid #f8bbd0;
}

/* Days Grid */
.days-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 4px;
}

.day-cell {
  aspect-ratio: 1/1;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 12px;
  border: 2px solid #f8bbd0;
  background: #ffffff;
  transition: all 0.2s ease;
  min-height: 44px;
  font-size: 16px;
  font-weight: 600;
  color: #424242;
}

.day-number {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}

/* Highlighted - August 31 */
.highlighted {
  border: 3px solid #d81b60 !important;
  background: linear-gradient(135deg, #fce4ec 0%, #f8bbd0 100%) !important;
  box-shadow: 0 4px 0 #d81b60;
  transform: scale(1.05);
  position: relative;
  z-index: 2;
}

.highlighted .day-number {
  font-size: 20px;
  font-weight: 900;
  color: #d81b60;
}

/* Responsive */
@media (max-width: 500px) {
  .calendar {
    padding: 16px 12px 20px;
    border-radius: 18px;
    border-width: 2px;
  }

  .month-year {
    font-size: 26px;
  }

  .day-cell {
    min-height: 38px;
    font-size: 14px;
    border-radius: 8px;
    border-width: 1.5px;
  }

  .weekday {
    font-size: 11px;
    padding: 6px 0;
  }

  .highlighted {
    border-width: 2px !important;
    transform: scale(1.04);
  }

  .highlighted .day-number {
    font-size: 17px;
  }
}

@media (max-width: 380px) {
  .calendar {
    padding: 12px 8px 16px;
  }

  .month-year {
    font-size: 22px;
  }

  .day-cell {
    min-height: 32px;
    font-size: 12px;
    border-radius: 6px;
    border-width: 1px;
  }

  .weekday {
    font-size: 10px;
    padding: 4px 0;
  }

  .highlighted .day-number {
    font-size: 14px;
  }
}
</style>
