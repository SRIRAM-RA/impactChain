<template>
    <div>
        <section class="section-container">
            <div class="inner-box">
                <div class="section-container-head">
                    <h2 id="head-name">Student Team</h2>
                    <p id="head-content">Click on Impacter to grow their impact chain</p>
                </div>
                <div class="impact-box" id="impactBox">
                    <div class="impact-box-block" v-for='(i,$user) in userList' :key='$user'  @click=growtree(i.mentor_id) >
                        <div class = "impact-dp-box">
                            <img :src="i.pic_url" alt="" class="impact-dp">
                        </div>
                        <div class="impact-user-content">
                            <h5 id="member-count"> {{i.name}}</h5>
                            <p id="cmp-name">{{i.company_name}}</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>
        <section class="impact-chain-box" id="impactChain" action="javascript:document.location.reload()">
            <h2 class="tree-head">Student Team</h2>
        </section>
    </div>
</template>

<script>
export default {
    data(){
        return{
            userList:[]
        }
    },
    mounted(){
         this.$axios.get('https://menteeapi.spotknack.com/api/impactmentorslist')
        .then((res)=>{
            // console.log(res.data)
            this.userList=res.data;
        })
    },
    methods: {
        growtree(id){
            var mentor_id = id;
            document.getElementById("impactChain").style.display = "block";
            fetch(`https://menteeapi.spotknack.com/api/growimpactchain?mentor_id=${mentor_id}`)
                .then((response) => response.json())
                .then((data) => {
                    // fetching api JSON details
                    var len = data.length;
                    var roundLength = Math.round(len / 3);
                    if(roundLength == 0){
                        roundLength+=1;
                    }
                    var innercounter = 0;
                    var columnCount = 4;
                    var impactbox = document.getElementById("impactChain");
                    // var impactHead = document.createElement('h2');
                    // impactHead.innerHTML = "Student Team";
                    // impactbox.appendChild(impactHead);
                    for (var i = 0; i < roundLength; i++) {          //Row incrementation 
                        var chainbox = document.createElement('div');
                        if ((i % 2 != 0) && (i != 0)) {
                            chainbox.className = "chain-even-box";
                        }
                        else {
                            chainbox.className = "chain-odd-box";
                        }
                        chainbox.id = "oddbox" + i;
                        impactbox.appendChild(chainbox);
                        for (var j = 0; j < columnCount; j++) {             //column incrementation
                            var boxclass = document.getElementById("oddbox" + i);
                            var outerdiv = document.createElement('div');
                            outerdiv.className = "tooltip";
                            boxclass.appendChild(outerdiv);
                            var img = document.createElement('img');
                            img.className = "student-profile-pic";
                            if (data[innercounter].pic_url == null) {    //if json doesnot have a pic_url default pic will be assigned
                                img.src = "User-Profile-PNG-File.png";
                                img.style.border = "1px solid black";
                                img.style.background = "white";
                            }
                            else {
                                img.src = data[innercounter].pic_url;
                            }
                            img.id = "number" + innercounter;
                            outerdiv.appendChild(img);
                            var spanclass = document.createElement('span');             // tooltip assigning
                            spanclass.className = "tooltiptext";
                            var student_name = document.createElement('h5');
                            student_name.id = "student-name-tip"
                            student_name.innerHTML = data[innercounter].student_name
                            var student_name1 = document.createElement('a');
                            student_name1.setAttribute("href", data[innercounter].linkedin);
                            spanclass.appendChild(student_name);
                            spanclass.appendChild(student_name1);
                            var linkedin_ico = document.createElement('i');
                            linkedin_ico.className = "fab fa-linkedin"
                            linkedin_ico.id = "linkedin"
                            student_name1.appendChild(linkedin_ico);
                            outerdiv.appendChild(spanclass);
                            if (j != (columnCount - 1)) {                               // removing extra hr lines
                                var line = document.createElement('hr');
                                line.className = "hrline"
                                boxclass.appendChild(line);
                            }
                            // console.log(innercounter)
                            // console.log("added one person" + innercounter)
                            innercounter++
                            if (innercounter >= len)
                                j = 5;
                        }
                        var curvedLine = document.createElement('div');             // creating curved lines for joining two rows.
                        if (i == (roundLength - 1)) {
                            break;
                        }
                        else if ((i % 2 != 0) && (i != 0)) {
                            curvedLine.className = "curved-line-left";
                        }
                        else {
                            curvedLine.className = "curved-line-right";
                        }
                        impactbox.appendChild(curvedLine);
                    }
                    var hr = document.getElementsByTagName('hr');
                    var hrlength = hr.length;
                    // console.log(hrlength)
                    for (var i = 0; i < hrlength; i++) {
                        if (i == (hrlength - 1)) {
                        // hr[i - 1].style.display = "none";
                        // console.log("b" + i)
                        hr[i].style.display = "none";
                        }
                        else {
                        continue
                        }
                    }
                })
                .then(
                    setTimeout(() => {
                        document.getElementById("impactChain").scrollIntoView({ behavior: "smooth"})
                    }, 1000)
                )
                .catch((err) => { console.log(err) })
                // document.getElementById(mentor_id).removeAttribute("onclick");
                var childnode = document.getElementById("impactChain");
                while (childnode.childNodes.length > 1) {
                    childnode.removeChild(childnode.lastChild);
                }
            }
    },

};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600&family=Poppins&display=swap');


