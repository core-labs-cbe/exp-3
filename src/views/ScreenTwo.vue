 <!--Designed & Developed by:
    Nirmal K (@cyberwizard1001)⁶
    Nitin Ravi (@nitinravi)
    Ritwik M (@ritwik-m) &
    Nalinadevi K (@knaldev)
 for the Ministry of Education (MOE) virtual-labs project, Govt. Of India -->

<script>
import { ref } from 'vue'
export default {
    setup() {
        const items = ref([

            { id: 10, word: 'getCustomerName():String', list: 13 },
            { id: 6, word: 'setCustomerName(name:String):void', list: 13 },
            { id: 4, word: 'setClerkName(name:String):void', list: 13 },
            { id: 13, word: 'signContract(cObj:Customer):void', list: 13 },
            { id: 31, word: 'getClerkName():String', list: 13 },
            { id: 12, word: 'visitOffice():void', list: 13 },
            { id: 5, word: 'releaseContract():String', list: 13 },
            { id: 1, word: 'locateReservation(cName:String):String', list: 13 },
            { id: 15, word: 'updateKeyStatus(processedBy:String):void', list: 13 },
            { id: 7, word: 'requestVehicle(cObj:Customer):String', list: 13 },

        ]);

        const getList = (list) => {
            return items.value.filter((item) => item.list === list)
        }

        const startDrag = (event, item) => {
            console.log(item)
            event.dataTransfer.setData('itemID', item.id)
            event.dataTransfer.effectAllowed = 'move'
            event.dataTransfer.dropEffect = 'move'
        }

        const onDrop = (event, list) => {
            event.preventDefault()
            const itemID = event.dataTransfer.getData('itemID')
            const item = items.value.find((item) => item.id === parseInt(itemID))
            item.list = list
        }

        return {
            getList,
            startDrag,
            onDrop
        }
    },
    data() {
        return {
            valid: [false, false, false, false],
            validated: false
        }
    },
    methods: {
        reload() {
            this.$router.go();
        },
        validate() {
            this.validated = true;

            for (let i = 0; i < 4; i++) {
                this.valid[i] = false;
            }

            let customer = ''
            let clerk = ''
            let vehicle = ''
            let reservation = ''

            for (let i = 0; i < this.getList(1).length; i++) {
                customer = customer + this.getList(1)[i].word
            }

            for (let i = 0; i < this.getList(2).length; i++) {
                clerk = clerk + this.getList(2)[i].word
            }

            for (let i = 0; i < this.getList(3).length; i++) {
                vehicle = vehicle + this.getList(3)[i].word
            }

            for (let i = 0; i < this.getList(4).length; i++) {
                reservation = reservation + this.getList(4)[i].word
            }

            if (customer.includes("visitOffice():void") && customer.includes("getCustomerName():String") && customer.includes("setCustomerName(name:String):void") && (customer.length == 75)) {
                this.valid[0] = true;
                var x = document.getElementById("snackbar");
                x.innerHTML = "Customer class completed.";
                x.style.backgroundColor = "green";
                x.className = "show";
                setTimeout(function () { x.className = x.className.replace("show", ""); }, 3000);
            }

            if (clerk.includes("getClerkName():String") && clerk.includes("setClerkName(name:String):void") && clerk.includes("requestVehicle(cObj:Customer):String") && clerk.includes("signContract(cObj:Customer):void") && (clerk.length == 119)) {
                this.valid[1] = true;
                var x = document.getElementById("snackbar");
                x.innerHTML = "Clerk class completed.";
                x.style.backgroundColor = "green";
                x.className = "show";
                setTimeout(function () { x.className = x.className.replace("show", ""); }, 3000);
            }

            if (vehicle.includes("updateKeyStatus(processedBy:String):void") && (vehicle.length == 40)) {
                this.valid[2] = true;
                var x = document.getElementById("snackbar");
                x.innerHTML = "Vehicle class completed.";
                x.style.backgroundColor = "green";
                x.className = "show";
                setTimeout(function () { x.className = x.className.replace("show", ""); }, 3000);
            }

            if (reservation.includes("locateReservation(cName:String):String") && reservation.includes("releaseContract():String") && (reservation.length == 62)) {
                this.valid[3] = true;
                var x = document.getElementById("snackbar");
                x.innerHTML = "Reservation class completed.";
                x.style.backgroundColor = "green";
                x.className = "show";
                setTimeout(function () { x.className = x.className.replace("show", ""); }, 3000);
            }


            if ((this.valid[0] == true) && (this.valid[1] == true) && (this.valid[2] == true) && (this.valid[3] == true)) { this.$router.push('screen-three'); }
            
            console.log(this.validated)
            console.log(reservation.length)
            console.log(customer)
            console.log(clerk)
            console.log(vehicle)
            console.log(reservation)


        }
    }
}
</script>


