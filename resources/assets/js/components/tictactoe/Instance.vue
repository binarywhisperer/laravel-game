<template>
<div class="instance-select" @click="selectInstance">
{{schematicName()}}
            <div class="instance-label"><div class="label-type">{{playerIcon(gamer.id)}}</div><div class="label-data label-gamer" :style="{background:gamer.primaryColor,borderColor:gamer.secondaryColor}">{{gamer.name}}</div></div>
            <div class="instance-label" :opponent="opponent" v-for="opponent in opponents" :key="opponent.id">
                <div class="label-type">{{playerIcon(opponent.id)}}</div><div class="label-data label-gamer" :style="{background:opponent.primaryColor,borderColor:opponent.secondaryColor}">{{opponent.name}}</div>
            </div>
            <div class="instance-label"><div class="label-type"><i class="fa fa-clock-o"></i></div><div class="label-data label-last">{{last_at.format('MMM Do')}}<br>{{last_at.format('h:mm:ssA')}}</div></div>
            <div class="instance-label"><div class="label-type"><i class="fa fa-refresh"></i></div><div class="label-data">{{turn}}</div></div>
            <div class="instance-label"><div class="label-type">#</div><div class="label-data">{{instance.id}}</div></div>
</div>
</template>

<script>
    export default {
        props:['instance','gamer'],
        data: function(){
                return {
                    turn: _.size(JSON.parse(this.instance.edges)) + 1,
                    last_at:moment(this.instance.updated_at.date)
                }
            },
        methods:{
            schematicName(){
                if(this.instance.schematic == 1){
                    return 'Tic Tac Toe';
                }
                if(this.instance.schematic == 2){
                    return 'Checkers';
                }
                if(this.instance.schematic == 3){
                    return 'Chess';
                }
                return 'unknown' + this.instance.schematic;
            },
            playerIcon(player){
                if(this.instance.users[0].id == player ){
                    return 'X';
                }
                return 'O';
            },
            selectInstance(){
                GameEvent.fire('instanceSelected', this.instance);
            }
        },
        computed:{
            opponents(){
                let gamer = this.gamer.id;
                return _.filter(this.instance.users, function(user){
                    return user.id != gamer;
                });
            }
        },
        mounted() {
        }
    }
</script>