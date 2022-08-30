# Leeds Digital Festival

## Typing Game

### HTML

        <!DOCTYPE html>
        <html lang="en">
        <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Typing game</title>
        </head>
        <body>
        <div class="timer" id="timer">0</div>

            <div class="container">

            <div class="quote-display" id="quote-display">QUOTE</div>

            <textarea  id="quote-input" class="quote-input" autofocus>QUOTE</textarea>

        </div>

        </body>
        </html>

### CSS

        * {
        box-sizing: border-box;
        }

        body {
        display:flex;
        justify-content: center;
        align-items:center;
        min-height: 100vh;
            margin: 0;
        background-color: #1E0555;
        }

        body, .quote-input {
        font-family: "Gill Sans"
        }

        .container {
        background-color: yellow;
        padding: 1rem;
        border-radius: .5rem;
        width: 700px;
        max-width: 90%;
        }

        .timer {
        position: absolute;
        top: 2rem;
        font-size: 3rem;
        color: yellow;
        font-weight: bold;
        }

        .quote-display {
        margin-bottom: 1rem;
        // Probably dont need this specific calc bit
        margin-left: calc(1rem + 2px);
        margin-right: calc(1rem + 2px);
        }

        .quote-input {
        background-color: transparent;
        border: 2px solid #A1922E;
        outline: none;
        width: 100%;
        height: 8rem;
        margin: auto;
        resize: none;
        padding: 0.5rem 1rem;
        font-size: 1rem;
        border-radius: .5rem;
        }

        .quote-input:focus {
        border-color: black;

        }

        // add this later

        .correct {
        color: green;
        }

        .incorrect {
        color: red;
        text-decoration: underline;
        }

### JavaScript

**How to get random quote** - https://api.quotable.io/random

        const RANDOM_QUOTE_API_URL = 'https://api.quotable.io/random'

**Build fetch quote function**

        function getRandomQuote(){
        return fetch(RANDOM_QUOTE_API_URL).then((res)=>{
            return res.json()
        }).then((data)=>{
            return data.content
        })
        }

**Start get render quote - show it working**

        async function renderNewQuote(){
            const quote = await getRandomQuote()
            console.log(quote)
        }

    renderNewQuote()

**Target quote display**

       //At the top
        const quoteDisplayElement = document.getElementById('quote-display')

        // in the renderNewQuote Function
        async function renderNewQuote(){
        const quote = await getRandomQuote()
        --> quoteDisplayElement.innerText = quote;
        }

**Grab input and clear it**

        //At the top

        const quoteInputElement = document.getElementById('quote-input')

        // in the renderNewQuote Function

        async function renderNewQuote(){
            const quote = await getRandomQuote()
            quoteDisplayElement.innerText = quote;

            --> quoteInputElement.value = null;
        }

**Discuss the innerText can't be the quote - each character needs to be its own element so that the red/green css will work for each char**

     async function renderNewQuote(){
     const quote = await getRandomQuote()

        //set to blank string everytime new quote
            quoteDisplayElement.innerHTML = "";

            //get current quote - split each char

            quote.split('').forEach(char =>{

                //create a span for each char

            const characterSpan = document.createElement('span');

                //set innerText of span to be the char

            characterSpan.innerText = char;

            quoteDisplayElement.appendChild(characterSpan)

        })

    quoteInputElement.value = null;
        }

^^ **Should look the same as before - can inspect the element to see each char inside a span now** ^^

**Show can now apply individual colours to each span/char**

    async function renderNewQuote(){
        const quote = await getRandomQuote()

                quoteDisplayElement.innerHTML = "";

                quote.split('').forEach(char =>{

                const characterSpan = document.createElement('span');

        --> Delete this later but -- characterSpan.classList.add('correct')
        // go into css and add correct class
        //do same for incorrect

                characterSpan.innerText = char;

                quoteDisplayElement.appendChild(characterSpan)

            })

        quoteInputElement.value = null;
            }

^^ _Remove classes before moving on_ ^^

**Create event listener to check each time something changes**

        quoteInputElement.addEventListener('input', ()=>{
            console.log('something has changed')
        } )

