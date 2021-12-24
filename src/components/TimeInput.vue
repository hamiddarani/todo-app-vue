<template>
  <input v-model="time" ref="input"/>
</template>

<script>
export default {
    name : "Test",
    props: ['value'],
    computed: {
        time : {
            get(){
                return this.value
            },
            set(val){
                this.$emit('input' , val)
            }
        },
    },
    mounted(){
        this.$refs.input.addEventListener('focusout' , this.changePatternTime)
    },
    methods: {
        changePatternTime(){
            if(String(this.time.split(':')[0].length === 1) && !this.time.split(':')[1] && this.time.split(':')[0] < 3){
                this.time = '0' + this.time.split(':')[0] + ':00'
            }else if(String(this.time).split(':')[0].length === 1 && String(this.time).split(':')[1].length === 1 && this.time.split(':')[0] < 3 && this.time.split(':')[0] < 9){
        
                console.log('we are here')
                this.time = '0' + this.time.split(':')[0] + ':0' + this.time.split(':')[1]
            }else if(!String(this.time.split(':')[0]).length){
                this.time = '00:' + this.time.split(':')[1]
            }else if(String(this.time.split(':')[0]).length < 2 && String(this.time.split(':')[0]).length > 0){
                this.time = '0' + this.time.split(':')[0] + ':' + this.time.split(':')[1]
            }else if(!String(this.time.split(':')[1]).length){
                this.time = this.time.split(':')[0] + ':00'
            }
            else if(String(this.time.split(':')[1]).length && String(this.time.split(':')[1]).length < 2){
                this.time = this.time.split(':')[0] + ':0' + this.time.split(':')[1]
            }
        }
    },
    watch: {
            time(newVal , oldVal){
                
               
                this.time = String(newVal).replace(/[^0-9:]+/g , '0')                

                if(newVal.split(':')[1] && newVal.split(':')[1] > 59 ){
                    this.time = newVal.split(':')[0] + ':00'
                }
                 if(newVal.split(':')[1] && newVal.split(':')[1].length === 1 && newVal.split(':')[1] > 5 ){
                    this.time = newVal.split(':')[0] + ':0' + newVal.split(':')[1]
                }
                const pattern1 = /[0-9]+/g
                const pattern2 = /^[1-2][0-9]$/
                
                // console.log(pattern2.test(String(newVal)[1]))
                if(newVal > 23){
                    if(oldVal < 23){
                        this.time = oldVal + ':' + String(newVal)[String(newVal).length -1]
                    }else{
                        this.time = '00:'
                    }
                }

                if(pattern2.test(newVal) && newVal < 24){
                    if(String(newVal.length) >  String(oldVal.length)){
                        this.time = newVal + ':'
                    }
                }

                if(pattern1.test(String(newVal)[0]) && String(newVal)[0] > 2){
                    this.time = '0' + String(newVal)[0] + ':'
                }
                if(pattern1.test(String(newVal)[1]) && String(newVal)[0] > 0 && String(newVal)[0] <2 ){
                    console.log('we are here')
                    this.time = String(newVal) + ':'
                }
                if(String(newVal).length === 1 && newVal === ':'){
                    this.time = '00:'
                }
                if(String(newVal).split(':')[1]?.length === 2 && this.time.length === 4){
                    console.log('sdsdadda')
                    // TODO
                    this.time = String(newVal).split(':')[0] + ':' + String(newVal).split(':')[1]
                }
                if(String(newVal).length > 5){
                    this.time = oldVal
                }
            }
        }
}
</script>
