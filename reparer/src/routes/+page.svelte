<script lang="ts">
    const alphabet = ["A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "X", "Y", "Z"]
    const deck = {
        marks: {
            length: 0,
            percentage: 0,
            startIndex: 0,
            stopIndex: 4,
            number: 0
        },
        length: 800,
        train: {
            direction: "left",
            sn: 0,
            nb: 2,
            length: 200,
            percentage: 0,
            coaches: {
                nb: 8,
                percentage: 0
            }
        }
    }
    function getBgClass(index, total) {
        if (index === 0 || index === total - 1) {
            return "bg-red-500"; // Red for the first or last coach
        } else {
            return "bg-blue-500"; // Blue for all other coaches
        }
    }
$: {
    deck.marks.number = alphabet.slice(deck.marks.startIndex, deck.marks.stopIndex + 1).length
    deck.marks.length = deck.length / deck.marks.number 
    deck.marks.percentage = (deck.marks.length / deck.length) * 100
    deck.train.percentage = (deck.train.length / deck.length) * 100
    deck.train.coaches.percentage = (deck.train.coaches.nb / deck.train.nb) * 100
}
</script>
<section>
   <div>
    <p> Nombre de reperes </p>
    <label>Repere de Depart</label>
    <select bind:value={deck.marks.startIndex}>
        {#each alphabet as letter, index}
            <option value={index}>{letter}</option>
        {/each}
    </select> 
    <label>Repere de fin</label>
    <select bind:value={deck.marks.stopIndex}>
        {#each alphabet as letter, index}
            <option value={index}>{letter}</option>
        {/each}
    </select> 
    <p>Longueur de quai</p>
    <input type="number" bind:value={deck.length} /> Metres
   </div>
   <p>Train</p>
   <label>Direction</label>
   <select bind:value={deck.train.direction}>
        <option value="left">Gauche</option>
        <option value="right">Droite</option>
   </select>
   <label>Nombre de trains</label>
   <input type="number" bind:value={deck.train.nb}/>
   <label>Nombre de wagons</label>
   <input type="number" bind:value={deck.train.coaches.nb}/>
   <label>longueur</label>
   <input type="number" bind:value={deck.train.length}/>
   <div class="border w-3/4 h-40">
    <div class="border w-full h-20 flex flex-row items-center {deck.train.direction == "left" ? "justify-start": "justify-end"}">        
        {#each {length: deck.train.nb} as _ }
            <div class="border h-10 flex" style="width: {deck.train.percentage}%;">
                {#each {length: deck.train.coaches.nb} as _, index }
                    <div class="border h-10 {getBgClass(index, deck.train.coaches.nb)}" style="width: {deck.train.coaches.percentage}%;">{index+1}</div>
                {/each}
            </div>
        {/each}
    </div>
    <div class="border w-full h-20 flex">
        {#each alphabet.slice(deck.marks.startIndex, deck.marks.stopIndex + 1) as letter}
            <div class="flex justify-center border" style="width: {deck.marks.percentage}%;">{letter}</div>
        {/each}
    </div>
   </div>
</section>