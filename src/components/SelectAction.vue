<template>
    <section>
        <div class="card mt-5">
            <div class="card-header mt-auto mb-auto">
                <div class="text-center">
                    <span class="h2">Bienvenidos al portal de Autoatencion</span>
                </div>
            </div>
            <div class="card-body">
                <!--Alertas-->
                <div class="row">
                    <div class="col-12">
                        <div v-if="showAlertUnlockSuccess === true" class="alert alert-success">
                            <h5><i class="fa fa-check fa-fw"></i>Bien</h5>
                            <span>Estimado se ha procedido a desbloquear su cuenta de red con exito.</span>
                        </div>
                        <div v-if="showAlertUnlockSuccess === false" class="alert alert-warning">
                            <h5><i class="fa fa-exclamation-triangle fa-fw"></i>Error</h5>
                            <span>Estimado lo sentimos hemos tenido un problema. Para proceder a desbloquear su cuenta de red intentelo nuevamente.</span><br>
                            <h5 v-if="showUnlockOption" class="text-dark small">Si el problema persiste, comunicate al 215-5400 opcion #3.</h5>
                        </div>
                        <div v-if="showAlertResetSuccess === true" class="alert alert-success">
                            <h5><i class="fa fa-check fa-fw"></i>Bien</h5>
                            <span>Estimado su password se reseteó con exito.</span>
                        </div>
                    </div>
                </div>
                <!--Informacion inicial-->
                <div class="row">
                    <div class="col-4">
                        <div class="img-thumbnail text-center">
                            <img src="http://www.moweble.com/console/wp-content/themes/MagMan/timthumb.php?src=http://www.moweble.com/console/wp-content/uploads/2015/08/marry.jpg&w=720&h=&zc=1&q=80"
                                 alt="" width="260">
                        </div>
                    </div>
                    <div class="col-8">
                        <!--Informacion del usuario-->
                        <table class="table">
                            <tr>
                                <th width="30%">Nombres Completos</th>
                                <td width="50%">{{data.name_complet}}</td>
                                <td width="30%"></td>
                                <td width="50%"></td>
                            </tr>
                            <tr>
                                <th>Dni</th>
                                <td>{{data.dni}}</td>
                                <th>Telefono</th>
                                <td>{{data.phone}}</td>
                            </tr>
                            <tr>
                                <th>Cargo</th>
                                <td>{{data.position}}</td>
                                <th>Fecha</th>
                                <td>{{data.date_contract}}</td>
                            </tr>
                            <!--<tr>-->
                                <!--<th>Fecha Contrato</th>-->
                                <!--<td>{{data.date_contract}}</td>-->
                            <!--</tr>-->
                        </table>
                        <!--Acciones del negocio-->
                        <div v-if="showReset === undefined && showResetAccept === undefined && showResetPwd === undefined " class="row">
                            <div class="col-6">
                                <button v-if="showAlertUnlockSuccess === undefined||true && showAlertUnlockSuccess !== false" class="btn btn-outline-success btn-block" @click="unlock()" @dblclick="unlockdbl()">
                                    <i class="fa fa-unlock fa-fw"></i>
                                    <span>Desbloquear Cuenta</span>
                                </button>
                                <button v-if="showAlertUnlockSuccess === false" class="btn btn-outline-dark btn-block" @click="unlock()">
                                    <i class="fa fa-refresh fa-fw"></i>
                                    <span>Intentar Nuevamente</span>
                                </button>
                            </div>
                            <div class="col-6">
                                <button class="btn btn-outline-danger btn-block" @click="reset()">
                                    <i class="fa fa-home fa-fw"></i>
                                    <span>Reset Contraseña</span>
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
                <br v-if="showReset === true || showResetAccept === true || showResetPwd === true">
                <!--Vista pasos para el reseteo-->
                <div class="row" v-if="showReset === true">
                    <div class="col-12">
                        <div class="alert alert-secondary">
                            <h5>Advertencia</h5>
                            <span>Para proceder a resetear su contraseña, como medida de seguridad, procederemos a enviar un sms al numero movil <b>9*******73</b>, que se encuentra en el Active Directory.</span>
                        </div>
                        <div class="row">
                            <div class="col-6">
                                <button class="btn btn-primary btn-block" @click="btnYes()">
                                    <span>Si</span>
                                </button>
                            </div>
                            <div class="col-6">
                                <button class="btn btn-primary btn-block" @click="btnNot()">
                                    <span>No</span>
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
                <!--Vista enviar codigo de seguridad-->
                <div class="row" v-if="showResetAccept === true">
                    <div class="col-12">
                        <div class="alert alert-secondary">
                            <h5>Atencion</h5>
                            <span>Ingrese el codigo de 6 digitos que hemos enviado al numero movil <b>9*******73</b>.</span><a title="click para volver a enviar sms a tu numero movil del Active Directory" class="btn btn-link" href="" @click="reset()"><i class="fa fa-link fa-fw"></i>Reenviar sms</a>
                        </div>
                        <div class="row">
                            <div class="col-8">
                                <div class="form-group">
                                    <input v-model="inputSecurity" type="text" class="form-control" placeholder="Ingrese su codigo de seguridad de 6 digitos" autofocus maxlength="6" />
                                </div>
                            </div>
                            <div class="col-2">
                                <template v-if="inputSecurity.charAt(5) === '' ">
                                    <button disabled class="btn btn-success btn-block">
                                        <i class="fa fa-check fa-fw"></i>
                                        <span>Si</span>
                                    </button>
                                </template>
                                <template v-else>
                                    <button class="btn btn-success btn-block" @click="btnYesSecutity()">
                                        <i class="fa fa-check fa-fw"></i>
                                        <span>Si</span>
                                    </button>
                                </template>
                            </div>
                            <div class="col-2">
                                <button class="btn btn-danger btn-block" @click="btnNotSecutity()">
                                    <i class="fa fa-close fa-fw"></i>
                                    <span>No</span>
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
                <!--Vista reseteo de contraseña-->
                <div class="row" v-if="showResetPwd === true">
                    <div class="col-12">
                        <div class="alert alert-secondary">
                            <p>Para el reseteo, porfavor tome en cuenta lo siguiente:</p>
                            <ul>
                                <li>Debe contener una letra en mayusculas.</li>
                                <li>Debe contener al menos un numero.</li>
                                <li>Ambas contraseñas deben coincidir para habilitar el boton.</li>
                            </ul>
                        </div>
                    </div>
                    <div class="col-5">
                        <label>Nueva Contraseña</label>
                        <div class="form-group">
                            <input v-model="params.password" type="password" class="form-control" @keyup="validateConfirmPwd()"/>
                        </div>
                    </div>
                    <div class="col-5">
                        <label>Confirmar Contraseña</label>
                        <div class="form-group">
                            <input v-model="password_confirm" type="password" class="form-control" @keyup="validateConfirmPwd()"/>
                        </div>
                    </div>
                    <div class="col-2">
                        <label class="text-white">.</label>
                        <template v-if="validateConfirmPwd()">
                            <button :disabled="validateConfirmPwd()" class="btn btn-success btn-block"><i class="fa fa-recycle fa-fw"></i>Reset</button>
                        </template>
                        <template v-else>
                            <button :disabled="validateConfirmPwd()" class="btn btn-success btn-block" @click="resetPwd()"><i class="fa fa-recycle fa-fw"></i>Reset</button>
                        </template>
                    </div>
                </div>
            </div>
            <div class="card-footer">
                <div class="row">
                    <div class="offset-6 col-6">
                        <div class="row">
                            <div class="col-9">
                                <div class="input-group">
                                    <input type="text" class="form-control" placeholder="Dni, Username, Phone" aria-label="Recipient's username" aria-describedby="basic-addon2">
                                    <div class="input-group-append">
                                        <button class="btn btn-primary" type="button">
                                            <i class="fa fa-search fa-fw"></i>
                                            <span>Buscar</span>
                                        </button>
                                    </div>
                                </div>
                            </div>
                            <div class="col-3">
                                <button class="btn btn-primary btn-block" @click="exit()">
                                    <i class="fa fa-sign-out fa-fw"></i>
                                    <span>Salir</span>
                                </button>
                                <!--<router-link class="btn btn-primary btn-block" :to="'/search'">-->
                                    <!--<i class="fa fa-sign-out fa-fw"></i>-->
                                    <!--<span>Salir</span>-->
                                <!--</router-link>-->
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div v-if="showLoadingModal" class="modal" id="exampleModalCenter" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle">
                <div class="modal-dialog modal-sm modal-dialog-centered" role="document">
                    <div class="modal-content">
                        <div class="modal-body">
                            <div class="row">
                                <div class="col-12">
                                    <div class="text-muted text-center">
                                        <i class="fa fa-circle-o-notch fa-spin fa-4x"></i>
                                        <p class="mt-2 mb-0">Procesando Solicitud</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
    </section>
