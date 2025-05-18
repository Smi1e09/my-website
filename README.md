<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ИС УЧК: Развитие человеческого капитала</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Inter', sans-serif;
    }
    .apple-btn {
      transition: background-color 0.3s ease, transform 0.2s ease;
    }
    .apple-btn:hover {
      transform: translateY(-1px);
    }
    .sidebar {
      backdrop-filter: blur(10px);
      background: rgba(255, 255, 255, 0.1);
    }
    .modal-backdrop {
      backdrop-filter: blur(5px);
    }
    .card {
      transition: transform 0.2s ease;
    }
    .card:hover {
      transform: translateY(-2px);
    }
    table {
      width: 100%;
      border-collapse: separate;
      border-spacing: 0;
    }
    th, td {
      padding: 12px;
      text-align: left;
    }
    th {
      background: #f7fafc;
      font-weight: 600;
    }
    tr:hover {
      background: #f1f5f9;
    }
    .title-3d {
      font-size: 4rem;
      font-weight: 700;
      color: #2563eb;
      text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2), -2px -2px 4px rgba(255, 255, 255, 0.5);
      background: linear-gradient(45deg, #1e40af, #60a5fa);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }
  </style>
</head>
<body class="bg-gray-50">
  <div class="flex min-h-screen">
    <!-- Боковое меню -->
    <aside id="sidebar" class="w-64 sidebar text-gray-800 p-6 hidden">
      <h2 class="text-2xl font-semibold mb-6">ИС УЧК</h2>
      <nav>
        <ul>
          <li><button id="dashboard-btn" class="w-full text-left p-3 hover:bg-gray-200 rounded apple-btn">Панель управления</button></li>
          <li id="admin-nav" class="hidden">
            <button id="employees-btn" class="w-full text-left p-3 hover:bg-gray-200 rounded apple-btn">Сотрудники</button>
            <button id="courses-btn" class="w-full text-left p-3 hover:bg-gray-200 rounded apple-btn">Курсы</button>
            <button id="reports-btn" class="w-full text-left p-3 hover:bg-gray-200 rounded apple-btn">Отчёты</button>
          </li>
          <li id="manager-nav" class="hidden">
            <button id="department-btn" class="w-full text-left p-3 hover:bg-gray-200 rounded apple-btn">Отдел</button>
            <button id="evaluations-btn" class="w-full text-left p-3 hover:bg-gray-200 rounded apple-btn">Оценки</button>
          </li>
          <li id="employee-nav" class="hidden"><button id="profile-btn" class="w-full text-left p-3 hover:bg-gray-200 rounded apple-btn">Личный кабинет</button></li>
          <li><button id="logout-btn" class="w-full text-left p-3 hover:bg-gray-200 rounded apple-btn">Выйти</button></li>
        </ul>
      </nav>
    </aside>

    <!-- Основной контент -->
    <main class="flex-1">
      <!-- Панель входа -->
      <div id="login-panel" class="flex flex-col items-center justify-center min-h-screen">
        <h1 class="title-3d mb-8">SM</h1>
        <div class="max-w-md bg-white p-8 rounded-2xl shadow-sm">
          <h2 class="text-3xl font-semibold mb-6 text-center text-gray-900">Вход в систему</h2>
          <select id="user-role" class="w-full p-3 mb-4 border rounded-xl bg-gray-50 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300">
            <option value="admin">Администратор</option>
            <option value="manager">Менеджер</option>
            <option value="employee">Сотрудник</option>
          </select>
          <button id="login-btn" class="w-full p-3 bg-blue-500 text-white rounded-xl hover:bg-blue-600 apple-btn">Войти</button>
        </div>
      </div>

      <!-- Панель управления -->
      <div id="dashboard" class="hidden p-8">
        <h2 class="text-4xl font-semibold mb-8 text-gray-900">Панель управления</h2>

        <!-- Секция администратора: Сотрудники -->
        <div id="admin-section" class="hidden">
          <div class="flex justify-between items-center mb-6">
            <h3 class="text-2xl font-semibold text-gray-900">Сотрудники</h3>
            <button id="open-add-employee-modal" class="p-3 bg-blue-500 text-white rounded-xl hover:bg-blue-600 apple-btn">Добавить сотрудника</button>
          </div>
          <div class="flex space-x-4 mb-6">
            <input id="filter-name" type="text" placeholder="Поиск по имени" class="p-3 border rounded-xl bg-gray-50 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300 flex-1">
            <input id="filter-department" type="text" placeholder="Фильтр по отделу" class="p-3 border rounded-xl bg-gray-50 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300 flex-1">
          </div>
          <div id="employee-list" class="grid gap-6 md:grid-cols-2 lg:grid-cols-3"></div>
          <div class="mt-6 flex justify-between">
            <button id="prev-page" class="p-2 bg-gray-200 rounded-xl hover:bg-gray-300 apple-btn">Назад</button>
            <span id="page-info" class="text-gray-600">Страница 1</span>
            <button id="next-page" class="p-2 bg-gray-200 rounded-xl hover:bg-gray-300 apple-btn">Вперёд</button>
          </div>
        </div>

        <!-- Секция администратора: Курсы -->
        <div id="courses-section" class="hidden bg-white p-8 rounded-2xl shadow-sm">
          <h3 class="text-2xl font-semibold mb-6 text-gray-900">Курсы обучения</h3>
          <button id="open-add-course-modal" class="p-3 bg-blue-500 text-white rounded-xl hover:bg-blue-600 apple-btn mb-6">Добавить курс</button>
          <table>
            <thead>
              <tr>
                <th>Название</th>
                <th>Продолжительность</th>
                <th>Назначено</th>
                <th>Действия</th>
              </tr>
            </thead>
            <tbody id="courses-table"></tbody>
          </table>
        </div>

        <!-- Секция администратора: Отчёты -->
        <div id="reports-section" class="hidden bg-white p-8 rounded-2xl shadow-sm">
          <h3 class="text-2xl font-semibold mb-6 text-gray-900">Отчёты</h3>
          <button id="generate-report-btn" class="p-3 bg-blue-500 text-white rounded-xl hover:bg-blue-600 apple-btn mb-6">Сгенерировать отчёт</button>
          <div id="report-output" class="bg-gray-50 p-4 rounded-xl text-gray-700"></div>
        </div>

        <!-- Секция менеджера: Отдел -->
        <div id="manager-section" class="hidden bg-white p-8 rounded-2xl shadow-sm">
          <h3 class="text-2xl font-semibold mb-6 text-gray-900">Управление отделом</h3>
          <p class="mb-4 text-gray-600">Список сотрудников вашего отдела:</p>
          <ul id="department-list" class="list-disc pl-5 text-gray-700"></ul>
        </div>

        <!-- Секция менеджера: Оценки -->
        <div id="evaluations-section" class="hidden bg-white p-8 rounded-2xl shadow-sm">
          <h3 class="text-2xl font-semibold mb-6 text-gray-900">Оценки производительности</h3>
          <button id="open-add-evaluation-modal" class="p-3 bg-blue-500 text-white rounded-xl hover:bg-blue-600 apple-btn mb-6">Добавить оценку</button>
          <table>
            <thead>
              <tr>
                <th>Сотрудник</th>
                <th>Дата</th>
                <th>Рейтинг</th>
                <th>Комментарий</th>
              </tr>
            </thead>
            <tbody id="evaluations-table"></tbody>
          </table>
        </div>

        <!-- Секция сотрудника: Личный кабинет -->
        <div id="employee-section" class="hidden bg-white p-8 rounded-2xl shadow-sm">
          <h3 class="text-2xl font-semibold mb-6 text-gray-900">Личный кабинет</h3>
          <div class="grid gap-4 mb-6">
            <p class="text-gray-600"><strong>Имя:</strong> <span id="employee-name-display">Иванов Иван</span></p>
            <p class="text-gray-600"><strong>Отдел:</strong> <span id="employee-department-display">ИТ</span></p>
            <p class="text-gray-600"><strong>Должность:</strong> <span id="employee-position-display">Разработчик</span></p>
            <p class="text-gray-600"><strong>Email:</strong> <span id="employee-email-display">ivanov@example.com</span></p>
            <p class="text-gray-600"><strong>Дата найма:</strong> <span id="employee-hire-date-display">2023-01-15</span></p>
          </div>
          <div class="mt-6">
            <h4 class="text-lg font-medium text-gray-900">Ваши навыки</h4>
            <ul id="skills-list" class="list-disc pl-5 text-gray-700 mb-4"></ul>
            <div class="flex items-center">
              <input id="new-skill" type="text" placeholder="Новый навык" class="p-3 border rounded-xl bg-gray-50 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300 flex-1 mr-2">
              <button id="add-skill-btn" class="p-3 bg-blue-500 text-white rounded-xl hover:bg-blue-600 apple-btn">Добавить</button>
            </div>
          </div>
          <div class="mt-6">
            <h4 class="text-lg font-medium text-gray-900">Назначенные курсы</h4>
            <ul id="assigned-courses-list" class="list-disc pl-5 text-gray-700"></ul>
          </div>
        </div>
      </div>
    </main>
  </div>

  <!-- Модальное окно для сотрудников -->
  <div id="employee-modal" class="hidden fixed inset-0 modal-backdrop flex items-center justify-center">
    <div class="bg-white p-8 rounded-2xl shadow-sm max-w-md w-full">
      <h3 id="modal-title" class="text-2xl font-semibold mb-6 text-gray-900">Добавить сотрудника</h3>
      <input id="modal-employee-name" type="text" placeholder="ФИО" class="w-full p-3 mb-4 border rounded-xl bg-gray-50 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300">
      <input id="modal-employee-department" type="text" placeholder="Отдел" class="w-full p-3 mb-4 border rounded-xl bg-gray-50 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300">
      <input id="modal-employee-position" type="text" placeholder="Должность" class="w-full p-3 mb-4 border rounded-xl bg-gray-50 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300">
      <input id="modal-employee-email" type="email" placeholder="Email" class="w-full p-3 mb-4 border rounded-xl bg-gray-50 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300">
      <input id="modal-employee-hire-date" type="date" placeholder="Дата найма" class="w-full p-3 mb-4 border rounded-xl bg-gray-50 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300">
      <div class="flex justify-end">
        <button id="modal-cancel-btn" class="p-3 mr-2 bg-gray-200 rounded-xl hover:bg-gray-300 apple-btn">Отмена</button>
        <button id="modal-save-btn" class="p-3 bg-blue-500 text-white rounded-xl hover:bg-blue-600 apple-btn">Сохранить</button>
      </div>
    </div>
  </div>

  <!-- Модальное окно для курсов -->
  <div id="course-modal" class="hidden fixed inset-0 modal-backdrop flex items-center justify-center">
    <div class="bg-white p-8 rounded-2xl shadow-sm max-w-md w-full">
      <h3 class="text-2xl font-semibold mb-6 text-gray-900">Добавить курс</h3>
      <input id="modal-course-name" type="text" placeholder="Название курса" class="w-full p-3 mb-4 border rounded-xl bg-gray-50 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300">
      <input id="modal-course-duration" type="text" placeholder="Продолжительность (часы)" class="w-full p-3 mb-4 border rounded-xl bg-gray-50 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300">
      <select id="modal-course-assignee" class="w-full p-3 mb-4 border rounded-xl bg-gray-50 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300">
        <option value="">Выберите сотрудника</option>
      </select>
      <div class="flex justify-end">
        <button id="modal-course-cancel-btn" class="p-3 mr-2 bg-gray-200 rounded-xl hover:bg-gray-300 apple-btn">Отмена</button>
        <button id="modal-course-save-btn" class="p-3 bg-blue-500 text-white rounded-xl hover:bg-blue-600 apple-btn">Сохранить</button>
      </div>
    </div>
  </div>

  <!-- Модальное окно для оценок -->
  <div id="evaluation-modal" class="hidden fixed inset-0 modal-backdrop flex items-center justify-center">
    <div class="bg-white p-8 rounded-2xl shadow-sm max-w-md w-full">
      <h3 class="text-2xl font-semibold mb-6 text-gray-900">Добавить оценку</h3>
      <select id="modal-evaluation-employee" class="w-full p-3 mb-4 border rounded-xl bg-gray-50 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300">
        <option value="">Выберите сотрудника</option>
      </select>
      <input id="modal-evaluation-date" type="date" class="w-full p-3 mb-4 border rounded-xl bg-gray-50 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300">
      <input id="modal-evaluation-rating" type="number" min="1" max="5" placeholder="Рейтинг (1-5)" class="w-full p-3 mb-4 border rounded-xl bg-gray-50 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300">
      <textarea id="modal-evaluation-comment" placeholder="Комментарий" class="w-full p-3 mb-4 border rounded-xl bg-gray-50 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300"></textarea>
      <div class="flex justify-end">
        <button id="modal-evaluation-cancel-btn" class="p-3 mr-2 bg-gray-200 rounded-xl hover:bg-gray-300 apple-btn">Отмена</button>
        <button id="modal-evaluation-save-btn" class="p-3 bg-blue-500 text-white rounded-xl hover:bg-blue-600 apple-btn">Сохранить</button>
      </div>
    </div>
  </div>

  <script>
    // Данные
    let employees = JSON.parse(localStorage.getItem('employees')) || [
      { id: 1, name: "Иванов Иван", department: "ИТ", position: "Разработчик", email: "ivanov@example.com", hireDate: "2023-01-15", skills: ["Программирование", "Управление проектами"], courses: [1] },
      { id: 2, name: "Петров Петр", department: "HR", position: "Рекрутер", email: "petrov@example.com", hireDate: "2022-06-10", skills: ["Рекрутинг"], courses: [] }
    ];
    let courses = JSON.parse(localStorage.getItem('courses')) || [
      { id: 1, name: "Основы программирования", duration: "40 часов", assignee: "Иванов Иван" }
    ];
    let evaluations = JSON.parse(localStorage.getItem('evaluations')) || [
      { id: 1, employee: "Иванов Иван", date: "2023-12-01", rating: 4, comment: "Хорошая работа" }
    ];
    let currentEmployeeId = null;
    let currentPage = 1;
    const itemsPerPage = 6;

    // Сохранение данных
    function saveData() {
      localStorage.setItem('employees', JSON.stringify(employees));
      localStorage.setItem('courses', JSON.stringify(courses));
      localStorage.setItem('evaluations', JSON.stringify(evaluations));
    }

    // Рендеринг сотрудников
    function renderEmployeeList(nameFilter = '', departmentFilter = '') {
      const container = document.getElementById('employee-list');
      container.innerHTML = '';
      const filteredEmployees = employees.filter(emp =>
        emp.name.toLowerCase().includes(nameFilter.toLowerCase()) &&
        emp.department.toLowerCase().includes(departmentFilter.toLowerCase())
      );
      const start = (currentPage - 1) * itemsPerPage;
      const end = start + itemsPerPage;
      const paginatedEmployees = filteredEmployees.slice(start, end);

      if (paginatedEmployees.length === 0) {
        container.innerHTML = '<p class="text-gray-600">Сотрудники не найдены.</p>';
      } else {
        paginatedEmployees.forEach(emp => {
          const card = document.createElement('div');
          card.className = 'card bg-white p-6 rounded-2xl shadow-sm';
          card.innerHTML = `
            <h4 class="text-lg font-semibold text-gray-900">${emp.name}</h4>
            <p class="text-gray-600">Отдел: ${emp.department}</p>
            <p class="text-gray-600">Должность: ${emp.position}</p>
            <p class="text-gray-600">Email: ${emp.email}</p>
            <p class="text-gray-600">Дата найма: ${emp.hireDate}</p>
            <p class="text-gray-600">Навыки: ${emp.skills.join(', ')}</p>
            <div class="mt-4 flex space-x-2">
              <button class="edit-btn p-2 bg-blue-500 text-white rounded-xl hover:bg-blue-600 apple-btn" data-id="${emp.id}">Редактировать</button>
              <button class="delete-btn p-2 bg-red-500 text-white rounded-xl hover:bg-red-600 apple-btn" data-id="${emp.id}">Удалить</button>
            </div>
          `;
          container.appendChild(card);
        });
      }

      document.getElementById('page-info').textContent = `Страница ${currentPage} из ${Math.ceil(filteredEmployees.length / itemsPerPage) || 1}`;
      document.getElementById('prev-page').disabled = currentPage === 1;
      document.getElementById('next-page').disabled = end >= filteredEmployees.length;

      document.querySelectorAll('.edit-btn').forEach(btn => {
        btn.addEventListener('click', () => openEditEmployeeModal(parseInt(btn.dataset.id)));
      });
      document.querySelectorAll('.delete-btn').forEach(btn => {
        btn.addEventListener('click', () => deleteEmployee(parseInt(btn.dataset.id)));
      });
    }

    // Рендеринг курсов
    function renderCoursesList() {
      const tbody = document.getElementById('courses-table');
      tbody.innerHTML = '';
      if (courses.length === 0) {
        tbody.innerHTML = '<tr><td colspan="4" class="text-gray-600">Курсы отсутствуют.</td></tr>';
      } else {
        courses.forEach(course => {
          const tr = document.createElement('tr');
          tr.innerHTML = `
            <td>${course.name}</td>
            <td>${course.duration}</td>
            <td>${course.assignee || 'Не назначено'}</td>
            <td>
              <button class="delete-course-btn p-2 bg-red-500 text-white rounded-xl hover:bg-red-600 apple-btn" data-id="${course.id}">Удалить</button>
            </td>
          `;
          tbody.appendChild(tr);
        });
      }

      document.querySelectorAll('.delete-course-btn').forEach(btn => {
        btn.addEventListener('click', () => deleteCourse(parseInt(btn.dataset.id)));
      });
    }

    // Рендеринг оценок
    function renderEvaluationsList() {
      const tbody = document.getElementById('evaluations-table');
      tbody.innerHTML = '';
      if (evaluations.length === 0) {
        tbody.innerHTML = '<tr><td colspan="4" class="text-gray-600">Оценки отсутствуют.</td></tr>';
      } else {
        evaluations.forEach(eval => {
          const tr = document.createElement('tr');
          tr.innerHTML = `
            <td>${eval.employee}</td>
            <td>${eval.date}</td>
            <td>${eval.rating}</td>
            <td>${eval.comment || '-'}</td>
          `;
          tbody.appendChild(tr);
        });
      }
    }

    // Рендеринг отдела
    function renderDepartmentList() {
      const list = document.getElementById('department-list');
      list.innerHTML = '';
      if (employees.length === 0) {
        list.innerHTML = '<li class="text-gray-600">Сотрудники отсутствуют.</li>';
      } else {
        employees.forEach(emp => {
          const li = document.createElement('li');
          li.textContent = `${emp.name} (${emp.department}, ${emp.position})`;
          list.appendChild(li);
        });
      }
    }

    // Рендеринг профиля сотрудника
    function renderEmployeeProfile() {
      const employee = employees.find(emp => emp.name === "Иванов Иван") || { skills: [], courses: [] };
      document.getElementById('skills-list').innerHTML = employee.skills.length ? employee.skills.map(skill => `<li>${skill}</li>`).join('') : '<li class="text-gray-600">Навыки отсутствуют.</li>';
      const assignedCourses = courses.filter(course => employee.courses.includes(course.id));
      document.getElementById('assigned-courses-list').innerHTML = assignedCourses.length ? assignedCourses.map(course => `<li>${course.name} (${course.duration})</li>`).join('') : '<li class="text-gray-600">Курсы отсутствуют.</li>';
    }

    // Модальные окна
    function openAddEmployeeModal() {
      currentEmployeeId = null;
      document.getElementById('modal-title').textContent = 'Добавить сотрудника';
      document.getElementById('modal-employee-name').value = '';
      document.getElementById('modal-employee-department').value = '';
      document.getElementById('modal-employee-position').value = '';
      document.getElementById('modal-employee-email').value = '';
      document.getElementById('modal-employee-hire-date').value = '';
      document.getElementById('employee-modal').classList.remove('hidden');
    }

    function openEditEmployeeModal(id) {
      currentEmployeeId = id;
      const employee = employees.find(emp => emp.id === id);
      if (!employee) return;
      document.getElementById('modal-title').textContent = 'Редактировать сотрудника';
      document.getElementById('modal-employee-name').value = employee.name;
      document.getElementById('modal-employee-department').value = employee.department;
      document.getElementById('modal-employee-position').value = employee.position;
      document.getElementById('modal-employee-email').value = employee.email;
      document.getElementById('modal-employee-hire-date').value = employee.hireDate;
      document.getElementById('employee-modal').classList.remove('hidden');
    }

    function openAddCourseModal() {
      document.getElementById('modal-course-name').value = '';
      document.getElementById('modal-course-duration').value = '';
      const select = document.getElementById('modal-course-assignee');
      select.innerHTML = '<option value="">Выберите сотрудника</option>' + employees.map(emp => `<option value="${emp.name}">${emp.name}</option>`).join('');
      document.getElementById('course-modal').classList.remove('hidden');
    }

    function openAddEvaluationModal() {
      const select = document.getElementById('modal-evaluation-employee');
      select.innerHTML = '<option value="">Выберите сотрудника</option>' + employees.map(emp => `<option value="${emp.name}">${emp.name}</option>`).join('');
      document.getElementById('modal-evaluation-date').value = '';
      document.getElementById('modal-evaluation-rating').value = '';
      document.getElementById('modal-evaluation-comment').value = '';
      document.getElementById('evaluation-modal').classList.remove('hidden');
    }

    // Удаление
    function deleteEmployee(id) {
      Swal.fire({
        title: 'Вы уверены?',
        text: 'Сотрудник будет удалён!',
        icon: 'warning',
        showCancelButton: true,
        confirmButtonText: 'Удалить',
        cancelButtonText: 'Отмена'
      }).then(result => {
        if (result.isConfirmed) {
          employees = employees.filter(emp => emp.id !== id);
          courses.forEach(course => {
            if (course.assignee === employees.find(emp => emp.id === id)?.name) course.assignee = '';
          });
          evaluations = evaluations.filter(eval => eval.employee !== employees.find(emp => emp.id === id)?.name);
          saveData();
          renderEmployeeList(
            document.getElementById('filter-name').value,
            document.getElementById('filter-department').value
          );
          Swal.fire('Удалено!', 'Сотрудник успешно удалён.', 'success');
        }
      });
    }

    function deleteCourse(id) {
      Swal.fire({
        title: 'Вы уверены?',
        text: 'Курс будет удалён!',
        icon: 'warning',
        showCancelButton: true,
        confirmButtonText: 'Удалить',
        cancelButtonText: 'Отмена'
      }).then(result => {
        if (result.isConfirmed) {
          courses = courses.filter(course => course.id !== id);
          employees.forEach(emp => {
            emp.courses = emp.courses.filter(cid => cid !== id);
          });
          saveData();
          renderCoursesList();
          Swal.fire('Удалено!', 'Курс успешно удалён.', 'success');
        }
      });
    }

    // Генерация отчёта
    function generateReport() {
      const report = employees.map(emp => ({
        name: emp.name,
        department: emp.department,
        skills: emp.skills.join(', '),
        courses: emp.courses.map(cid => courses.find(c => c.id === cid)?.name || 'Не найдено').join(', ')
      }));
      const output = document.getElementById('report-output');
      output.innerHTML = report.length ? report.map(r => `
        <div class="mb-4">
          <p><strong>Имя:</strong> ${r.name}</p>
          <p><strong>Отдел:</strong> ${r.department}</p>
          <p><strong>Навыки:</strong> ${r.skills || 'Нет'}</p>
          <p><strong>Курсы:</strong> ${r.courses || 'Нет'}</p>
        </div>
      `).join('') : '<p class="text-gray-600">Данные отсутствуют.</p>';
    }

    // Обработчики событий
    document.getElementById('login-btn').addEventListener('click', () => {
      const role = document.getElementById('user-role').value;
      document.getElementById('login-panel').classList.add('hidden');
      document.getElementById('sidebar').classList.remove('hidden');
      document.getElementById('dashboard').classList.remove('hidden');
      document.getElementById(`${role}-nav`).classList.remove('hidden');
      document.getElementById(`${role}-section`).classList.remove('hidden');
      if (role === 'admin') renderEmployeeList();
      if (role === 'manager') renderDepartmentList();
      if (role === 'employee') renderEmployeeProfile();
    });

    document.getElementById('logout-btn').addEventListener('click', () => {
      document.getElementById('login-panel').classList.remove('hidden');
      document.getElementById('sidebar').classList.add('hidden');
      document.getElementById('dashboard').classList.add('hidden');
      document.querySelectorAll('[id$="-section"]').forEach(section => section.classList.add('hidden'));
      document.querySelectorAll('[id$="-nav"]').forEach(nav => nav.classList.add('hidden'));
    });

    document.getElementById('open-add-employee-modal').addEventListener('click', openAddEmployeeModal);
    document.getElementById('open-add-course-modal').addEventListener('click', openAddCourseModal);
    document.getElementById('open-add-evaluation-modal').addEventListener('click', openAddEvaluationModal);

    document.getElementById('modal-cancel-btn').addEventListener('click', () => {
      document.getElementById('employee-modal').classList.add('hidden');
    });

    document.getElementById('modal-save-btn').addEventListener('click', () => {
      const name = document.getElementById('modal-employee-name').value.trim();
      const department = document.getElementById('modal-employee-department').value.trim();
      const position = document.getElementById('modal-employee-position').value.trim();
      const email = document.getElementById('modal-employee-email').value.trim();
      const hireDate = document.getElementById('modal-employee-hire-date').value;
      if (name && department && position && email && hireDate) {
        if (currentEmployeeId) {
          const employee = employees.find(emp => emp.id === currentEmployeeId);
          employee.name = name;
          employee.department = department;
          employee.position = position;
          employee.email = email;
          employee.hireDate = hireDate;
          Swal.fire('Успех!', 'Сотрудник обновлён.', 'success');
        } else {
          employees.push({ id: employees.length + 1, name, department, position, email, hireDate, skills: [], courses: [] });
          Swal.fire('Успех!', 'Сотрудник добавлен.', 'success');
        }
        saveData();
        renderEmployeeList(
          document.getElementById('filter-name').value,
          document.getElementById('filter-department').value
        );
        document.getElementById('employee-modal').classList.add('hidden');
      } else {
        Swal.fire('Ошибка!', 'Заполните все поля.', 'error');
      }
    });

    document.getElementById('modal-course-cancel-btn').addEventListener('click', () => {
      document.getElementById('course-modal').classList.add('hidden');
    });

    document.getElementById('modal-course-save-btn').addEventListener('click', () => {
      const name = document.getElementById('modal-course-name').value.trim();
      const duration = document.getElementById('modal-course-duration').value.trim();
      const assignee = document.getElementById('modal-course-assignee').value;
      if (name && duration) {
        const courseId = courses.length + 1;
        courses.push({ id: courseId, name, duration, assignee });
        if (assignee) {
          const employee = employees.find(emp => emp.name === assignee);
          employee.courses.push(courseId);
        }
        saveData();
        renderCoursesList();
        document.getElementById('course-modal').classList.add('hidden');
        Swal.fire('Успех!', 'Курс добавлен.', 'success');
      } else {
        Swal.fire('Ошибка!', 'Заполните название и продолжительность.', 'error');
      }
    });

    document.getElementById('modal-evaluation-cancel-btn').addEventListener('click', () => {
      document.getElementById('evaluation-modal').classList.add('hidden');
    });

    document.getElementById('modal-evaluation-save-btn').addEventListener('click', () => {
      const employee = document.getElementById('modal-evaluation-employee').value;
      const date = document.getElementById('modal-evaluation-date').value;
      const rating = document.getElementById('modal-evaluation-rating').value;
      const comment = document.getElementById('modal-evaluation-comment').value.trim();
      if (employee && date && rating) {
        evaluations.push({ id: evaluations.length + 1, employee, date, rating: parseInt(rating), comment });
        saveData();
        renderEvaluationsList();
        document.getElementById('evaluation-modal').classList.add('hidden');
        Swal.fire('Успех!', 'Оценка добавлена.', 'success');
      } else {
        Swal.fire('Ошибка!', 'Заполните сотрудника, дату и рейтинг.', 'error');
      }
    });

    document.getElementById('filter-name').addEventListener('input', (e) => {
      currentPage = 1;
      renderEmployeeList(e.target.value, document.getElementById('filter-department').value);
    });

    document.getElementById('filter-department').addEventListener('input', (e) => {
      currentPage = 1;
      renderEmployeeList(document.getElementById('filter-name').value, e.target.value);
    });

    document.getElementById('prev-page').addEventListener('click', () => {
      if (currentPage > 1) {
        currentPage--;
        renderEmployeeList(
          document.getElementById('filter-name').value,
          document.getElementById('filter-department').value
        );
      }
    });

    document.getElementById('next-page').addEventListener('click', () => {
      const filteredEmployees = employees.filter(emp =>
        emp.name.toLowerCase().includes(document.getElementById('filter-name').value.toLowerCase()) &&
        emp.department.toLowerCase().includes(document.getElementById('filter-department').value.toLowerCase())
      );
      if (currentPage < Math.ceil(filteredEmployees.length / itemsPerPage)) {
        currentPage++;
        renderEmployeeList(
          document.getElementById('filter-name').value,
          document.getElementById('filter-department').value
        );
      }
    });

    document.getElementById('add-skill-btn').addEventListener('click', () => {
      const skill = document.getElementById('new-skill').value.trim();
      if (skill) {
        const employee = employees.find(emp => emp.name === "Иванов Иван");
        if (employee) {
          employee.skills.push(skill);
          saveData();
          renderEmployeeProfile();
          document.getElementById('new-skill').value = '';
          Swal.fire('Успех!', 'Навык добавлен.', 'success');
        } else {
          Swal.fire('Ошибка!', 'Сотрудник не найден.', 'error');
        }
      } else {
        Swal.fire('Ошибка!', 'Введите навык.', 'error');
      }
    });

    document.getElementById('generate-report-btn').addEventListener('click', generateReport);

    // Навигация
    document.getElementById('dashboard-btn').addEventListener('click', () => {
      document.querySelectorAll('[id$="-section"]').forEach(section => section.classList.add('hidden'));
      const role = document.getElementById('user-role').value;
      document.getElementById(`${role}-section`).classList.remove('hidden');
      if (role === 'admin') renderEmployeeList();
      if (role === 'manager') renderDepartmentList();
      if (role === 'employee') renderEmployeeProfile();
    });

    document.getElementById('employees-btn').addEventListener('click', () => {
      document.querySelectorAll('[id$="-section"]').forEach(section => section.classList.add('hidden'));
      document.getElementById('admin-section').classList.remove('hidden');
      renderEmployeeList();
    });

    document.getElementById('courses-btn').addEventListener('click', () => {
      document.querySelectorAll('[id$="-section"]').forEach(section => section.classList.add('hidden'));
      document.getElementById('courses-section').classList.remove('hidden');
      renderCoursesList();
    });

    document.getElementById('reports-btn').addEventListener('click', () => {
      document.querySelectorAll('[id$="-section"]').forEach(section => section.classList.add('hidden'));
      document.getElementById('reports-section').classList.remove('hidden');
      generateReport();
    });

    document.getElementById('department-btn').addEventListener('click', () => {
      document.querySelectorAll('[id$="-section"]').forEach(section => section.classList.add('hidden'));
      document.getElementById('manager-section').classList.remove('hidden');
      renderDepartmentList();
    });

    document.getElementById('evaluations-btn').addEventListener('click', () => {
      document.querySelectorAll('[id$="-section"]').forEach(section => section.classList.add('hidden'));
      document.getElementById('evaluations-section').classList.remove('hidden');
      renderEvaluationsList();
    });

    document.getElementById('profile-btn').addEventListener('click', () => {
      document.querySelectorAll('[id$="-section"]').forEach(section => section.classList.add('hidden'));
      document.getElementById('employee-section').classList.remove('hidden');
      renderEmployeeProfile();
    });
  </script>
</body>
</html>
