<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do App</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: linear-gradient(135deg, #4c63d2 0%, #2d3748 100%); min-height: 100vh; color: white; }
        .header { background: linear-gradient(135deg, #4c63d2 0%, #667eea 100%); padding: 20px; text-align: center; display: flex; align-items: center; justify-content: center; gap: 15px; position: relative; }
        .logo { width: 60px; height: 60px; background: white; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 24px; color: #4c63d2; font-weight: bold; }
        .header-text h1 { font-size: 2.5em; margin-bottom: 5px; font-weight: bold; }
        .header-text p { font-size: 1.2em; opacity: 0.9; }
        .sun-icon { position: absolute; right: 30px; top: 50%; transform: translateY(-50%); font-size: 30px; cursor: pointer; transition: transform 0.3s; }
        .sun-icon:hover { transform: translateY(-50%) rotate(90deg); }
        .main-container { display: flex; min-height: calc(100vh - 140px); }
        .sidebar { width: 300px; background: rgba(255, 255, 255, 0.1); padding: 30px 20px; backdrop-filter: blur(10px); border-right: 1px solid rgba(255, 255, 255, 0.2); }
        .sidebar h2 { font-size: 1.5em; margin-bottom: 20px; color: white; }
        .list-item { background: rgba(255, 255, 255, 0.1); padding: 15px 20px; margin-bottom: 10px; border-radius: 10px; display: flex; justify-content: space-between; align-items: center; cursor: pointer; transition: all 0.3s; border: 2px solid transparent; }
        .list-item:hover { background: rgba(255, 255, 255, 0.2); transform: translateX(5px); }
        .list-item.active { background: rgba(255, 255, 255, 0.9); color: #4c63d2; border-color: white; box-shadow: 0 5px 15px rgba(255, 255, 255, 0.3); }
        .list-name { font-size: 1.1em; font-weight: 500; }
        .delete-list { background: rgba(255, 255, 255, 0.2); border: none; color: white; padding: 8px 10px; border-radius: 5px; cursor: pointer; font-size: 16px; transition: all 0.3s; }
        .delete-list:hover { background: rgba(255, 0, 0, 0.5); transform: scale(1.1); }
        .list-item.active .delete-list { color: #4c63d2; }
        .add-list-section { margin-top: 30px; padding-top: 20px; border-top: 1px solid rgba(255, 255, 255, 0.2); }
        .add-list-input { width: 100%; padding: 12px; border: none; border-radius: 8px; background: rgba(255, 255, 255, 0.9); color: #333; font-size: 16px; margin-bottom: 10px; outline: none; }
        .add-list-input:focus { box-shadow: 0 0 0 3px rgba(255, 255, 255, 0.3); }
        .add-list-btn { background: linear-gradient(135deg, #4c63d2, #667eea); color: white; border: none; padding: 12px 20px; border-radius: 8px; cursor: pointer; font-size: 16px; width: 100%; font-weight: bold; transition: all 0.3s; }
        .add-list-btn:hover { transform: translateY(-2px); box-shadow: 0 5px 15px rgba(76, 99, 210, 0.3); }
        .content { flex: 1; padding: 30px; background: rgba(0, 0, 0, 0.3); backdrop-filter: blur(10px); }
        .current-list-header { font-size: 2.5em; margin-bottom: 30px; color: white; text-transform: lowercase; font-weight: 300; }
        .stats { display: flex; gap: 20px; margin-bottom: 30px; }
        .stat-card { padding: 20px 30px; border-radius: 15px; display: flex; align-items: center; gap: 15px; font-size: 1.2em; font-weight: bold; backdrop-filter: blur(10px); border: 1px solid rgba(255, 255, 255, 0.2); }
        .pending-card { background: linear-gradient(135deg, #667eea, #764ba2); box-shadow: 0 5px 15px rgba(102, 126, 234, 0.3); }
        .completed-card { background: linear-gradient(135deg, #4CAF50, #45a049); box-shadow: 0 5px 15px rgba(76, 175, 80, 0.3); }
        .task-input-section { display: flex; gap: 15px; margin-bottom: 30px; align-items: center; }
        .task-input { flex: 1; padding: 15px; border: none; border-radius: 10px; background: rgba(255, 255, 255, 0.9); color: #333; font-size: 16px; outline: none; transition: all 0.3s; }
        .task-input:focus { box-shadow: 0 0 0 3px rgba(255, 255, 255, 0.3); transform: translateY(-2px); }
        .datetime-input { padding: 15px; border: none; border-radius: 10px; background: rgba(255, 255, 255, 0.9); color: #333; font-size: 16px; width: 200px; outline: none; }
        .add-task-btn { background: linear-gradient(135deg, #8b5cf6, #a855f7); color: white; border: none; padding: 15px 30px; border-radius: 10px; cursor: pointer; font-size: 16px; font-weight: bold; transition: all 0.3s; }
        .add-task-btn:hover { transform: translateY(-2px); box-shadow: 0 5px 15px rgba(139, 92, 246, 0.4); }
        .tasks-container { max-height: 400px; overflow-y: auto; padding-right: 10px; }
        .tasks-container::-webkit-scrollbar { width: 8px; }
        .tasks-container::-webkit-scrollbar-track { background: rgba(255, 255, 255, 0.1); border-radius: 10px; }
        .tasks-container::-webkit-scrollbar-thumb { background: rgba(255, 255, 255, 0.3); border-radius: 10px; }
        .task-item { background: rgba(255, 255, 255, 0.1); padding: 20px; margin-bottom: 15px; border-radius: 12px; display: flex; justify-content: space-between; align-items: center; backdrop-filter: blur(10px); border: 1px solid rgba(255, 255, 255, 0.2); transition: all 0.3s; }
        .task-item:hover { background: rgba(255, 255, 255, 0.15); transform: translateX(5px); box-shadow: 0 5px 15px rgba(255, 255, 255, 0.1); }
        .task-item.completed { opacity: 0.6; background: rgba(255, 255, 255, 0.05); }
        .task-item.pinned { border-left: 4px solid #ff6b6b; background: rgba(255, 107, 107, 0.1); }
        .task-left { display: flex; align-items: center; gap: 15px; flex: 1; }
        .task-checkbox { width: 24px; height: 24px; border: 2px solid rgba(255, 255, 255, 0.7); border-radius: 6px; cursor: pointer; display: flex; align-items: center; justify-content: center; background: transparent; transition: all 0.3s; }
        .task-checkbox:hover { border-color: #4CAF50; background: rgba(76, 175, 80, 0.1); }
        .task-checkbox.checked { background: linear-gradient(135deg, #4CAF50, #45a049); border-color: #4CAF50; color: white; }
        .task-title { font-size: 1.2em; color: white; font-weight: 500; }
        .task-title.completed { text-decoration: line-through; opacity: 0.7; }
        .task-right { display: flex; align-items: center; gap: 15px; }
        .task-datetime { color: #a0a0ff; font-size: 0.9em; background: rgba(160, 160, 255, 0.1); padding: 5px 10px; border-radius: 20px; border: 1px solid rgba(160, 160, 255, 0.3); }
        .task-actions { display: flex; gap: 8px; }
        .task-action { background: rgba(255, 255, 255, 0.1); border: none; color: white; font-size: 16px; cursor: pointer; padding: 8px 10px; border-radius: 8px; transition: all 0.3s; }
        .task-action:hover { background: rgba(255, 255, 255, 0.2); transform: scale(1.1); }
        .edit-btn:hover { background: rgba(255, 193, 7, 0.3); color: #ffc107; }
        .delete-btn:hover { background: rgba(255, 71, 87, 0.3); color: #ff4757; }
        .pin-btn.pinned { background: rgba(255, 107, 107, 0.3); color: #ff6b6b; }
        .empty-state { text-align: center; padding: 60px 20px; color: rgba(255, 255, 255, 0.7); }
        .empty-state-icon { font-size: 4em; margin-bottom: 20px; opacity: 0.5; }
        .empty-state h3 { font-size: 1.5em; margin-bottom: 10px; font-weight: 300; }
        .modal { display: none; position: fixed; z-index: 1000; left: 0; top: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.7); backdrop-filter: blur(5px); }
        .modal-content { background: linear-gradient(135deg, #4c63d2, #667eea); margin: 10% auto; padding: 30px; border-radius: 20px; width: 90%; max-width: 500px; color: white; box-shadow: 0 20px 40px rgba(0,0,0,0.3); }
        .modal h2 { margin-bottom: 20px; font-size: 1.8em; font-weight: 300; }
        .modal-input { width: 100%; padding: 15px; border: none; border-radius: 10px; background: rgba(255, 255, 255, 0.9); color: #333; font-size: 16px; margin-bottom: 15px; outline: none; }
        .modal-buttons { display: flex; gap: 15px; justify-content: flex-end; margin-top: 20px; }
        .modal-btn { padding: 12px 24px; border: none; border-radius: 10px; cursor: pointer; font-size: 16px; font-weight: bold; transition: all 0.3s; }
        .save-btn { background: linear-gradient(135deg, #4CAF50, #45a049); color: white; }
        .save-btn:hover { transform: translateY(-2px); box-shadow: 0 5px 15px rgba(76, 175, 80, 0.3); }
        .cancel-btn { background: rgba(255, 255, 255, 0.2); color: white; }
        .cancel-btn:hover { background: rgba(255, 255, 255, 0.3); }
        @media (max-width: 768px) {
            .main-container { flex-direction: column; }
            .sidebar { width: 100%; padding: 20px; }
            .task-input-section { flex-direction: column; align-items: stretch; }
            .datetime-input { width: 100%; }
            .stats { flex-direction: column; }
            .task-item { flex-direction: column; align-items: flex-start; gap: 15px; }
            .task-right { align-self: flex-end; }
        }
    </style>
</head>
<body>
    <div class="header">
        <div class="logo">✓</div>
        <div class="header-text">
            <h1>To-Do App</h1>
            <p>Your productivity, reimagined.</p>
        </div>
        <div class="sun-icon">☀️</div>
    </div>

    <div class="main-container">
        <div class="sidebar">
            <h2>Your Lists</h2>
            <div id="listContainer"></div>
            <div class="add-list-section">
                <input type="text" class="add-list-input" id="newListInput" placeholder="New list name">
                <button class="add-list-btn" onclick="addNewList()">Add</button>
            </div>
        </div>

        <div class="content">
            <h1 class="current-list-header" id="currentListTitle"></h1>
            <div class="stats">
                <div class="stat-card pending-card">
                    <span>⏳</span>
                    <span>Tasks Pending: <span id="pendingCount">0</span></span>
                </div>
                <div class="stat-card completed-card">
                    <span>✅</span>
                    <span>Tasks Completed: <span id="completedCount">0</span></span>
                </div>
            </div>
            <div class="task-input-section">
                <input type="text" class="task-input" id="taskInput" placeholder="New task">
                <input type="datetime-local" class="datetime-input" id="taskDateTime">
                <button class="add-task-btn" onclick="addNewTask()">Add</button>
            </div>
            <div class="tasks-container" id="tasksContainer"></div>
        </div>
    </div>

    <div id="editModal" class="modal">
        <div class="modal-content">
            <h2>Edit Task</h2>
            <input type="text" class="modal-input" id="editTaskInput" placeholder="Task title">
            <input type="datetime-local" class="modal-input" id="editTaskDateTime">
            <div class="modal-buttons">
                <button class="modal-btn cancel-btn" onclick="closeEditModal()">Cancel</button>
                <button class="modal-btn save-btn" onclick="saveTaskEdit()">Save</button>
            </div>
        </div>
    </div>

    <script>
        // Global state
        let appState = {
            lists: { general: [] },
            currentList: 'general',
            currentEditId: null
        };

        // Initialize the app
        function initializeApp() {
            // If all lists are deleted, add a default one
            if (Object.keys(appState.lists).length === 0) {
                appState.lists['general'] = [];
                appState.currentList = 'general';
            }
            renderAllLists();
            renderCurrentTasks();
            updateStatistics();
        }

        // Add new list
        function addNewList() {
            const input = document.getElementById('newListInput');
            const listName = input.value.trim().toLowerCase();
            if (!listName) {
                alert('Please enter a list name!');
                return;
            }
            if (appState.lists[listName]) {
                alert('List already exists!');
                return;
            }
            appState.lists[listName] = [];
            appState.currentList = listName;
            input.value = '';
            renderAllLists();
            renderCurrentTasks();
            updateStatistics();
        }

        // Delete list (allows deleting any list)
        function deleteList(listName, event) {
            if (event) event.stopPropagation();
            if (confirm(`Delete "${listName}" list and all its tasks?`)) {
                delete appState.lists[listName];
                const remainingLists = Object.keys(appState.lists);
                appState.currentList = remainingLists.length > 0 ? remainingLists[0] : '';
                renderAllLists();
                renderCurrentTasks();
                updateStatistics();
            }
        }

        // Switch to different list
        function switchToList(listName) {
            appState.currentList = listName;
            renderAllLists();
            renderCurrentTasks();
            updateStatistics();
        }

        // Render all lists in sidebar
        function renderAllLists() {
            const container = document.getElementById('listContainer');
            const lists = Object.keys(appState.lists);
            if (lists.length === 0) {
                container.innerHTML = `<div style="color:#fff;opacity:0.7;text-align:center;margin-top:2em;">No lists. Add one!</div>`;
                document.getElementById('currentListTitle').textContent = '';
                document.getElementById('tasksContainer').innerHTML = '';
                return;
            }
            const listsHTML = lists.map(listName => {
                const isActive = listName === appState.currentList;
                return `
                    <div class="list-item ${isActive ? 'active' : ''}" onclick="switchToList('${listName}')">
                        <span class="list-name">${listName}</span>
                        <button class="delete-list" onclick="deleteList('${listName}', event)">🗑️</button>
                    </div>
                `;
            }).join('');
            container.innerHTML = listsHTML;
            document.getElementById('currentListTitle').textContent = appState.currentList;
        }

        // Add new task
        function addNewTask() {
            const input = document.getElementById('taskInput');
            const datetime = document.getElementById('taskDateTime');
            if (!input.value.trim()) {
                alert('Please enter a task!');
                return;
            }
            if (!appState.currentList) {
                alert('Please create a list first!');
                return;
            }
            const newTask = {
                id: Date.now(),
                title: input.value.trim(),
                completed: false,
                datetime: datetime.value,
                pinned: false
            };
            appState.lists[appState.currentList].push(newTask);
            input.value = '';
            datetime.value = '';
            renderCurrentTasks();
            updateStatistics();
        }

        // Render tasks for current list
        function renderCurrentTasks() {
            const container = document.getElementById('tasksContainer');
            if (!appState.currentList || !appState.lists[appState.currentList]) {
                container.innerHTML = '';
                return;
            }
            const tasks = appState.lists[appState.currentList];
            if (tasks.length === 0) {
                container.innerHTML = `
                    <div class="empty-state">
                        <div class="empty-state-icon">📝</div>
                        <h3>No tasks yet!</h3>
                        <p>Add your first task above to get started.</p>
                    </div>
                `;
                return;
            }
            // Sort tasks: pinned first, then incomplete, then complete
            const sortedTasks = [...tasks].sort((a, b) => {
                if (a.pinned && !b.pinned) return -1;
                if (!a.pinned && b.pinned) return 1;
                if (a.completed && !b.completed) return 1;
                if (!a.completed && b.completed) return -1;
                return 0;
            });
            const tasksHTML = sortedTasks.map(task => {
                const formattedDate = formatTaskDateTime(task.datetime);
                return `
                    <div class="task-item ${task.completed ? 'completed' : ''} ${task.pinned ? 'pinned' : ''}">
                        <div class="task-left">
                            <div class="task-checkbox ${task.completed ? 'checked' : ''}" onclick="toggleTaskCompletion(${task.id})">
                                ${task.completed ? '✓' : ''}
                            </div>
                            <div class="task-title ${task.completed ? 'completed' : ''}">${task.title}</div>
                        </div>
                        <div class="task-right">
                            ${formattedDate ? `<div class="task-datetime">${formattedDate}</div>` : ''}
                            <div class="task-actions">
                                <button class="task-action edit-btn" onclick="openEditModal(${task.id})">✏️</button>
                                <button class="task-action pin-btn ${task.pinned ? 'pinned' : ''}" onclick="toggleTaskPin(${task.id})">📌</button>
                                <button class="task-action delete-btn" onclick="deleteTask(${task.id})">🗑️</button>
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
            container.innerHTML = tasksHTML;
        }

        // Toggle task completion
        function toggleTaskCompletion(taskId) {
            const task = appState.lists[appState.currentList].find(t => t.id === taskId);
            if (task) {
                task.completed = !task.completed;
                renderCurrentTasks();
                updateStatistics();
            }
        }

        // Toggle task pin
        function toggleTaskPin(taskId) {
            const task = appState.lists[appState.currentList].find(t => t.id === taskId);
            if (task) {
                task.pinned = !task.pinned;
                renderCurrentTasks();
            }
        }

        // Delete task
        function deleteTask(taskId) {
            if (confirm('Are you sure you want to delete this task?')) {
                const listTasks = appState.lists[appState.currentList];
                appState.lists[appState.currentList] = listTasks.filter(t => t.id !== taskId);
                renderCurrentTasks();
                updateStatistics();
            }
        }

        // Open edit modal
        function openEditModal(taskId) {
            const task = appState.lists[appState.currentList].find(t => t.id === taskId);
            if (task) {
                appState.currentEditId = taskId;
                document.getElementById('editTaskInput').value = task.title;
                document.getElementById('editTaskDateTime').value = task.datetime;
                document.getElementById('editModal').style.display = 'block';
            }
        }

        // Close edit modal
        function closeEditModal() {
            document.getElementById('editModal').style.display = 'none';
            appState.currentEditId = null;
        }

        // Save task edit
        function saveTaskEdit() {
            const title = document.getElementById('editTaskInput').value.trim();
            const datetime = document.getElementById('editTaskDateTime').value;
            if (!title) {
                alert('Please enter a task title!');
                return;
            }
            const task = appState.lists[appState.currentList].find(t => t.id === appState.currentEditId);
            if (task) {
                task.title = title;
                task.datetime = datetime;
                renderCurrentTasks();
                closeEditModal();
            }
        }

        // Update statistics
        function updateStatistics() {
            if (!appState.currentList || !appState.lists[appState.currentList]) {
                document.getElementById('pendingCount').textContent = 0;
                document.getElementById('completedCount').textContent = 0;
                return;
            }
            const tasks = appState.lists[appState.currentList];
            const pendingTasks = tasks.filter(t => !t.completed).length;
            const completedTasks = tasks.filter(t => t.completed).length;
            document.getElementById('pendingCount').textContent = pendingTasks;
            document.getElementById('completedCount').textContent = completedTasks;
        }

        // Format datetime for display
        function formatTaskDateTime(datetime) {
            if (!datetime) return '';
            const date = new Date(datetime);
            const now = new Date();
            const isToday = date.toDateString() === now.toDateString();
            if (isToday) {
                return `Today, ${date.toLocaleTimeString('en-US', { hour: 'numeric', minute: '2-digit', hour12: true })}`;
            } else {
                return date.toLocaleString('en-US', {
                    month: 'numeric',
                    day: 'numeric',
                    year: 'numeric',
                    hour: 'numeric',
                    minute: '2-digit',
                    hour12: true
                });
            }
        }

        // Event listeners
        document.getElementById('taskInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                addNewTask();
            }
        });

        document.getElementById('newListInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                addNewList();
            }
        });

        // Close modal when clicking outside
        window.addEventListener('click', function(event) {
            const modal = document.getElementById('editModal');
            if (event.target === modal) {
                closeEditModal();
            }
        });

        // Initialize app when page loads
        document.addEventListener('DOMContentLoaded', function() {
            initializeApp();
        });
    </script>
</body>
</html>