</template>

<script>
    import $ from 'jquery';
    import Storage from 'vue-local-storage'
    import SERVICE from "../services/ApiService";

    export default {
        name: "select-action",
        data: () => ({
            showAlertUnlockSuccess: undefined,
            showAlertResetSuccess: undefined,
            showReset: undefined,
            showResetAccept: undefined,
            showResetPwd:undefined,
            showUnlockOption: false,
            countClicked: 0,
            dataClicked: [],
            inputSecurity: "",
            showLoadingModal: false,
            password_confirm:"",
            params:{
                password:""
            },
            data:[],
        }),
        created() {
            this.load();
        },
        methods: {
            exit(){
                SERVICE.dispatch("exit",{self:this});
            },
            //Funcion que carga la informacion
            load(){
                this.openLoadModal();
                setTimeout(() => {
                    this.data = Storage.get("data_user");
                    this.closeLoadModal();
                }, 2000)
            },
            //Funcion que muestra la carga por modal
            openLoadModal(){
                this.showLoadingModal = true;
                $(document).ready(() => {
                    $('#exampleModalCenter').modal({show: true, backdrop: 'static', keyboard: false});
                });
            },
            //Funcion que oculta la carga por modal
            closeLoadModal(){
                $('#exampleModalCenter').modal('hide');
                this.showLoadingModal = false;
            },
            //Funcion que envia por POST el desbloqueo de la cuenta
            unlock() {
                this.showAlertUnlockSuccess = undefined;
                this.showAlertResetSuccess = undefined;
                //ejecutar con modal carga
                this.openLoadModal();
                setTimeout(() => {
                    this.closeLoadModal();
                    this.showAlertUnlockSuccess = true;
                }, 2000)
            },
            //Funcion que simula un error
            unlockdbl() {
                this.dataClicked.push(this);
                if (this.dataClicked.length >= 3) this.showUnlockOption = true;
                this.showAlertUnlockSuccess = false;
            },
            //Funcion que muestra la informacion de pasos para resetear una contraseña
            reset() {
                //cerrar alertas activas
                this.showAlertUnlockSuccess = undefined;
                this.showUnlockOption = false;
                this.dataClicked = [];
                this.showReset = true;
            },
            //funcion que acepta obtener el codigo de seguridad por mensaje
            btnYes() {
                //ejecutar con modal carga
                this.openLoadModal();
                setTimeout(() => {
                    this.closeLoadModal();
                    this.showReset = undefined;
                    this.showResetAccept = true;
                    this.inputSecurity = "";
                }, 2000)
            },
            //Funcion que cancela el envio del codigo de seguridad
            btnNot(){
                this.btnNotSecutity();
            },
            //Funcion que envia por POST el codigo de seguridad
            btnYesSecutity() {
                //ejecutar con modal carga
                this.openLoadModal();
                setTimeout(() => {
                    this.closeLoadModal();
                    this.showReset = undefined ;
                    this.showResetAccept = undefined ;
                    this.showResetPwd = true;
                }, 2000)
            },
            //Funcion que cancela enviar por POST el codigo de seguridad
            btnNotSecutity() {
                this.showReset = undefined;
                this.showResetAccept = undefined;
            },
            //Funcion que envia por POST las contraseña nueva confirmada
            resetPwd(){
                //ejecutar con modal carga
                this.openLoadModal();
                setTimeout(() => {
                    this.closeLoadModal();
                    this.showReset = undefined ;
                    this.showResetAccept = undefined ;
                    this.showResetPwd = undefined;
                    this.showAlertUnlockSuccess = undefined;
                    this.showAlertResetSuccess = true;
                }, 2000)
            },
            //Funcion que confirma la validacion de contraseñas
            validateConfirmPwd() {
                return (this.params.password !== "" && this.password_confirm !== "") ? (this.params.password === this.password_confirm) ? false : true : true
            }
        }
    }

</script>

<style scoped>

</style>