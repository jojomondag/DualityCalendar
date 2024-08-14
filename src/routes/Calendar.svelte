<script lang="ts">
  import { onMount } from 'svelte';
  import { format, startOfMonth, endOfMonth, startOfWeek, endOfWeek, addDays, subMonths, addMonths, isSameMonth } from 'date-fns';

  let currentMonth: Date = new Date();
  let days: Date[] = [];

  const getDaysInMonth = (date: Date): Date[] => {
    const startDate = startOfWeek(startOfMonth(date));
    const endDate = endOfWeek(endOfMonth(date));

    const dates: Date[] = [];
    let day: Date = startDate;

    while (day <= endDate) {
      dates.push(day);
      day = addDays(day, 1);
    }

    return dates;
  };

  const nextMonth = () => {
    currentMonth = addMonths(currentMonth, 1);
    days = getDaysInMonth(currentMonth);
  };

  const previousMonth = () => {
    currentMonth = subMonths(currentMonth, 1);
    days = getDaysInMonth(currentMonth);
  };

  onMount(() => {
    days = getDaysInMonth(currentMonth);
  });
</script>

<div class="calendar">
  <div class="header">
    <button on:click={previousMonth}>&lt;</button>
    <h2>{format(currentMonth, 'MMMM yyyy')}</h2>
    <button on:click={nextMonth}>&gt;</button>
  </div>
  <div class="days">
    {#each ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'] as dayName}
      <div class="day-name">{dayName}</div>
    {/each}
    {#each days as day}
      <div class="day" class:other-month={!isSameMonth(day, currentMonth)}>
        {format(day, 'd')}
      </div>
    {/each}
  </div>
</div>

<style>
  .calendar {
    width: 100%;
    max-width: 800px;
    margin: 0 auto;
  }
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
  }
  .days {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 5px;
  }
  .day-name, .day {
    text-align: center;
    padding: 10px;
    border: 1px solid #ddd;
  }
  .day-name {
    font-weight: bold;
    background-color: #f0f0f0;
  }
  .other-month {
    color: #999;
  }
</style>