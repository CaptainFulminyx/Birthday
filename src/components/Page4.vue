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
          :class="{
            'other-month': !day.isCurrentMonth,
            highlighted: day.isHighlighted,
            weekend: day.isWeekend,
          }"
        >
          <span class="day-number">{{ day.day }}</span>
        </div>
      </div>

      <!-- Highlight Legend -->
    </div>
  </div>
</template>

<script setup>
import { computed } from "vue";

// Constants
const weekdays = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"];
const HIGHLIGHT_DAY = 31;
const HIGHLIGHT_MONTH = 7; // August (0-indexed)
const MONTH = 7; // August
const YEAR = 2026;
const TOTAL_CELLS = 42;

// Computed
const calendarDays = computed(() => {
  const firstDayOfMonth = new Date(YEAR, MONTH, 1);
  const lastDayOfMonth = new Date(YEAR, MONTH + 1, 0);

  // Get first day of month (0=Sun, 1=Mon, ...)
  let firstDayIndex = firstDayOfMonth.getDay();
  firstDayIndex = firstDayIndex === 0 ? 6 : firstDayIndex - 1;

  const daysInMonth = lastDayOfMonth.getDate();
  const daysInPrevMonth = new Date(YEAR, MONTH, 0).getDate();

  const days = [];

  // Helper to check if date is weekend
  const isWeekend = (date) => {
    const dayOfWeek = date.getDay();
    return dayOfWeek === 0 || dayOfWeek === 6;
  };

  // Previous month days
  for (let i = firstDayIndex - 1; i >= 0; i--) {
    const day = daysInPrevMonth - i;
    const date = new Date(YEAR, MONTH - 1, day);
    days.push({
      day,
      date: date.toISOString(),
      isCurrentMonth: false,
      isHighlighted: false,
      isWeekend: isWeekend(date),
    });
  }

  // Current month days
  for (let i = 1; i <= daysInMonth; i++) {
    const date = new Date(YEAR, MONTH, i);
    days.push({
      day: i,
      date: date.toISOString(),
      isCurrentMonth: true,
      isHighlighted: i === HIGHLIGHT_DAY,
      isWeekend: isWeekend(date),
    });
  }

  // Next month days
  const remaining = TOTAL_CELLS - days.length;
  for (let i = 1; i <= remaining; i++) {
    const date = new Date(YEAR, MONTH + 1, i);
    days.push({
      day: i,
      date: date.toISOString(),
      isCurrentMonth: false,
      isHighlighted: false,
      isWeekend: isWeekend(date),
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
  text-shadow: none;
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

/* Other month days */
.other-month {
  border-color: #fce4ec;
  background: #fafafa;
}

.other-month .day-number {
  color: #bdbdbd;
  font-weight: 400;
}

/* Weekend days */
.weekend:not(.other-month) {
  border-color: #ec407a;
  background: #fff5f7;
}

.weekend .day-number {
  color: #d81b60;
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
  position: relative;
}

.highlighted .day-number::after {
  content: "★";
  position: absolute;
  top: -8px;
  right: -12px;
  font-size: 12px;
  color: #d81b60;
}

/* Legend */
.legend {
  margin-top: 20px;
  padding-top: 16px;
  border-top: 3px solid #f8bbd0;
  display: flex;
  justify-content: center;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 15px;
  font-weight: 600;
  color: #424242;
}

.legend-dot {
  width: 18px;
  height: 18px;
  border-radius: 6px;
  border: 2px solid #d81b60;
  background: linear-gradient(135deg, #fce4ec, #f8bbd0);
  flex-shrink: 0;
}

.highlight-text {
  color: #d81b60;
  font-weight: 800;
  background: #fce4ec;
  padding: 2px 10px;
  border-radius: 12px;
  border: 2px solid #d81b60;
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

  .highlighted .day-number::after {
    font-size: 10px;
    top: -6px;
    right: -10px;
  }

  .legend-item {
    font-size: 13px;
    gap: 8px;
  }

  .legend-dot {
    width: 14px;
    height: 14px;
    border-radius: 4px;
  }

  .highlight-text {
    padding: 1px 8px;
    font-size: 12px;
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

  .highlighted .day-number::after {
    font-size: 8px;
    top: -4px;
    right: -8px;
  }
}
</style>
