<template>
  <div class="app">
    <div class="left-panel">
      <h2>Добавление нового навыка</h2>
      <form @submit.prevent="addSkill">
        <div class="form-group">
          <label for="skillName">Название навыка</label>
          <input type="text" id="skillName" v-model="newSkill.name" required>
        </div>
        <div class="form-group">
          <label for="skillLevel">Уровень владения</label>
          <select id="skillLevel" v-model="newSkill.level" required>
            <option value="Начинающий">Начинающий</option>
            <option value="Средний">Средний</option>
            <option value="Продвинутый">Продвинутый</option>
          </select>
        </div>
        <div class="form-group">
          <label for="skillDescription">Описание навыка</label>
          <textarea id="skillDescription" v-model="newSkill.description"></textarea>
        </div>
        <button type="submit">Добавить навык</button>
      </form>
    </div>
    <div class="right-panel">
      <h2>Список навыков</h2>
      <ul>
        <li v-for="skill in skills" :key="skill.id">
          <h3>{{ skill.name }}</h3>
          <p>Уровень владения: {{ skill.level }}</p>
          <p>{{ skill.description }}</p>
          <button @click="removeSkill(skill)">Удалить</button>
          <button @click="resetSkills">Обнулить все навыки</button><!-- Кнопка для обнуления всех навыков -->
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newSkill: {
        name: '',
        level: 'Начинающий',
        description: ''
      },
      skills: []
    };
  },
  methods: {
    addSkill() {
      this.skills.push(this.newSkill);
      this.saveSkillsToStorage(); // Сохраняем навыки в localStorage
      this.newSkill = {
        name: '',
        level: 'Начинающий',
        description: ''
      };
    },
    removeSkill(skill) {
      const index = this.skills.indexOf(skill);
      if (index > -1) {
        this.skills.splice(index, 1);
        this.saveSkillsToStorage(); // Сохраняем навыки в localStorage
      }
    },
    saveSkillsToStorage() {
      localStorage.setItem('skills', JSON.stringify(this.skills));
    },
    loadSkillsFromStorage() {
      const storedSkills = localStorage.getItem('skills');
      if (storedSkills) {
        this.skills = JSON.parse(storedSkills);
      }
    }
  },
  mounted() {
    this.loadSkillsFromStorage(); // Восстанавливаем навыки из localStorage при загрузке страницы
  }
};
</script>


<style scoped>
/* Стили компонента */
</style>
