<template>
    <div>
        <h2>Custom Form</h2>
        <form @submit.prevent="handleSubmit">
            <div class="form-group">
                <label for="title">Title</label>
                <input type="text" v-model="title" name="title" class="form-control" :class="{ 'is-invalid': submitted && !title }" />
                <div v-show="submitted && !title" class="invalid-feedback">title is required</div>
            </div>

            <div class="form-group row" v-for="(input, index) in inputs" :key="index">
                <div class="col-md-8">
                    <label for="title">Field Name</label>
                    <input type="text" v-model="input.name" class="form-control" :class="{ 'is-invalid': submitted && !input.name }" />
                    <div v-show="submitted && !input.name" class="invalid-feedback">field name is required</div>
                </div>

                <div class="col-md-3">
                    <label>Select Type</label>
                    <select class="form-control" v-model="input.type">
                        <option value="text">Text</option>
                        <option value="date">Date Picker</option>
                        <option value="number">Number</option>
                    </select>
                </div>
                <div class="col-md-1">
                    <label>&nbsp;</label>
                    <button class="btn btn-sm" @click.prevent="remove(index)" v-show="index || ( !index && inputs.length > 1)">x</button>
                </div>
            </div>

            <div class="form-group row">
                <button class="offset-sm-11 btn btn-success btn-circle m-t" type="button" @click.prevent="add()">
                    +
                </button>
            </div>

            <div class="form-group">
                <button class="btn btn-primary" :disabled="status.formCreating">Submit</button>
                <img v-show="status.formCreating" src="data:image/gif;base64,R0lGODlhEAAQAPIAAP///wAAAMLCwkJCQgAAAGJiYoKCgpKSkiH/C05FVFNDQVBFMi4wAwEAAAAh/hpDcmVhdGVkIHdpdGggYWpheGxvYWQuaW5mbwAh+QQJCgAAACwAAAAAEAAQAAADMwi63P4wyklrE2MIOggZnAdOmGYJRbExwroUmcG2LmDEwnHQLVsYOd2mBzkYDAdKa+dIAAAh+QQJCgAAACwAAAAAEAAQAAADNAi63P5OjCEgG4QMu7DmikRxQlFUYDEZIGBMRVsaqHwctXXf7WEYB4Ag1xjihkMZsiUkKhIAIfkECQoAAAAsAAAAABAAEAAAAzYIujIjK8pByJDMlFYvBoVjHA70GU7xSUJhmKtwHPAKzLO9HMaoKwJZ7Rf8AYPDDzKpZBqfvwQAIfkECQoAAAAsAAAAABAAEAAAAzMIumIlK8oyhpHsnFZfhYumCYUhDAQxRIdhHBGqRoKw0R8DYlJd8z0fMDgsGo/IpHI5TAAAIfkECQoAAAAsAAAAABAAEAAAAzIIunInK0rnZBTwGPNMgQwmdsNgXGJUlIWEuR5oWUIpz8pAEAMe6TwfwyYsGo/IpFKSAAAh+QQJCgAAACwAAAAAEAAQAAADMwi6IMKQORfjdOe82p4wGccc4CEuQradylesojEMBgsUc2G7sDX3lQGBMLAJibufbSlKAAAh+QQJCgAAACwAAAAAEAAQAAADMgi63P7wCRHZnFVdmgHu2nFwlWCI3WGc3TSWhUFGxTAUkGCbtgENBMJAEJsxgMLWzpEAACH5BAkKAAAALAAAAAAQABAAAAMyCLrc/jDKSatlQtScKdceCAjDII7HcQ4EMTCpyrCuUBjCYRgHVtqlAiB1YhiCnlsRkAAAOwAAAAAAAAAAAA==" />
            </div>
            
        </form>
    </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'

export default {
    data () {
        return {
            title: '',
            inputs: [{
                name: '',
                type: 'text'
            }],
            submitted: false
        }
    },

    mounted() {
        console.log("Status : ", status.formCreating)
    },

    computed: {
        ...mapState('account', ['status'])
    },
    
    methods: {
       ...mapActions('account', ['createForm']),

        add () {
            this.inputs.push({
                name: '',
                type: 'text'
            })
            console.log(this.inputs)
        },

        remove (index) {
            console.log("Index: ", index);
            this.inputs.splice(index, 1)
        },

        handleSubmit (e) {
            this.submitted = true;
            const { title, inputs } = this;
            this.$validator.validate().then(valid => {
                if (valid) {
                    this.createForm({title, inputs});
                }
            });
        }
    }
};
</script>