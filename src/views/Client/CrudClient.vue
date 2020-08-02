<template>
    <div class="client">
        <div class="container">
            <div class="row mt-3">
                <div class="col-lg-6 col-md-6">
                    <h3 class="text-info">Registrar Cliente</h3>
                </div>
                <div class="col-lg-6 col-md-6">
                    <button class="btn btn-info float-right" @click="showAgregarModal=true">
                        <i class="fas fa-user"></i>&nbsp;&nbsp;
                        Agregar Nuevo Cliente
                    </button>
                </div>
            </div>

            <hr class="bg-info">

            <div class="alert alert-danger" v-if="errorMsg">
                {{ errorMsg }}
            </div>
            <div class="alert alert-success" v-if="successMsg">
                {{ successMsg }}
            </div>
            
            <div class="row">
                <div class="col-lg-12">
                    <table class="table table-bordered table-striped">
                        <thead>
                            <tr class="text-center bg-info text-light">
                                <th>ID</th>
                                <th>Nombre</th>
                                <th>e-mail</th>
                                <th>Telefono</th>
                                <th>Editar</th>
                                <th>Borrar</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr class="text-center" v-for="cliente in clientes" v-bind:key="cliente.id">
                                <td>{{ cliente.id }}</td>
                                <td>{{ cliente.nombre }}</td>
                                <td>{{ cliente.email }}</td>
                                <td>{{ cliente.telefono }}</td>
                                <td>
                                    <a href="#" class="text-success" @click="showEditarModal=true; selectClient(cliente);">
                                        <i class="fas fa-edit"></i>
                                    </a>
                                </td>
                                <td>
                                    <a href="#" class="text-danger" @click="showBorrarModal=true; selectClient(cliente);">
                                        <i class="fas fa-trash-alt"></i>
                                    </a>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- Modal Nuevo Cliente -->
        <div id="overlay" v-if="showAgregarModal">
            <div class="modal-dialog">
                <div class="modal-content">

                    <div class="modal-header">
                        <h5 class="modal-title">Agregar Nuevo Cliente</h5>
                        <button type="button" class="close" @click="showAgregarModal=false">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>

                    <div class="modal-body p-4">
                        <form action="#" method="POST">
                            <div class="form-group">
                                <input type="text" class="form-control form-control-lg" placeholder="Nombre" v-model="nuevoCliente.nombre">
                            </div>
                            <div class="form-group">
                                <input type="email" class="form-control form-control-lg" placeholder="e-mail" v-model="nuevoCliente.email">
                            </div>
                            <div class="form-group">
                                <input type="tel" class="form-control form-control-lg" placeholder="Telefono" v-model="nuevoCliente.telefono">
                            </div>
                            <div class="form-group">
                                <button class="btn btn-info btn-block btn-lg"  @click="showAgregarModal=false; addClient();">
                                    Agregar Cliente
                                </button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>

        <!-- Modal Editar Cliente -->
        <div id="overlay" v-if="showEditarModal">
            <div class="modal-dialog">
                <div class="modal-content">

                    <div class="modal-header">
                        <h5 class="modal-title">Editar Cliente</h5>
                        <button type="button" class="close" @click="showEditarModal=false">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>

                    <div class="modal-body p-4">
                        <form action="#" method="POST">
                            <div class="form-group">
                                <input type="text" class="form-control form-control-lg" v-model="clienteActual.nombre">
                            </div>
                            <div class="form-group">
                                <input type="email" class="form-control form-control-lg" placeholder="e-mail" v-model="clienteActual.email">
                            </div>
                            <div class="form-group">
                                <input type="tel" class="form-control form-control-lg" placeholder="Telefono" v-model="clienteActual.telefono">
                            </div>
                            <div class="form-group">
        
                                <button class="btn btn-info btn-block btn-lg"  @click="showEditarModal=false; updateClient();">
                                    Editar Cliente
                                </button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>

        <!-- Modal Eliminar Cliente -->
        <div id="overlay" v-if="showBorrarModal">
            <div class="modal-dialog">
                <div class="modal-content">

                    <div class="modal-header">
                        <h5 class="modal-title">Eliminar Cliente</h5>
                        <button type="button" class="close" @click="showBorrarModal=false">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>

                    <div class="modal-body p-4">
                        <h4 class="text-danger">¿Estas seguro de que quieres eliminar este cliente?</h4>
                        <h5>Tu estas eliminando a '{{ clienteActual.nombre }}'</h5>
                        <hr>
                        <button class="btn btn-danger btn-lg" @click="showBorrarModal=false; deleteClient();">Sí</button>
                        &nbsp;&nbsp;&nbsp;&nbsp;
                        <button class="btn btn-success btn-lg" @click="showBorrarModal=false">No</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    export default {
        name: 'CrudClient',
        data() {
            return {
                errorMsg: "",
                successMsg: "",
                showAgregarModal: false,
                showEditarModal: false,
                showBorrarModal: false,
                clientes: [],
                nuevoCliente: {nombre:"", email:"", telefono:""},
                clienteActual: {}
            }
        },
        mounted: function () {
            this.getAllClients();
        },
        methods: {
            getAllClients() {
                this.axios.get("http://api-crud-users-laravel7.test/api/clients")
                .then((res)=>{
                    if (res.data.error) {
                        this.errorMsg = res.data.message;
                    } else {
                        this.clientes = res.data;
                    }
                })
            },
            addClient() {
                this.clearMsg();
                var formData = this.toFormData(this.nuevoCliente);
                this.axios.post("http://api-crud-users-laravel7.test/api/clients", formData)
                .then((res)=>{
                    this.nuevoCliente = {nombre:"", email:"", telefono:""};
                    if (res.data.error) {
                        this.errorMsg = res.data.message;
                    } else {
                        this.successMsg = res.data.message;
                        this.getAllClients();
                    }
                });
            },
            updateClient() {
                this.clearMsg();

                this.axios.put("http://api-crud-users-laravel7.test/api/clients/"+this.clienteActual.id, this.clienteActual)
                .then((res)=>{
                    this.clienteActual = {};
                    if (res.data.error) {
                        this.errorMsg = res.data.message;
                    } else {
                        this.successMsg = res.data.message;
                        this.getAllClients();
                    }
                });
            },
            deleteClient() {
                this.clearMsg();
                
                this.axios.delete("http://api-crud-users-laravel7.test/api/clients/"+this.clienteActual.id)
                .then((res)=>{
                    this.clienteActual = {};
                    if (res.data.error) {
                        this.errorMsg = res.data.message;
                    } else {
                        this.successMsg = res.data.message;
                        this.getAllClients();
                    }
                });
            },
            toFormData(obj) {
                var fd = new FormData();
                for(var i in obj) {
                    fd.append(i, obj[i]);
                }
                return fd;
            },
            selectClient(client) {
                this.clienteActual = client;
            },
            clearMsg() {
                this.errorMsg = "";
                this.successMsg = "";
            }
        }
    }
</script>