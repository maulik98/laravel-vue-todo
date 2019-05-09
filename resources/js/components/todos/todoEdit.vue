<template>
    <div>
        <div class="form-group">
            <router-link to="/">
                <button class="btn btn-info">Back</button>
            </router-link>
        </div>

        <div class="panel panel-default">
            <div class="panel-heading">Create new Todo</div>
            <div class="panel-body">
                <form v-on:submit.prevent="saveForm()">
                    <!--<div class="row">-->
                    <div class="col-xs-12 form-group">
                        <label class="control-label">Todo task</label>
                        <input :class="{ error: errors.has('task') }" type="text" v-validate="'required'" name="task"
                               v-model="todo.task" class="form-control">
                        <span v-show="errors.has('task')" class="error">{{ errors.first('task') }}</span>

                    </div>
                    <!--</div>-->
                    <!--<div class="row">-->
                    <div class="col-xs-12 form-group">
                        <button class="btn btn-success">Update</button>
                    </div>
                    <!--</div>-->
                </form>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "todoEdit",
        mounted() {
            let app = this;
            let id = app.$route.params.id;
            app.todoId = id;
            axios.get('api/v1/todos/' + id)
                .then(function (resp) {
                    app.todo = resp.data;
                })
        },
        data: function () {
            return {
                todoId: null,
                todo: {
                    task: '',
                    csrf: document.querySelector('meta[name="csrf-token"]').getAttribute('content')
                },
                success: false,
            }
        },
        methods: {
            saveForm() {
                var app = this;
                var newTodo = app.todo;
                this.$validator.validateAll().then(isValid => {
                    if (isValid) {
                        axios.put('api/v1/todos/' + app.todoId, newTodo).then((resp) => {
                            app.$router.replace('/');
                        })
                    }
                })
                    .catch((error) => {
                        // this.errors = error.response.data.errors;
                        alert('Could not create Todo');
                    })
            }
        }
    }
</script>

<style scoped>

</style>