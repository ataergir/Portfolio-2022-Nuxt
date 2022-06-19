<template>
  <a target="_blank" rel="noopener noreferrer" href="mailto:aergir@proton.me" class="available-container">
    <div class="circle"></div>
    <!-- <p class="available-text">Available</p> -->
    <p class="available-text">
    Available
    </p>
  </a>
</template>

<style lang="scss">
$available-size: 2.2rem;
$available-size-small: 1rem;
.available-container{
    border-radius: 999vw;
    width:min-content;
    height:min-content;
    border: 1px solid black;
    display: flex;
    flex-direction: row;
    &:hover{
        .circle{
            animation: none;
        }
    }
}

.circle{
    border-radius: 999vw;
    background-color: $available-color;
    animation: blinkingCircle 2s infinite ease-in-out;
    border:1px solid black;
    
    margin: auto;
    
    @media screen and (min-width: 320px) {
        margin-left: $available-size-small * 0.75 * 0.5;
        width: $available-size-small * 0.75;
        height: $available-size-small * 0.75;
    }
    @media screen and (min-width: 720px) {
        margin-left: $available-size * 0.75 * 0.5;
        width: $available-size * 0.75;
        height: $available-size * 0.75;
    }
}

@keyframes blinkingCircle{
    0%		{ background-color: $available-color;}
    50%		{ background-color: transparent;}
    100%	{ background-color: $available-color;}
}

.available-text{
    opacity:1;
    
    white-space: nowrap;
    color:black;
    text-decoration: none;
    

    @media screen and (min-width: 320px) {
        font-size:$available-size-small;
        padding:0.2rem 0.5rem;
    }
    @media screen and (min-width: 720px) {
        font-size:$available-size;
        padding:0.4rem 1rem;
    }
}

.available-container:visited{
    color:black;
}
</style>