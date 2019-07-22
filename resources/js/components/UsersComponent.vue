<template>
    <div class="container">
 <div class="row mt-4">
          <div class="col-12">
            <div class="card">
              <div class="card-header">
                <h3 class="card-title"> Users </h3>

                <div class="card-tools">
                  <div class="input-group input-group-sm" style="width: 150px;">
                      <button  data-toggle="modal" data-target="#exampleModal" class="btn btn-success"> <i class="fas fa-user-plus"></i> Add User </button>
 
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
                      <th>Control</th>
                     </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <td>183</td>
                      <td>John Doe</td>
                      <td>11-7-2014</td>
                      <td><span class="tag tag-success">
                        <a href="#">
                             <i class="fa fa-edit">  </i> |  
                        </a> 
                        <a href="#"> 
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
         <form @submit.prevent="login" @keydown="form.onKeydown($event)">
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
               <textarea  name="bio" placeholder="bio" class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }"  cols="20" rows="3"></textarea>
            
            <has-error :form="form" field="bio"></has-error>

            </div>
             <div class="form-group">
             <select name="role" class="form-control" :class="{ 'is-invalid': form.errors.has('role') }" >
                 <option value=""> Select User Role </option>
                  <option value="admin"> Admin </option>
                   <option value="user"> User </option>
             </select>
            <has-error :form="form" field="role"></has-error>
            
            </div>
        </form>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-success ">Save </button>
      </div>
    </div>
  </div>
</div>
    </div>
</template>

<script>
    export default {
        data(){
            return {
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
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
