<template>
    <div>
        <div class="form-group">
            <router-link :to="{ name:'todoCreate' }" class="btn btn-success">Create new Todo</router-link>
        </div>
        <div class="panel panel-default">
            <div class="panel-heading">
                List of todos
            </div>
            <div class="panel-body">
                <table class="table table-bordered table-striped">
                    <thead>
                    <tr>
                        <th>Task Name</th>
                        <th>Created at</th>
                        <th>Action</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="todo, index in todos">
                        <td>{{ todo.task }}</td>
                        <td>{{ todo.created_at }}</td>
                        <td>
                            <router-link :to="{name:'todoEdit', params:{id:todo.id}}" class="btn btn-info">Edit
                            </router-link>
                            <a href="#" class="btn btn-danger" v-on:click="deleteTodo(todo.id, index)">Delete</a>
                        </td>
                    </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>
<script>
    export default {

        data: function () {
            return {
                todos: []
            }
        },
        mounted() {
            var app = this;
            axios.get('api/v1/todos').then(function (resp) {
                app.todos = resp.data;
            })
                .catch(function (resp) {
                    alert('Not loaded');
                })
        },
        methods: {
            deleteTodo(id, index) {
                if (confirm('Do you really want to delete?')) {
                    var app = this;
                    axios.delete('api/v1/todos/' + id)
                        .then(function (resp) {
                            app.todos.splice(index, 1);
                        })
                        .catch(function (resp) {
                                alert('Could not delete Todo');
                            }
                        )
                }
            }
        }
    }
</script>