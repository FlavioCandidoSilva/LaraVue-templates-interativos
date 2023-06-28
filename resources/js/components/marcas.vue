<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-6">
                <card-component titulo="Busca de marcas">
                    <template v-slot:conteudo>
                        <div class="row">
                            <div class="mb-3 col">
                                <input-container-component titulo="ID" id="inputId" id-help="idHelp"
                                    texto-ajuda="Informe o ID da marca">
                                    <input type="number" placeholder="ID" class="form-control" id="inputId"
                                        aria-describedby="idHelp">
                                </input-container-component>
                            </div>
                            <div class="mb-3 col">
                                <input-container-component titulo="Nome da marca" id="inputNome" id-help="nomeHelp"
                                    texto-ajuda="Informe o nome da marca">
                                    <input type="text" placeholder="Nome da marca" class="form-control" id="inputNome"
                                        aria-describedby="nomeHelp">
                                </input-container-component>
                            </div>
                        </div>
                    </template>
                    <template v-slot:rodape>
                        <button type="submit" class="btn btn-primary btn-sm float-right">Pesquisar</button>
                    </template>
                </card-component>
                <!-- card marca -->

                <card-component titulo="Relação de marca">
                    <template v-slot:conteudo>
                        <table-component></table-component>
                    </template>
                    <template v-slot:rodape>
                        <button type="button" class="btn btn-primary btn-sm float-right" data-bs-toggle="modal"
                            data-bs-target="#modalMarca">Adicionar</button>
                    </template>
                </card-component>
            </div>
        </div>
        <modal-component id="modalMarca" titulo="Adicionar marca">
            <template v-slot:conteudo>
                <div class="form-group">
                    <input-container-component titulo="Nome da marca" id="novoNome" id-help="novoNomeHelp"
                        texto-ajuda="Informe o nome da marca">
                        <input type="text" placeholder="Nome da marca" class="form-control" id="novoNome"
                            aria-describedby="novoNomeHelp" v-model="nomeMarca">
                    </input-container-component>
                    {{ nomeMarca  }}
                </div>

                <div class="form-group">
                    <input-container-component titulo="Imagem" id="novoImagem" id-help="novoImagemHelp"
                        texto-ajuda="Imagem">
                        <input type="file" placeholder="Selecione uma imagem" class="form-control-file" id="novoImagem"
                            aria-describedby="novoImagemHelp" @change="carregarImagem($event)">
                    </input-container-component>
                    {{ arquivoImagem }}
                </div>
            </template>
            <template v-slot:rodape>
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Fechar</button>
                <button type="button" class="btn btn-primary" @click="salvar()">Salvar</button>
            </template>
        </modal-component>

    </div>
</template>

<script>
    export default {
        data(){
            return {
                urlBase: 'http://localhost:8000/api/v1/marca',
                nomeMarca: '',
                arquivoImagem: []
            }
        },
        methods: {
            carregarImagem(e){
                this.arquivoImagem = e.target.files[0];
            },
            salvar(){
                console.log(this.nomeMarca);
                console.log(this.arquivoImagem);

                let formData = new FormData();
                formData.append('nome', this.nomeMarca);
                formData.append('imagem', this.arquivoImagem);

                let config = {
                    headers: {
                        'Content-Type': 'multipart/form-data',
                        'Accept': 'application/json'
                    }
                }
                axios.post(this.urlBase, formData , config).then((response) => {
                    console.log(response);
                }).catch((error) => {
                    console.log(error);
                });
            }
        }
    }
</script>