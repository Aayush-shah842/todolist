<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>To-Do List App</title>
<style>
    body {
        font-family: Arial, sans-serif;
    }
    #taskInput {
        width: 70%;
        padding: 10px;
        margin-bottom: 10px;
    }
    #addBtn {
        padding: 10px 20px;
        background-color: #4CAF50;
        color: white;
        border: none;
        cursor: pointer;
    }
    .taskItem {
        margin-bottom: 5px;
    }
    .deleteBtn {
        background-color: #f44336;
        color: white;
        border: none;
        padding: 5px 10px;
        cursor: pointer;
        margin-left: 5px;
    }
</style>
</head>
<body>
    <h2>To-Do List</h2>
    <input type="text" id="taskInput" placeholder="Enter a task">
    <button id="addBtn">Add Task</button>
    <div id="taskList"></div>

    <script>
        // Function to add a task
        function addTask() {
            var taskInput = document.getElementById("taskInput");
            var taskText = taskInput.value.trim();
            if (taskText !== "") {
                var taskList = document.getElementById("taskList");
                var taskItem = document.createElement("div");
                taskItem.className = "taskItem";
                taskItem.innerHTML = taskText + ' <button class="deleteBtn" onclick="deleteTask(this)">Delete</button>';
                taskList.appendChild(taskItem);
                saveTasksToLocalStorage(taskText);
                taskInput.value = "";
            } else {
                alert("Please enter a task!");
            }
        }

        // Function to delete a task
        function deleteTask(btn) {
            var taskText = btn.parentNode.textContent.trim();
            var taskList = document.getElementById("taskList");
            taskList.removeChild(btn.parentNode);
            removeTaskFromLocalStorage(taskText);
        }

        // Function to save tasks to local storage
        function saveTasksToLocalStorage(task) {
            var tasks = JSON.parse(localStorage.getItem("tasks")) || [];
            tasks.push(task);
            localStorage.setItem("tasks", JSON.stringify(tasks));
        }

        // Function to remove task from local storage
        function removeTaskFromLocalStorage(task) {
            var tasks = JSON.parse(localStorage.getItem("tasks")) || [];
            var index = tasks.indexOf(task);
            if (index !== -1) {
                tasks.splice(index, 1);
                localStorage.setItem("tasks", JSON.stringify(tasks));
            }
        }

        // Function to load tasks from local storage
        function loadTasksFromLocalStorage() {
            var tasks = JSON.parse(localStorage.getItem("tasks")) || [];
            var taskList = document.getElementById("taskList");
            tasks.forEach(function(task) {
                var taskItem = document.createElement("div");
                taskItem.className = "taskItem";
                taskItem.innerHTML = task + ' <button class="deleteBtn" onclick="deleteTask(this)">Delete</button>';
                taskList.appendChild(taskItem);
            });
        }

        // Load tasks when the page is loaded
        window.onload = loadTasksFromLocalStorage;

        // Add event listener for the add task button
        document.getElementById("addBtn").addEventListener("click", addTask);
    </script>
</body>
</html>
