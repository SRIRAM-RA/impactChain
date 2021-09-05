<template>
    <div class="container-1">
        <h1 class="welcome-head">Impact Chain</h1>
        <p class="welcome-content">We measure our growth and progress based on the number of students we impact, to help them pursue what they wanted to be. From day one of spotknack, students who were part of the spotknack programs, are always ahead of other students, step out from the crowd, and move forward.</p>
    </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600&family=Poppins&display=swap');

.container-1{
    padding: 40px;
    padding-top: 100px;
    text-align: center;
}

.welcome-head{
    font-family: 'Playfair Display', serif;
    font-size: 71px;
    font-weight: normal;
    font-stretch: normal;
    font-style: normal;
    line-height: normal;
    letter-spacing: normal;
    text-align: center;
    color: #000000;
}

.welcome-content{
    width: 500px;
    font-family: Poppins;
    font-size: 21px;
    margin-left: 32%;
    font-weight: normal;
    font-stretch: normal;
    font-style: normal;
    line-height: normal;
    letter-spacing: 0.6px;
    color: #000000;
}


@media screen and (max-width: 600px)
{
    .container-1{
        padding: 7px;
        padding-top: 70px;
    }
    .welcome-head{
        font-size: 55px;
    }
    .welcome-content{
        width: 100%;
        margin-left: 0px;
    }
    .col-1 {
        width: 70%;
    }
    .logo {
        width: 100%;
    }
}

@media screen and (min-width:700px) and (max-width: 800px) {
    .welcome-content {
        width: 100%;
        margin-left: 0px;
        text-align: center;
    }
}

@media screen and (min-width:800px) and (max-width: 900px) {
    .welcome-content {
        width: 100%;
        margin-left: 0px;
        text-align: center;
    }
}


@media screen and (min-width:900px) and (max-width:1200px)
{
    .welcome-content{
        width: 100%;
        margin-left: 0px;
        text-align: center;
    }
}

</style>