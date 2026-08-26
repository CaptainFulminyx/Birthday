<template>
  <div class="calendar-wrapper">
    <div class="calendar">
      <!-- Header -->
      <div class="calendar-header">
        <button @click="prevMonth" class="nav-btn" aria-label="Previous month">
          <svg
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
          >
            <path d="M15 18l-6-6 6-6" />
          </svg>
        </button>
        <div class="month-year">
          <span class="month">{{ monthName }}</span>
          <span class="year">{{ year }}</span>
        </div>
        <button @click="nextMonth" class="nav-btn" aria-label="Next month">
          <svg
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
          >
            <path d="M9 18l6-6-6-6" />
          </svg>
        </button>
      </div>

      <!-- Weekdays -->
      <div class="weekdays">
        <div v-for="day in WEEKDAYS" :key="day" class="weekday">
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
            today: day.isToday,
            highlighted: day.isHighlighted,
            weekend: day.isWeekend,
          }"
        >
          <span class="day-number">{{ day.day }}</span>
          <div v-if="day.isHighlighted" class="highlight-badge">🎯</div>
        </div>
      </div>

      <!-- Legend -->
      <div class="legend">
        <div class="legend-item">
          <span class="legend-dot highlighted-dot"></span>
          <span>Highlighted (31st)</span>
        </div>
        <div class="legend-item">
          <span class="legend-dot today-dot"></span>
          <span>Today</span>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, computed, reactive } from "vue";

// Constants
const WEEKDAYS = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"];
const HIGHLIGHT_DAY = 31;
const HIGHLIGHT_MONTH = 7; // August (0-indexed)
const TOTAL_CELLS = 42; // 6 rows × 7 columns

// State
const currentDate = reactive({
  month: 7, // August
  year: 2026,
});

// Today's date
const today = new Date();
const todayInfo = {
  day: today.getDate(),
  month: today.getMonth(),
  year: today.getFullYear(),
};

// Computed
const monthName = computed(() => {
  return new Date(currentDate.year, currentDate.month).toLocaleString(
    "default",
    {
      month: "long",
    },
  );
});

const year = computed(() => currentDate.year);

const calendarDays = computed(() => {
  const firstDayOfMonth = new Date(currentDate.year, currentDate.month, 1);
  const lastDayOfMonth = new Date(currentDate.year, currentDate.month + 1, 0);

  // Get first day of month (0=Sun, 1=Mon, ...)
  let firstDayIndex = firstDayOfMonth.getDay();
  // Adjust to make Monday first (0=Mon, 6=Sun)
  firstDayIndex = firstDayIndex === 0 ? 6 : firstDayIndex - 1;

  const daysInMonth = lastDayOfMonth.getDate();
  const daysInPrevMonth = new Date(
    currentDate.year,
    currentDate.month,
    0,
  ).getDate();

  const days = [];

  // Helper to check if a date is today
  const isToday = (day, month, year) => {
    return (
      day === todayInfo.day &&
      month === todayInfo.month &&
      year === todayInfo.year
    );
  };

  // Helper to check if a date should be highlighted
  const isHighlighted = (day, month) => {
    return day === HIGHLIGHT_DAY && month === HIGHLIGHT_MONTH;
  };

  // Helper to check if a date is weekend
  const isWeekend = (date) => {
    const dayOfWeek = date.getDay();
    return dayOfWeek === 0 || dayOfWeek === 6;
  };

  // Previous month days
  const startOffset = firstDayIndex;
  for (let i = startOffset - 1; i >= 0; i--) {
    const day = daysInPrevMonth - i;
    const date = new Date(currentDate.year, currentDate.month - 1, day);
    days.push({
      day,
      date: date.toISOString(),
      isCurrentMonth: false,
      isToday: false,
      isHighlighted: false,
      isWeekend: isWeekend(date),
    });
  }

  // Current month days
  for (let i = 1; i <= daysInMonth; i++) {
    const date = new Date(currentDate.year, currentDate.month, i);
    days.push({
      day: i,
      date: date.toISOString(),
      isCurrentMonth: true,
      isToday: isToday(i, currentDate.month, currentDate.year),
      isHighlighted: isHighlighted(i, currentDate.month),
      isWeekend: isWeekend(date),
    });
  }

  // Next month days to fill grid
  const remaining = TOTAL_CELLS - days.length;
  for (let i = 1; i <= remaining; i++) {
    const date = new Date(currentDate.year, currentDate.month + 1, i);
    days.push({
      day: i,
      date: date.toISOString(),
      isCurrentMonth: false,
      isToday: false,
      isHighlighted: false,
      isWeekend: isWeekend(date),
    });
  }

  return days;
});

// Methods
const prevMonth = () => {
  if (currentDate.month === 0) {
    currentDate.month = 11;
    currentDate.year--;
  } else {
    currentDate.month--;
  }
};

const nextMonth = () => {
  if (currentDate.month === 11) {
    currentDate.month = 0;
    currentDate.year++;
  } else {
    currentDate.month++;
  }
};
</script>