<template class="app">
    <header style="padding: 20px">
        <h1>Step 2: Classifying functions into classes</h1>
        <p>To classify functions into classes, drag functions from the right corner and drop them into the class boxes.
        </p>
        <button class="button-30" @click="reload()">Reload</button>
    </header>
    <main>
        <div id="snackbar">Some text some message..</div>

        <div class="flex">
            <div class="flex-horizontal">
                <div>
                    <div class="flex-horizontal">
                        <div class="outer-box">
                            <h3>Customer</h3>
                            <div class="unidentified drag-el-horizontal" draggable="false"
                                :class="{ correct: (validated && valid[0]), incorrect: (validated && !valid[0]) }">

                                <div class="drop-zone" @drop="onDrop($event, 1)" @dragenter.prevent @dragover.prevent>
                                    <div v-for="item in getList(1)" :key="item.id"
                                        class="unidentified drag-el-horizontal" draggable="true"
                                        @dragstart="startDrag($event, item)">
                                        <p>{{ item.word }}</p>
                                    </div>
                                </div>
                            </div>
                        </div>


                        <div class="outer-box">
                            <h3>Clerk</h3>
                            <div class="unidentified drag-el-horizontal" draggable="false"
                                :class="{ correct: (validated && valid[1]), incorrect: (validated && !valid[1]) }">

                                <div class="drop-zone" @drop="onDrop($event, 2)" @dragenter.prevent @dragover.prevent>
                                    <div v-for="item in getList(2)" :key="item.id"
                                        class="unidentified drag-el-horizontal" draggable="true"
                                        @dragstart="startDrag($event, item)">
                                        <p>{{ item.word }}</p>
                                    </div>
                                </div>
                            </div>

                        </div>
                    </div>

                    <div class="flex-horizontal">
                        <div class="outer-box">
                            <h3>Vehicle</h3>

                            <div class="unidentified drag-el-horizontal" draggable="false"
                                :class="{ correct: (validated && valid[2]), incorrect: (validated && !valid[2]) }">

                                <div class="drop-zone" @drop="onDrop($event, 3)" @dragenter.prevent @dragover.prevent>
                                    <div v-for="item in getList(3)" :key="item.id"
                                        class="unidentified drag-el-horizontal" draggable="true"
                                        @dragstart="startDrag($event, item)">
                                        <p>{{ item.word }}</p>
                                    </div>
                                </div>
                            </div>

                        </div>

                        <div class="outer-box">
                            <h3>Reservation</h3>

                            <div class="unidentified drag-el-horizontal" draggable="false"
                                :class="{ correct: (validated && valid[3]), incorrect: (validated && !valid[3]) }">

                                <div class="drop-zone" @drop="onDrop($event, 4)" @dragenter.prevent @dragover.prevent>
                                    <div v-for="item in getList(4)" :key="item.id"
                                        class="unidentified drag-el-horizontal" draggable="true"
                                        @dragstart="startDrag($event, item)">
                                        <p>{{ item.word }}</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

            </div>

            <div>
                <div class="outer-box">
                    <h3>Methods</h3>

                    <div class="unidentified drag-el-horizontal" draggable="false">
                        <div class="drop-zone" @drop="onDrop($event, 13)" @dragenter.prevent @dragover.prevent>
                            <div v-for="item in getList(13)" :key="item.id" class="unidentified drag-el-horizontal"
                                draggable="true" @dragstart="startDrag($event, item)">
                                <p>{{ item.word }}</p>
                            </div>
                        </div>
                    </div>

                </div>
            </div>
        </div>
    </main>
    <footer>
        <div id="buttons" class="relative">
            <button class="navitem" @click="validate()">Next</button>
        </div>
    </footer>
</template>

<style scoped>
.image {
    align-items: center;
    display: flex;
    flex-direction: row;
    justify-content: center;
    width: 15%;
    height: auto;
    padding: 0;
}

.identify-box {
    background-color: #fff;
}

.identified {
    box-shadow: 2px 3px 10px 2px #D7DFFF;
    background-color: #6D71FA;
    color: white;
}

.unidentified {
    background-color: #fff;
    color: black;
    box-shadow: 2px 3px 10px 2px #D7DFFF;
}

.incorrect {
    border: 1px solid #CB3434;
}

.correct {
    border: 1px solid #32A962;
}

.flex-between {
    display: flex;
    flex-direction: column;
    margin-left: 10px;
    margin-right: 10px;
    width: auto;
    gap: 10px;
    background-color: #F9FAFE;
}

.drop-zone {
    display: flex;
    /* width: fit-content; */
    min-width: 30ch;
    max-width: 600px;
    margin: 5px;
    height: fit-content;
    /* box-shadow: 2px 3px 10px 2px #D7DFFF; */
    max-height: fit-content;
    min-height: 8vh;
    flex-wrap: wrap;
    flex-direction: column;
    border-radius: 10px;
    padding: 5px;
}


