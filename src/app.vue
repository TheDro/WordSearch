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

let directions = [[1,0], [1,1], [0,1], [-1,1], [-1,0], [-1,-1], [0, -1], [1,-1]];


function recursiveSearch(letterMatrix, coord, dir, searchWord) {
    let [ix,iy] = coord
    if (searchWord.length === 0) {
        return true
    } else if (ix < 0 || ix >= 40 || iy < 0 || iy >= 40) {
        return false;
    } else if (searchWord[0] === letterMatrix[ix][iy].value) {
        let found = recursiveSearch(letterMatrix, [ix+dir[0], iy+dir[1]], dir, searchWord.substr(1,searchWord.length));
        letterMatrix[ix][iy].highlighted = letterMatrix[ix][iy].highlighted || found;
        return found;
    } else {
        return false;
    }
}

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
            let searchWord = newValue.toUpperCase() || ' ';

            // reset
            for (let ix=0; ix < this.letterMatrix.length; ix++) {
                let row = this.letterMatrix[ix]
                for (let iy=0; iy < row.length; iy++) {
                    let letter = row[iy];
                    letter.highlighted = false;
                }
            }
            // search
            for (let ix=0; ix < this.letterMatrix.length; ix++) {
                let row = this.letterMatrix[ix]
                for (let iy=0; iy < row.length; iy++) {
                    let letter = row[iy];
                    for (let dir of directions) {
                        recursiveSearch(this.letterMatrix, [ix,iy], dir, searchWord)
                    }
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