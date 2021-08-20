<template>
    <div class="topnav" id="myTopnav">
        <div class="col-1">
            <a href=#><img src="https://spotknack.com/mailimages/spotknacklogo.png" class="logo">
            </a>
        </div>
    </div>
</template>

<style>

.topnav {
    width: 100%;
    overflow: none;
    display: flex;
    background-color: #ffffff;
    flex-direction: row;
    justify-content: space-between;
    vertical-align: middle;
    z-index: 999;
    position: -webkit-sticky;
  position: sticky;
  top: 0;
}

.topnav a {
    display: block;
    padding: 18px;
    float: right;
    font-size: 18px;
    text-decoration: none;
    font-family: 'Poppins', sans-serif;
    font-weight: normal;
    font-stretch: normal;
    font-style: normal;
    line-height: normal;
    letter-spacing: 0.6px;
    text-align: left;
    color: #000000;
    text-align: center;
}

.topnav .icon {
    display: none;
}
.logo {
    width: 70%;
}
    
</style>

