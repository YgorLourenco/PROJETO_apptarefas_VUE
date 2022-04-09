<template>
    <div id="task">
        <!-- <h1> Componente Task</h1> -->
        <form @submit.prevent='addItem'>
            <input
                type="text"
                placeholder="Tarefa de hoje?"
                v-model="tarefa"
            />
            <button type="submit">Adicionar</button>
        </form>
        <!-- Pegando variaveis do componente Task pra enviar para o Item -->
        <Item :lista="tarefas" :delete="deleteTask"/>

        <!-- Vai contando a quantidade de itens do array que são tarefas e mostrando -->
        <span v-show="tarefas.length > 0">
            <!-- pend: vai estilizar a contagem -->
            Você tem <strong :class="{pend: pendente}">{{tarefas.length}}</strong> tarefas pendentes
        </span>

    </div>

   

</template>


<script>
// Importando componente pra colocar em outro componente
import Item from './Item.vue'
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name:'Task',
    components:{
        Item
    },
    data() {
        return{
            tarefa:'',
            tarefas:[],
            pendente: false
        }
    },
    methods:{
        // Se a tarefa for diferente de vazio, 
        // colocar uma tarefa no array com uma data de agora
        // Se não, alerta com mensagem
        addItem(){
           if(this.tarefa !== ''){
               this.tarefas.push({
                   text: this.tarefa,
                   key: Date.now(),
               })
           }else{
               alert('Digite uma tarefa...')
               return;
           }
           this.tarefa = '';
           console.log(this.tarefas)
        },
        // Vai filtrar todas as keys que são diferentes da que esta armazenada em tarefas
        deleteTask(key){
            let filtro = this.tarefas.filter((item) => {
                return (item.key !== key);
            });
            return this.tarefas = filtro
        }
    },
    watch:{
        // Vai salvar os items no cache do site
        tarefas:{
            // Deep monitora mudanças em cada propriedade de um objeto
            deep:true,
            handler(){
                localStorage.setItem('tasks', JSON.stringify(this.tarefas))
                console.log('SALVOU...')
                // Vai criar a regra pra estilização da contagem, se maior que 4 items vai ficar vermelho
                this.tarefas.length > 4 ? this.pendente = true : this.pendente = false;
            }
        }
    },
    // created - É um ciclo de vida do Vue, que é chamado automaticamente após a instancia do Vue ser criada
    created(){
        // Salvar as tarefas dentro do JSON das tarefas
        const minhaLista = localStorage.getItem('tasks');
        this.tarefas = JSON.parse(minhaLista) || []
    }
}
</script>

<style scoped>
    #task{
        max-width: 700px;
        background: #FFF;
        border-radius: 4px;
        padding: 20px;
        margin: 20px auto;
        box-shadow: 0 0 20px rgba(0,0,0,0.3);
    }
    form{
        margin-top: 30px;
        display: flex;
        flex-direction: row;
    }
    form button{
        cursor: pointer;
        background: #0f5959;
        border: 0;
        border-radius: 4px;
        margin-left: 10px;
        padding: 0 15px;
        display: flex;
        justify-content: center;
        align-items: center;
        color: #FFF;
    }
    input{
        flex:1;
        border: 1px solid #eee;
        padding: 6px 10px;
        border-radius: 4px;
        font-size: 14px;
        outline: none;
    }
    .pend{
        color: #FF0000;
    }
</style>