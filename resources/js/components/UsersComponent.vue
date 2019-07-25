<template>
    <div class="container">
 <div class="row mt-4">
          <div class="col-12">
            <div class="card">
              <div class="card-header">
                <h3 class="card-title"> Users </h3>

                <div class="card-tools">
                  <div class="input-group input-group-sm" style="width: 150px;">
                      <button   @click="NewUserModelOpen" class="btn btn-success"> <i class="fas fa-user-plus"></i> Add User </button>
 
                  </div>
                </div>
              </div>
              <!-- /.card-header -->
              <div class="card-body table-responsive p-0">
                <table class="table table-hover">
                  <thead>
                    <tr>
                      <th>ID</th>
                      <th>Name</th>
                      <th>Email</th>
                       <th>Type</th>
                        <th>Created_at </th>
                      <th>Control</th>
                     </tr>
                  </thead>
                  <tbody>
                    <tr v-for="user in users" :key="user.id">
                      <td>{{ user.id}}</td>
                      <td>{{ user.name}}</td>
                      <td>{{ user.email}}</td>
                       <td>{{ user.type | capitalize }}</td>
                       <td>{{ user.created_at | ConvertDate }}</td>
                      <td><span class="tag tag-success">
                        <a href="#" @click="EditUserModelOpen(user)">
                             <i class="fa fa-edit" >  </i> |  
                        </a> 
                        <a href="#" @click="DeleteUser(user.id)"> 
                             <i class="fa fa-trash text-danger">  </i> 
                        </a>
                        </span></td>
                     </tr>
  
                  </tbody>
                </table>
              </div>
              <!-- /.card-body -->
            </div>
            <!-- /.card -->
          </div>
        </div>
        <!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class=" modal-dialog-centered modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Add User</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        

         <form @submit.prevent="CreateUser">
            <div class="form-group">
            <input v-model="form.name" type="text" name="name"
              placeholder="name"  class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
            <has-error :form="form" field="name"></has-error>
            </div>
          <div class="form-group">
            <input v-model="form.email" type="text" name="email"
               placeholder="email" class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
            <has-error :form="form" field="email"></has-error>
            </div>
           <div class="form-group">
               <textarea v-model="form.bio" name="bio" placeholder="bio" class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }"  cols="20" rows="3"></textarea>
            
            <has-error :form="form" field="bio"></has-error>

            </div>
             <div class="form-group">
             <select  v-model="form.type" name="type" class="form-control" :class="{ 'is-invalid': form.errors.has('type') }" >
                 <option value=""> Select User type </option>
                  <option value="admin"> Admin </option>
                   <option value="user"> User </option>
             </select>
            <has-error :form="form" field="role"></has-error>
            
            </div>
          <div class="form-group">
               <input type="password" v-model="form.password" name="password" placeholder="password" class="form-control" :class="{ 'is-invalid': form.errors.has('password') }" >
            <has-error :form="form" field="password"></has-error>

            </div>
                  <div class="modal-footer">
        <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
        <button type="submit" class="btn btn-success ">Save </button>
      </div>
        </form>
      </div>

    </div>
  </div>
</div>
    </div>
</template>

<script>
import { setInterval } from 'timers';
    export default {
        data(){
            return {
                users :{},
                form: new Form({
                    name: '',
                    email: '',
                    password: '',
                    type:'',
                    bio:'',
                    photo:''
                      })  
            }
        }
        ,
        methods:{
          NewUserModelOpen()
          {
            this.form.reset();
             $('#exampleModal').modal('show')
          }
          ,
           EditUserModelOpen(user)
          {
             $('#exampleModal').modal('show')
            this.form.fill(user);
            
          }
          ,
            LoadUsers()
            {
                this.$Progress.start()
                      axios.get('api/users').then(( {data}) => (this.users = data.data));
                  this.$Progress.finish()

            }
            ,
            CreateUser()
            {
                this.$Progress.start()
                 this.form.post('api/users').then(()=> {

                 this.$Progress.finish()
                Toast.fire({
                type: 'success',
                title: 'User created in successfully'
                })
                fire.$emit('AfterCreate');
                $('#exampleModal').modal('hide')

                 }).catch(()=>{
                       this.$Progress.fail()
                 });

            },
            DeleteUser(userid)
            {
                    Swal.fire({
                    title: 'Are you sure?',
                    text: "You won't be able to revert this!",
                    type: 'warning',
                    showCancelButton: true,
                    confirmButtonColor: '#3085d6',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Yes, delete it!'
                  }).then((result) => {
                   
                      this.form.delete('api/users/' + userid).then( () => {
                        if(result.value)
                        {
                  Swal.fire(
                          'Deleted!',
                          'Your file has been deleted.',
                          'success'
                        )
                        fire.$emit('AfterCreate');
                        }
                         
                         
                      }).catch( () => {
                        Swal("failed!", "error" ,"warning");
                      });
                    
                  });
            }
        },
        created() {
            this.LoadUsers();
            fire.$on('AfterCreate' , () => {
                this.LoadUsers() 
            })
           // setInterval(() => this.LoadUsers() , 3000)
        }
    }
</script>