**Discuss the logic needed - want to loop over spans and compare each character with its matching position in the input/whats been typed in. i.e. does index 0 of the input match index 0 of the quote**

**Get every span i.e the quote**

**Get input array/typed in**

    quoteInputElement.addEventListener('input', ()=>{
    // Quote
    const arrayQuote = quoteDisplayElement.querySelectorAll('span');
    //Input
    const arrayValue = quoteInputElement.value.split('');
    } )

**Check the two arrays - input and quote**

    quoteInputElement.addEventListener('input', ()=>{
        const arrayQuote = quoteDisplayElement.querySelectorAll('span');
        const arrayValue = quoteInputElement.value.split('');

    arrayQuote.forEach((characterSpan, index)=>{
        const character = arrayValue[index]

             if(character === characterSpan.innerText){
            characterSpan.classList.add('correct);
            characterSpan.classList.remove('incorrect);

            } else {
            characterSpan.classList.remove('correct);
            characterSpan.classList.add('incorrect);
             }

        })

    } )

_Can start typing to show it turns green, but everything else goes red - missing a check to see if the character is null, i.e. we havent typed it yet_

**Add check for null/undefined - change if to else if**

quoteInputElement.addEventListener('input', ()=>{
const arrayQuote = quoteDisplayElement.querySelectorAll('span');
const arrayValue = quoteInputElement.value.split('');

    arrayQuote.forEach((characterSpan, index)=>{
        const character = arrayValue[index]

    //If we havent typed this char- make it black/remove all classes
        if(!character){
            characterSpan.classList.remove('correct');
            characterSpan.classList.remove('incorrect')
        }

            else if(character === characterSpan.innerText){
            characterSpan.classList.add('correct);
            characterSpan.classList.remove('incorrect);

            } else {
            characterSpan.classList.remove('correct);
            characterSpan.classList.add('incorrect);
             }

        })

    } )

_Show it now working_

**Move onto next quote if all correct**

**correct variable = presume all corect until told otherwise**

    quoteInputElement.addEventListener('input', ()=>{
    const arrayQuote = quoteDisplayElement.querySelectorAll('span');
    const arrayValue = quoteInputElement.value.split('');

    -->  let correct = true;

        arrayQuote.forEach((characterSpan, index)=>{
            const character = arrayValue[index]

            if(!character){
                characterSpan.classList.remove('correct');
                characterSpan.classList.remove('incorrect')
                --> correct = false
            }

                else if(character === characterSpan.innerText){
                characterSpan.classList.add('correct);
                characterSpan.classList.remove('incorrect);

                } else {
                characterSpan.classList.remove('correct);
                characterSpan.classList.add('incorrect);
                --> correct = false;
                }

            })
    --> if(correct) renderNewQuote()
        } )

_Should not work and clear if typed correctly_

**Create timer function and grab timer element**

**After this - go through and remove default quote and 0 value from html**

        const timerElement = document.getElementById('timer')

        function startTimer(){
        timerElement.innerText = 0;

    }

**In renderNewQuote function - add startTimer at the bottom**

    quoteInputElement.value = null;
    StartTimer()
                }

**Set up timer to change every second**

_Optional - set interval not exact to the second, flight flunctations - compare start date to current date to get actual seconds_

        function startTimer(){
            timerElement.innerText = 0;

            setInterval(()=>{

            },1000)
    }

**Add set up for correct time**

        --> let startTime;

        function startTimer(){
            timerElement.innerText = 0;


            --> startTime = new Date();


            setInterval(()=>{

          -->  getTimerTime()

        },1000)

    }

**Make getTimerTime function**

//take current date

//subtract startTime

//divide by 1000 to convert to seconds

//floor to make an integer

    function getTimerTime(){
    return Math.floor((new Date() - startTime) /1000);
    }

**Set timer text to correct time**

    let startTime;

        function startTimer(){
            timerElement.innerText = 0;


            startTime = new Date();


            setInterval(()=>{

          --> timerElement.innerText = getTimerTime()

        },1000)

    }
