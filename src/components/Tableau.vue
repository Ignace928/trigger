<script lang="ts">
    export default {
        data: function(){
            return{
                couleur: "Un texte 'couleur'",
                liste: ["Bleue", "Verte", "rouge", "Violette"],
                type:["Eau", "Feuille", "Feu", "Spectre"],
                tableau:[]
            }
        },
        methods:{
            trans:function(){
                for(let i: number = 0 ; i < this.liste.length ; i++){
                    let a = this.liste[i]
                    let b = this.type[i]
                    let fusion = {couleur:a, type: b}
                    this.tableau.push(fusion)
                }
            }
        }
    }
</script>
<template>
    <table>
        <th>
            <td>Couleur</td>
            <td>Type{{trans()}}</td>
        </th>
        <tr v-for="i in tableau" :key="i.couleur">
            <td>{{i.couleur}}</td>
            <td>{{i.type}}</td>
        </tr>
    </table>
</template>