<template>
	<div id="app">
		<h1>Tarefas</h1>
		<TaskProgress :progress="progress" />
		<NewTask @taskAdded="addTask" />
		<TaskGrid :tasks="tasks" 
			@taskDeleted="deleteTask"
			@stateTaskChanged="toggleStateChange" 
		/>
		<footer style="margin-top:100px">Feito por <a href="https://portfolio-jovimoura.vercel.app/" class="github" target="_blank">João Victor Moura</a></footer>
	</div>
</template>

<script>
import TaskProgress from './components/TaskProgress.vue'
import TaskGrid from './components/TaskGrid.vue'
import NewTask from './components/NewTask.vue'

export default {
	components: {
		TaskGrid,
		NewTask,
		TaskProgress
	},
	data() {
		return {
			tasks: []
		}
	},
	watch: {
		tasks: {
			deep: true,
			handler() {
				localStorage.setItem('tasks', JSON.stringify(this.tasks))
			}
		}
	},
	computed: {
		progress() {
			const total = this.tasks.length
			const done = this.tasks.filter(t => !t.pending).length
			return Math.round(done / total*100) || 0
		}
	},
	methods: {
		addTask(task) {
			const sameName = t => t.name === task.name
			const reallyNew = this.tasks.filter(sameName).length == 0
			if(reallyNew) {
				this.tasks.push({
					name: task.name,
					pending: task.pending || true
				})
			}
		},
		deleteTask(i) {
			this.tasks.splice(i,1)
		},
		toggleStateChange(i) {
			this.tasks[i].pending = !this.tasks[i].pending
		}
	},
	created() {
		const json = localStorage.getItem('tasks')
		const array = JSON.parse(json)
		this.tasks = Array.isArray(array) ? array : []
	},
}
</script>

<style>
	body {
		font-family: 'Lato', sans-serif;
		background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
		color: #FFF;
	}

	#app {
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	#app h1 {
		margin-bottom: 5px;
		font-weight: 300;
		font-size: 3rem;
	}

	a {
		text-decoration: none;
		color: #FFF;
		transition: ease-in-out .22s;
	}

	a:hover {
		color: #33a1fd;
	}

	a:active {
		text-decoration: underline;
	}

	@media(max-width:768px) {
        #app {
			height: auto;
			width: 100%;
		}
    }
</style>