.section-container{
    padding: 40px;
    align-items: center;
}
.inner-box{
    width: 70%;
    margin-left: 16%;
    border-radius: 25px;
    padding: 40px;
    background-color: rgb(116,211,11, 0.05);
    display: flex;
    flex-direction: row;
}

.tree-head{
    font-family: Poppins;
}

.section-container-head{
    width: 30%;
    padding: 15px;
    padding-top: 30px;
    font-family: Poppins;
    color: #000000;
}

#head-name{
    font-size: 42px;
    font-weight: 700;
    font-stretch: normal;
    font-style: normal;
    line-height: normal;
    letter-spacing: 0.6px;
    text-align: left;
}
#head-content{
    opacity: 0.7;
    font-size: 12px;
    font-weight: normal;
    font-stretch: normal;
    font-style: normal;
    line-height: normal;
    letter-spacing: 0.6px;
    text-align: left;
}
.impact-box{
    width: 70%;
    padding-left: 20px;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: flex-start;
}    

.impact-box-block{
    width: 30%;
    display: flex;
    flex-direction: column;
    cursor: pointer;
}
.impact-box-block:hover{
    color: rgb(214, 185, 22);
}
.impact-dp{
    border-radius: 10px;
    width: 170px;
    height: 170px;  
    object-fit: cover;
}

.impact-user-content{
    padding: 3px;
    font-family: Poppins;
    font-stretch: normal;
    font-style: normal;
    line-height: normal;
    letter-spacing: 0.6px;
    text-align: left;
    color: #000000;
}
#member-count{
    font-size: 14px;
    font-weight: 700;
    padding-top: 7px;
    margin-top: -2%;
}

#student-name-tip{
    font-size: 14px;
    font-weight: 700;
    padding-top: 7px;
    margin: 0;
}

.impact-user-content h5:hover{
    color: rgb(214, 185, 22);
}
#cmp-name{
    font-size: 12px;
    opacity: 0.7;
    margin-top: -10%;
}
.impact-chain-box{
    display: none;
    padding: 45px;
    text-align: center;
    font-size: 40px;
    font-stretch: normal;
    font-style: normal;
    line-height: normal;
    letter-spacing: 0.6px;
    color: #000000;
}
h2{
    font-weight: 700;
}
.chain-odd-box{
    padding: 35px;
    padding-left: 110px;
    display: flex;
    flex-direction: row;
    align-self: flex-start;
}
.img-block{
    width: 25%;
    padding: 15px;
    border-radius: 50%;
    justify-content: space-evenly;
}
.student-profile-pic{
    width: 195px;
    height: 195px;
    margin-top: 2%;
    border-radius: 50%;
    object-fit: cover;
    z-index: 1;
    background-color: transparent;
}
.student-profile-pic:hover{
    border: 3px solid #74d30b ; 
    transition: 0.025s; 
}

