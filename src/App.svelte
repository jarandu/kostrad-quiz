<script>

let questions = [
    {
        q: "Hvor mange kopper med kaffe drikker du hver dag?",
        a: [
            "3-4 kopper",
            "1-2 kopper",
            "5 eller flere"
        ],
        c: 0,
        i: "coffee.jpg",
        r: "De nye kostrådene anbefaler ... . Det betyr atlkek .dawpok d."
    },
    {
        q: "Hvor mange kopper med BRUS drikker du hver dag?",
        a: [
            "3-4 kopper",
            "1-2 kopper",
            "5 eller flere"
        ],
        c: 0,
        i: "brus.jpg",
        r: "De nye kostrådene anbefaler ... . Det betyr atlkek .dawpok d."
    },
    {
        q: "Hvor mange kopper med MELK drikker du hver dag?",
        a: [
            "3-4 kopper",
            "1-2 kopper",
            "5 eller flere"
        ],
        c: 0,
        i: "brus.jpg",
        r: "De nye kostrådene anbefaler ... . Det betyr atlkek .dawpok d."
    }
]

let finalResult

$: state = {
    question: 0,
    answers: [],
    score: [],
    sum: function() {
        return this.score.reduce((b, a) => b + a, 0)
    },
    status: 0
}

function chooseAlternative(question, altenative) {
    if (state.answers[question] != undefined) return
    if (state.question != question) return
    state.answers[question] = altenative
    state.score[question] = questions[question].c == altenative ? 1 : 0
    console.log(state)
}

function nextQuestion(question) {
    if (question + 1 > questions.length)  {
        state.question = question
        state.status = 1
        finalResult.scrollIntoView({ behavior: "smooth", block: "start", inline: "nearest" })
    }
    else {
        state.question = question
        questions[question].e.scrollIntoView({ behavior: "smooth", block: "start", inline: "nearest" })
    }
}

function great() {
    return "Topp!"
}
function notGreat() {
    return "Dårlig!"
}

</script>

<div class="container">

    <div class="header">
        <h2>Hvor godt følger du de nye kostrådene?</h2>
    
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Magnam illum error, dicta deleniti distinctio magni repellendus ipsum voluptas reiciendis odit impedit in mollitia dolor explicabo provident repellat! Esse, porro amet!</p>
        
        <div class="score">
            {#each questions as question, i}
            <div class="{state.score[i] == 1 ? "pass" : state.score[i] == 0 ? "fail" : "waiting"}"></div>
            {/each}
        </div>
    </div>
    
    <div class="questions">
        {#each questions as question, i}
        <div class:answered={state.answers[i] != undefined} class:active={state.question == i} bind:this={questions[i].e}>
            <div class="pre-title">{i + 1} / {questions.length}</div>
            <h3>{question.q}</h3>
            <div class="answers">
                {#each question.a as answer, j}
                <div class="{state.answers[i] == j && state.score[i] == 1 ? "pass" : state.answers[i] == j && state.score[i] == 0 ? "fail" : ""}" on:click={() => { chooseAlternative(i, j) }} on:keypress={() => { chooseAlternative(i, j) }}><span></span>{answer}</div>
                {/each}
            </div>
            {#if state.answers[i] != undefined}
            <div class="result">
                <h4>{state.score[i] == 1 ? great() : notGreat()}</h4>
                {question.r}
                <div class="next" on:click={() => { nextQuestion(i + 1) }} on:keypress={() => { nextQuestion(i + 1) }}>Neste</div>
            </div>
            {/if}
        </div>
        {/each}
    </div>

    <div class="final-result" class:show={state.status} bind:this={finalResult}>
        {state.sum()} av {questions.length}
        <h2>{state.sum() > 2 ? "Sykt bra!" : state.sum() > 0 ? "Midt på treet!" : "Elendig!" }</h2>
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Mollitia autem esse rerum explicabo tempora amet deserunt quae quasi perferendis repellat distinctio ipsum ab voluptatum harum exercitationem, aliquam quos corporis sit?</p>
    </div>

</div>

<style>
.container {
    position: relative;
}
.header {
    background: white;
    position: sticky;
    top: 0;
    z-index: 100;
}
.score {
    display: flex;
    justify-content: space-around;
    gap: .5rem;
    margin-block: 1rem;
}
.score > div {
    height: 6px;
    flex: 1;
    background: #bbb;
    border-radius: 2px;
}
.questions {
    display: flex;
    flex-direction: column;
    gap: 5rem;
    margin-block: 3rem;
}
.questions > div {
    opacity: .3;
    user-select: none;
}
.questions > div.active {
    opacity: initial;
    user-select:initial;
    cursor: initial;
}
.pre-title {
    display: none;
    font-size: .85em;
    font-weight: 300;
    opacity: .9;
    margin-bottom: 1rem;
}
.active .pre-title {
    display: block;
}
.answers {
    display: flex;
    flex-direction: column;
    gap: 1rem;
}
.answers > div {
    display: flex;
    gap: 1rem;
    align-items: center;
    line-height: 1;
    padding: 1.5rem;
    box-shadow: 3px 3px 10px 2px #ccc;
    border-radius: 3px;
    transition: .3s;
}
.active:not(.answered) .answers > div:hover {
    background: #fcda51;
    box-shadow: none;
    transition: none;
    cursor: pointer;
}
.active.answered .answers > div:not(.pass) {
    opacity: .6;
}
.answers > div.pass,
.answers > div.fail {
    box-shadow: none;
}
.answers span {
    position: relative;
    display: inline-block;
    width: 17px;
    height: 17px;
    background: white;
    border: 2px solid #787878;
    border-radius: 50%;
}
.answers > div.fail span {
    border: 2px solid #af4537;
}
.answers > div.pass span {
    border: 2px solid #406619;
}
.active:not(.answered) .answers > div:hover span::after,
.answers > div.pass span:after,
.answers > div.fail span:after {
    position: absolute;
    width: 9px;
    height: 9px;
    left: 2px;
    top: 2px;
    content: "";
    border-radius: 50%;
    background: #787878;
}
.answers > div.fail span:after {
    background: #af4537;
}
.answers > div.pass span:after {
    background: #406619;
}
.result {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    margin-top: 2rem;
}
.next {
    display: none;
    cursor: pointer;
    font-weight: bold;
    padding: 1rem 1.5rem;
    background: #fcda51;
    width: fit-content;
    margin: 1rem auto 0;
    border-radius: 3px;
    box-shadow: 3px 3px 10px 2px #ccc;
}
.next:hover {
    box-shadow: none;
}
.active .next {
    display: block;
}
.final-result {
    display: none;
    margin-block: 4rem;
}
.final-result.show {
    display: block;
}

.pass {
    background: #8aad67 !important;
    border-color: #406619 !important;
}
.fail {
    background: #c7786d !important;
    border-color: #af4537 !important;
}
</style>
