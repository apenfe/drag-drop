<script setup>

    import { reactive } from 'vue';
    import InputNew from './InputNew.vue';

    let boards = reactive([
        {
            id: crypto.randomUUID(),
            name: 'waiting',
            items: [
                {
                    id: crypto.randomUUID(),
                    title: 'Limpiar la casa'
                },
                {
                    id: crypto.randomUUID(),
                    title: 'Limpiar el coche'
                },
            ]
        },
        {
            id: crypto.randomUUID(),
            name: 'to do',
            items: [
                {
                    id: crypto.randomUUID(),
                    title: 'Hacer los deberes diarios'
                },
                {
                    id: crypto.randomUUID(),
                    title: 'Hacer la cama'
                },
            ]
        }
    ]);

    function handleNewItem(text, board){

        console.log(text + " " +board.id + " " + board.name);

        board.items.push({
            id: crypto.randomUUID(),
            title: text
        });

    }

    function handleNewBoard(){

        const name = prompt("Nombre de la nueva tabla: ");

        if(name != ""){
            boards.push({

                id: crypto.randomUUID(),
                name: name,
                items: []

            });
        }

    }

    function startDrag(event, board, item){

        event.dataTransfer.setData('text/plain', JSON.stringify({boardId: board.id, itemId: item.id}));

    }

    function onDrop(event, destination){

        let {boardId, itemId} = JSON.parse(event.dataTransfer.getData('text/plain'));

        let originBoard = boards.find(item => item.id === boardId);
        let originItem = originBoard.items.find(item => item.id === itemId);

        destination.items.push({...originItem});

        originBoard.items = originBoard.items.filter(item => item != originItem);

    }

</script>

<template>

    <nav>
        <ul>
            <li><a href="#" @click.prevent="handleNewBoard">Crear tablero</a></li>
        </ul>
    </nav>

    <div class="boards-container">

        <div class="boards">

            <div class="board" 
                @drop="onDrop($event, board)" 
                @dragover.prevent 
                @dragenter.prevent 
                v-for="(board) in boards" 
                :key="board.id">

                <div class="name-board">{{board.name}}</div>

                <InputNew @onNewItem="(text) => handleNewItem(text,board)"/>

                <div class="items">

                    <div class="item"
                        draggable="true" 
                        @dragstart="startDrag($event, board, item)" 
                        v-for="item in board.items" 
                        :key="item.id">

                        <div>{{item.title}}</div>

                    </div>

                </div>

            </div>

        </div>

    </div>

</template>

<style scoped>

nav{
    background-color: black;  
}

nav ul{
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
}

nav ul li a{
    display: block;
    padding: 1.5rem;
    color: white;
    text-decoration: none;
    font-size: 2rem;
}

nav ul li a:hover{
    text-decoration: underline;
    text-decoration-color: yellow;
}

.boards{
    position: absolute;
    width: 100%;
    min-height: 100%;
    display: flex;
    flex-wrap: wrap;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    gap: 2rem;
    background: url(../assets/corcho.jpg);
}

.board{
    font-family: Arial, Helvetica, sans-serif;
    flex-basis: 20%;
    background-color: #ffef5b;
    padding: 10px;
    text-align: center;
    font-size: 18px;
    border-radius: 1rem;
    box-shadow: 0px 2px 5px black;
}

.name-board{
    font-size: 2.5rem;
    font-family: Arial, Helvetica, sans-serif;
    font-weight: 600;
    transition: all 300ms linear;
}

.name-board:hover{
    font-size: 2.8rem;
}

.items{
    margin-top: 1rem;
    display: flex;
    flex-direction: column;
    gap: 5px;
}

.item{
    background-color: white;
    padding: 10px;
    box-sizing: border-box;
    cursor: pointer;
    border-radius: 1rem;
    font-size: 2rem;
    font-weight: 500;
    font-family: Arial, Helvetica, sans-serif;
}

</style>