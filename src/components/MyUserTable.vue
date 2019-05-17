<template>
    <div class="hello">

<!--    <Add />-->

        <el-button type="primary" @click="add.visible = true">Add</el-button>

        <el-dialog
                title="Login"
                :visible.sync="add.visible"
                width="30%"
                :before-close="handleCloseAdd">

            <el-form :label-position="labelPositionAdd" label-width="100px" :model="formLabelAlignAdd">
                <el-form-item label="Name">
                    <el-input v-model="formLabelAlignAdd.username"></el-input>
                </el-form-item>
                <el-form-item label="Phone">
                    <el-input v-model="formLabelAlignAdd.phone"></el-input>
                </el-form-item>
            </el-form>

            <span slot="footer" class="dialog-footer">
            <el-button @click="add.visible = false">Cancel</el-button>
            <el-button type="primary" @click="addUser">OK</el-button>
          </span>
        </el-dialog>

<!--    <Table />-->

        <el-table
                :data="tableData.filter(data => !search || data.username.toLowerCase().includes(search.toLowerCase()))"
                style="width: 100%">
            <el-table-column
                    label="Name"
                    prop="username">
            </el-table-column>
            <el-table-column
                    label="Phone"
                    prop="phone">
            </el-table-column>
            <el-table-column
                    align="right">

                <template slot="header" slot-scope="scope">
                    <el-input
                            v-model="search"
                            size="mini"
                            placeholder="Type to search"/>
                </template>

                <template slot-scope="scope">
                    <el-button
                            size="mini"
                            @click="handleEdit(scope.$index)">Edit</el-button>
                    <el-button
                            size="mini"
                            type="danger"
                            @click="handleDelete(scope.$index)">Delete</el-button>
                </template>
            </el-table-column>
        </el-table>

<!--    Edit-->

        <el-dialog
                title="Login"
                :visible.sync="edit.visible"
                width="30%"
                :before-close="handleClose">

            <el-form :label-position="labelPositionEdit" label-width="100px" :formLabelAlignEdit="formLabelAlignEdit">
                <el-form-item label="Name">
                    <el-input v-model="formLabelAlignEdit.username"></el-input>
                </el-form-item>
                <el-form-item label="Phone">
                    <el-input v-model="formLabelAlignEdit.phone"></el-input>
                </el-form-item>
            </el-form>

            <span slot="footer" class="dialog-footer">
                <el-button @click="edit.visible = false">Cancel</el-button>
                <el-button type="primary" @click="editOk">OK</el-button>
            </span>
        </el-dialog>

<!--    Delete-->

        <el-dialog
                title="Login"
                :visible.sync="dlt.visible"
                width="30%"
                :before-close="handleCloseDelete">

            Are you sure you want to delete {{dlt.username}}?

            <span slot="footer" class="dialog-footer">
                    <el-button @click="dlt.visible = false">Cancel</el-button>
                    <el-button type="primary" @click="deleteUser">OK</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script>
    import axios from 'axios';

    export default {
        name: 'MyUserTable',
        props: {
            msg: String
        },
        data() {
            return {
                add: {
                    index: '',
                    username: '',
                    phone: '',
                    address: '',
                    visible: false
                },
                dlt: {
                    index: '',
                    username: '',
                    phone: '',
                    address: '',
                    visible: false
                },
                edit: {
                    index: '',
                    username: '',
                    phone: '',
                    address: '',
                    visible: false
                },
                tableData: [],
                //     [{
                //     username: 'Tom',
                //     phone: '+380951111111',
                //     address: 'No. 189, Grove St, Los Angeles'
                // }, {
                //     username: 'John',
                //     phone: '+380962222222',
                //     address: 'No. 189, Grove St, Los Angeles'
                // }, {
                //     username: 'Morgan',
                //     phone: '+380973333333',
                //     address: 'No. 189, Grove St, Los Angeles'
                // }, {
                //     username: 'Jessy',
                //     phone: '+380984444444',
                //     address: 'No. 189, Grove St, Los Angeles'
                // }],
                search: '',
                labelPositionAdd: 'right',
                formLabelAlignAdd: {
                    username: '',
                    phone: '',
                },
                labelPositionEdit: 'right',
                formLabelAlignEdit: {
                    username: '',
                    phone: '',
                }
            }
        },
        mounted() {
            axios
                .get('http://localhost:8080/userslist')
                .then(response => (this.tableData = response.data));
        },
        // mounted() {
        //     if (localStorage.getItem('tableData')) {
        //         try {
        //             this.tableData = JSON.parse(localStorage.getItem('tableData'));
        //         } catch(e) {
        //             localStorage.removeItem('tableData');
        //         }
        //     }
        // },
        methods: {
            handleCloseAdd(done) {
                this.$confirm('Are you sure to close this dialog?')
                    .then(() => {
                        done();
                    })
                    .catch(() => {});
            },
            handleCloseDelete(done) {
                this.$confirm('Are you sure to close this dialog?')
                    .then(() => {
                        done();
                    })
                    .catch(() => {});
            },

            // Edit
            handleEdit(index) {
                this.edit.index = index;
                this.edit.visible = true;
                this.edit.username = this.tableData[index].username;
                this.edit.phone = this.tableData[index].phone;
                this.formLabelAlignEdit.username = this.edit.username;
                this.formLabelAlignEdit.phone= this.edit.phone;
            },
            editOk() {
                this.edit.visible = false;
                this.tableData[this.edit.index].username = this.formLabelAlignEdit.username;
                this.tableData[this.edit.index].phone= this.formLabelAlignEdit.phone;
                // this.saveUser();
            },

            //Delete
            handleDelete(index) {
                // console.log(index, row);
                this.dlt.index = index;
                this.dlt.visible = true;
                this.dlt.username = this.tableData[index].username;
            },
            deleteUser() {
                this.dlt.visible = false;
                this.tableData.splice(this.dlt.index, 1);
                // this.saveUser();
            },

            addUser() {
                if (!this.formLabelAlignAdd.username && !this.formLabelAlignAdd.username) {
                    return;
                }

                this.tableData.push({
                    username:this.formLabelAlignAdd.username,
                    phone:this.formLabelAlignAdd.phone
                });
                this.formLabelAlignAdd.username = '';
                this.formLabelAlignAdd.phone = '';
                this.saveUser();
            },
            saveUser: function () {
                this.add.visible = false;
                this.edit.visible = false;
                this.delete.visible = false;

                const parsed = JSON.stringify(this.tableData);
                localStorage.setItem('tableData', parsed);
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h3 {
        margin: 40px 0 0;
    }
    ul {
        list-style-type: none;
        padding: 0;
    }
    li {
        display: inline-block;
        margin: 0 10px;
    }
    a {
        color: #42b983;
    }
</style>