hr {
    width: 10%;
    display: block;
    height: 2px;
    border: 0;
    border-top: 4px solid #74d30b;
    margin: 2.5em 0;
    padding: 0;
}
.chain-even-box{
    flex-direction: row-reverse !important;
    padding: 35px;
    padding-right: 145px;
    display: flex;
    align-self: flex-end;
}

.curved-line-right {
    margin-left: 70.1%;
    margin-top: -1%;
    width: 470px; 
    height: 105px;  
    border: solid 5px #74d30b;
    border-color: #74d30b transparent transparent transparent ;
    border-radius: 50%/100px 110px 0 0;
    transform: rotate(90deg);
    z-index: -999 ;
}
.curved-line-left{
    margin-left: -8%;
    margin-top: -1%;
    width: 470px; 
    height: 105px;  
    border: solid 5px #74d30b;
    border-color: #74d30b transparent transparent transparent ;
    border-radius: 50%/100px 110px 0 0;
    transform: rotate(270deg);
    z-index: -999 ;   
}


/* tooltip */
.tooltip {
    border-radius: 50%;
    position: relative;
    display: inline-block;
    z-index: 1;
}
  
.tooltip .tooltiptext {
    visibility: hidden;
    width: 160px;
    background-color: #fff;
    text-align: center;
    border-radius: 6px;
    position: absolute;
    font-family: Poppins;
    font-size: 16px;
    font-weight: 600;
    font-stretch: normal;
    font-style: normal;
    line-height: normal;
    letter-spacing: 0.6px;
    text-align: left;
    color: #000000;
    z-index: 1;
    bottom: 102%;
    left: 0%;
    padding: 0px 2px 1px 2px;
    box-shadow: 0 4px 4px 0 rgba(0, 0, 0, 0.25);
    border: solid 3px #74d30b;
    z-index: 20;
}
  
.tooltip .tooltiptext::after {
    content: "";
    position: absolute;
    top: 100%;
    left: 50%;
    margin-top: -5px;
    border-width: 8px;
    border-style: solid;
    z-index: 20;
    border-color: transparent #74d30b transparent transparent;
}
.tooltip:hover .tooltiptext {
    visibility: visible;
}

@media screen and (max-width: 600px) {
    .section-container {
        padding: 3px;
    }
    .section-container-head {
        width: 100%;
        text-align: center !important;
        padding: 7px;
    }
    .inner-box {
        width: 100%;
        margin-left: 0px;
        padding: 0px;
        flex-direction: column;
    }
    #head-content {
        font-size: 15px;
        margin-top: -5%;
    }
    .impact-box {
        width: 100%;
        padding: 0px;
    }
    .impact-box-block {
        width: 50%;
    }
    .impact-dp-box {
        padding: 10px;
    }
    .impact-dp {
        width: 155px;
        height: 155px;
    }
    .impact-user-content {
        text-align: center !important;
    }
    .impact-chain-box {
        padding: 20px;
    }
    .chain-even-box {
        padding: 65px;
        padding-top: 0px;
        padding-bottom: 0px;
        flex-direction: column !important;
    }
    .chain-odd-box {
        padding: 65px;
        padding-top: 0px;
        padding-bottom: 0px;
        flex-direction: column;
    }
    hr {
        width: 100%;
        margin: 1.5rem 0;
        transform: rotate(90deg);
        margin-left: 2%;
    }
    .student-profile-pic {
        margin-top: 17%;
    }
    .curved-line-right {
        width: 235px;
        height: 40px;
        transform: rotate(90deg);
        border: solid 4px #74d30b;
        border-color: #74d30b transparent transparent transparent;
        margin-left: 21px;
        margin-top: 0;
    }
    .curved-line-left {
        width: 235px;
        height: 40px;
        transform: rotate(90deg);
        border: solid 4px #74d30b;
        border-color: #74d30b transparent transparent transparent;
        margin-left: 21px;
        margin-top: 0;
    }
    .tooltiptext {
        bottom: 90% !important;
        margin-left: 0%;
    }
}

