<template>
    <div>
        <h2> Hello {{name}} </h2>
        <input placeholder="search" v-model="searchTerm"/>
        <div v-for="line in letterMatrix">
            <span v-for="letter in line" :class="{highlighted: letter.highlighted}"> {{letter.value}}</span>
        </div>
    </div>
</template>

<script>
import matrix from './letter-array.js'

let letterMatrix = matrix.map((line) => {
    return line.map((letter) => {
        return {
            value: letter,
            highlighted: false
        }
    })
})


export default {
    data: function() {
        return {
            letterMatrix,
            name: 'World',
            searchTerm: ''
        }
    },
    watch: {
        searchTerm(newValue, oldValue) {
            console.log(newValue);
            let searchLetter = newValue.toUpperCase()[0] || ' ';
            for (let ix=0; ix < this.letterMatrix.length; ix++) {
                let row = this.letterMatrix[ix]
                for (let iy=0; iy < row.length; iy++) {
                    let letter = row[iy];
                    console.log(letter.value, letter.value === searchLetter);
                    
                        letter.highlighted = letter.value === searchLetter;
                    
                }
            }
        }
    }
}

</script>

<style scoped lang="scss">

span {
    font-family: monospace;
    &.highlighted {
        background-color: yellow;
    }
}



</style>