.flex-horizontal {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: flex-start;
    justify-content: space-between;
}

.flex {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    justify-content: space-between;
}

.outer-box {
    padding: 20px;
    background-color: #F9FAFE;
    /* box-shadow: 1px 2px 5px 1px #D7DFFF; */
    max-width: 600px;
    min-width: fit-content;
    border-radius: 10px;
    display: flex;
    flex-direction: column;

}

.drag-el-horizontal {
    padding-left: 2vh;
    padding-right: 2vh;
    border-radius: 5px;
    margin: 5px;
    padding-top: 1vh;
    padding-bottom: 1vh;
    height: fit-content;
    max-width: inherit;
    /* box-shadow: 2px 3px 10px 2px #D7DFFF; */
    cursor: pointer;
}

.drop-zone-horizontal {
    display: flex;
    max-width: 1000px;
    min-width: 700px;
    margin: 10px;
    margin-bottom: 25px;
    box-shadow: 2px 3px 10px 2px #D7DFFF;
    background-color: #F9FAFE;
    min-height: 10vh;
    border-radius: 10px;
    max-height: fit-content;
    overflow: auto;
    flex-wrap: wrap;
    flex-direction: row;
    padding: 10px;
}

.drag-el-horizontal:nth-last-child() {
    margin-bottom: 0;
}

.drag-el:nth-last-child() {
    margin-bottom: 0;
}

.flexbox {
    display: flex;
    justify-content: space-between;

    height: 100%;
    width: 98%;

    overflow: hidden;

    margin: 15px;
    padding: 15px
}

#buttons {
    margin: 0;
    text-align: center;
}

.relative {
    padding: 10px;
    display: flex;
    width: 100%;
    flex-direction: row;
    justify-content: space-between;
}

.navitem {
    width: 100px;
    height: 50px;
    padding: 10px;
    text-align: center;
    border: none;
    background-color: #272971;
    border-radius: 5px;
    color: #fff;
    cursor: pointer;
    font-weight: normal;
    font-size: large;
}

.navitem:hover {
    background-color: #fff;
    color: #272971;
    border: 1px solid #272971;
    cursor: pointer;
    font-weight: bold;
}

#snackbar {
    visibility: hidden;
    min-width: 250px;
    margin-left: -125px;
    background-color: #333;
    color: #fff;
    text-align: center;
    border-radius: 2px;
    padding: 16px;
    position: fixed;
    z-index: 1;
    left: 50%;
    bottom: 30px;
    font-size: 17px;
}

#snackbar.show {
    visibility: visible;
    -webkit-animation: fadein 0.5s, fadeout 0.5s 2.5s;
    animation: fadein 0.5s, fadeout 0.5s 2.5s;
}

@-webkit-keyframes fadein {
    from {
        bottom: 0;
        opacity: 0;
    }

    to {
        bottom: 30px;
        opacity: 1;
    }
}

@keyframes fadein {
    from {
        bottom: 0;
        opacity: 0;
    }

    to {
        bottom: 30px;
        opacity: 1;
    }
}

@-webkit-keyframes fadeout {
    from {
        bottom: 30px;
        opacity: 1;
    }

    to {
        bottom: 0;
        opacity: 0;
    }
}

@keyframes fadeout {
    from {
        bottom: 30px;
        opacity: 1;
    }

    to {
        bottom: 0;
        opacity: 0;
    }
}

.button-30 {
  align-items: center;
  appearance: none;
  background-color: #FCFCFD;
  border-radius: 4px;
  border-width: 0;
  box-shadow: rgba(45, 35, 66, 0.4) 0 2px 4px,rgba(45, 35, 66, 0.3) 0 7px 13px -3px,#D6D6E7 0 -3px 0 inset;
  box-sizing: border-box;
  color: #36395A;
  cursor: pointer;
  display: inline-flex;
  font-family: "JetBrains Mono",monospace;
  height: 48px;
  justify-content: center;
  line-height: 1;
  list-style: none;
  overflow: hidden;
  padding-left: 16px;
  padding-right: 16px;
  position: relative;
  text-align: left;
  text-decoration: none;
  transition: box-shadow .15s,transform .15s;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  white-space: nowrap;
  will-change: box-shadow,transform;
  font-size: 18px;
}

.button-30:focus {
  box-shadow: #D6D6E7 0 0 0 1.5px inset, rgba(45, 35, 66, 0.4) 0 2px 4px, rgba(45, 35, 66, 0.3) 0 7px 13px -3px, #D6D6E7 0 -3px 0 inset;
}

.button-30:hover {
  box-shadow: rgba(45, 35, 66, 0.4) 0 4px 8px, rgba(45, 35, 66, 0.3) 0 7px 13px -3px, #D6D6E7 0 -3px 0 inset;
  transform: translateY(-2px);
}

.button-30:active {
  box-shadow: #D6D6E7 0 3px 7px inset;
  transform: translateY(2px);
}
</style>