@media screen and (min-width: 300px) and (max-width: 350px) {
    hr {
        margin-left: 10%;
    }
    .chain-even-box {
        padding-left: 49px;
    }
    .chain-odd-box {
        padding-left: 49px;
    }
    .curved-line-left {
        margin-left: 4px !important;
    }
    .curved-line-right {
        margin-left: 4px;
    }
    .impact-dp{
        width: 139px;
        height: 145px;
    }
}

@media screen and (min-width: 380px) and (max-width: 400px){
    .curved-line-left {
        margin-left: 4px !important;
    }
    .curved-line-right {
        margin-left: 26px;
    }
}

@media screen and (min-width: 400px) and (max-width: 600px) {
    .chain-even-box {
        padding: 97px !important;
        padding-top: 0px !important;
        padding-bottom: 0px;
        flex-direction: column !important;
    }
    .chain-odd-box {
        padding: 97px !important;
        padding-top: 0px !important;
        padding-bottom: 0px;
        flex-direction: column;
    }
    hr {
        margin-left: 6% !important;
    }
    .curved-line-right {
        margin-left: 33%;
        margin-top: -10%;
        width: 140px;
        height: 0px;
        border: solid 4px #74d30b;
        border-color: #74d30b transparent transparent transparent;
        /* border-radius: 50%/100px 110px 0 0; */
        transform: rotate(90deg);
        z-index: -999;
    }
    .curved-line-left {
        margin-left: 33%;
        margin-top: -10%;
        width: 140px;
        height: 0px;
        border: solid 4px #74d30b;
        border-color: #74d30b transparent transparent transparent;
        /* border-radius: 50%/100px 110px 0 0; */
        transform: rotate(90deg);
        z-index: -999;
    }
}
@media screen and (min-width: 600px) and (max-width: 700px){
    .welcome-content{
        width: 100%;
        margin-left: 0px;
    }
    .inner-box{
        margin-left: 0px;
        width: 100%;
        padding: 10px;
        flex-direction: column;
    }
    .section-container-head{
        width: 100%;
    }
    .impact-box-block{
        width:50%;
    }
    #head-name{
        font-size: 50px;
    }
    #head-content{
        font-size: 15px;
    }
    .chain-odd-box{
        flex-direction: column;
        padding-left: 25px; 
    }
    .chain-even-box{
        flex-direction: column !important;
        padding-right: 120px;
        padding-right: 50px;
    }
    hr{
        width: 100%;
        margin: 3.5rem 0;
        transform: rotate(
        90deg);
        margin-left: 1%;
    }
    .curved-line-right{
        width: 235px;
        height: 40px;
        transform: rotate(
        90deg);
        border: solid 4px #74d30b;
        border-color: #74d30b transparent transparent transparent;
        margin-left: 22%;
        margin-top: 0;
    }
}
@media screen and (min-width: 600px) and (max-width: 640px){
    .curved-line-right{
        margin-left: 22% !important;
    }
}
@media screen and (min-width: 640px) and (max-width: 660px){
    .curved-line-right{
        margin-left: 24% !important;
    }
}
@media screen and (min-width: 660px) and (max-width: 680px){
    .curved-line-right{
        margin-left: 26% !important;
    }
}
@media screen and (min-width: 680px) and (max-width: 700px){
    .curved-line-right{
        margin-left: 26.1% !important;
    }
}
@media screen and (min-width: 400px) and (max-width: 450px){
     .impact-dp{
        width: 174px;
        height: 170px;
    }
}
@media screen and (min-width:700px) and (max-width: 800px) {
    .inner-box {
        width: 100%;
        margin-left: 0px;
        padding: 0px;
    }
    .impact-chain-box {
        padding: 20px;
    }
    .impact-box-block {
        width: 45%;
    }
    #member-count {
        margin-top: 0%;
    }
    .chain-even-box {
        padding: 0px;
    }
    .chain-odd-box {
        padding: 0px;
    }
    .student-profile-pic {
        width: 128px;
        height: 128px;
    }
    hr {
        margin: 2em 0;
    }
    .curved-line-left {
        margin-left: -23%;
        width: 485px;
        height: 144px;
    }
    .curved-line-right {
        margin-left: 59%;
        width: 463px;
        height: 114px;
    }
    .impact-box {
        padding-top: 15px;
    }
}

@media screen and (min-width: 700px) and (max-width: 730px){
    .curved-line-right{
        margin-left: 56% !important;
        width: 445px !important;
        height: 117px !important;
    }
}
@media screen and (min-width:800px) and (max-width: 900px) {
    .inner-box {
        width: 90%;
        margin-left: 33px;
        padding: 0px;
    }
    .impact-chain-box {
        padding: 20px;
    }
    .impact-box-block {
        width: 40%;
    }
    #member-count {
        margin-top: 0%;
    }
    .chain-even-box {
        padding: 0px;
    }
    .chain-odd-box {
        padding: 0px;
    }
    .student-profile-pic {
        width: 145px;
        height: 145px;
    }
    hr {
        margin: 2em 0;
    }
    .curved-line-left {
        display: none;
    }
    .curved-line-right {
        display: none;
    }
}
@media screen and (min-width:800px) and (max-width: 860px){
    .impact-box-block{
        width: 43%;
    }
}
@media screen and (min-width:900px) and (max-width:1200px) {
    .inner-box {
        width: 75%;
        margin-left: 8.5%;
    }
    .impact-box-block {
        width: 33%;
    }
    .impact-dp {
        width: 150px;
        height: 150px;
    }
    .chain-even-box {
        padding-left: 3px;
        padding-right: 35px;
    }
    .chain-odd-box {
        padding-left: 0px;
    }
}

@media screen and (min-width: 900px) and (max-width: 1000px) {
    .curved-line-left {
        display: none;
    }
    .curved-line-right {
        display: none;
    }
}

@media screen and (min-width:900px) and (max-width: 980px){
    .impact-box-block{
        width: 41%;
    }
}
@media screen and (min-width:980px) and (max-width: 1050px){
    .impact-box-block{
        width: 37%;
    }
}
@media screen and (min-width: 1000px) and (max-width: 1040px) {
    .curved-line-left {
        margin-left: -20%;
    }
    .curved-line-right {
        margin-left: 67%;
    }
}

@media screen and (min-width: 1200px) and (max-width: 1350px) {
    .chain-odd-box {
        padding-left: 88px;
    }
    .chain-even-box {
        padding-right: 20px;
    }
}
@media screen and (min-width: 1350px) and (max-width: 1400px){
    .chain-odd-box {
        padding-left: 100px;
    }
    .chain-even-box{
        padding-right: 36px;
    }
    .curver-line-right{
        margin-left: 72.1%;
    }
}
@media screen and (min-width: 1400px) and (max-width: 1450px){
    .chain-odd-box {
        padding-left: 100px;
    }
    .chain-even-box{
        padding-right: 65px;
    }
    .curver-line-right{
        margin-left: 72.1%;
    }
}
@media screen and (min-width: 1450px) and (max-width: 1490px){
    .chain-odd-box {
        padding-left: 100px;
    }
    .chain-even-box{
        padding-right: 105px;
    }
    .curver-line-right{
        margin-left: 72.1%;
    }
}
@media screen and (min-width: 1200px) and (max-width: 1350px){
    .impact-box-block{
        width: 33%;
    }
}
@media screen and (min-width:1200px) and (max-width: 1260px){
    .impact-box-block{
        width: 35%;
    }
}
</style>
