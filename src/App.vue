    <template>
        <div class="form">            
            <form @submit.prevent="submit">
                <label for="city">Город:</label>
                <select @change="updateWorkshops" v-model="selectedCity">
                    <option value="" selected disabled>Выберите город</option>
                    <option v-for="city in cities" :key="city" :value="city">{{ city.name }}</option>
                </select>
    
                <label for="workshop">Цех:</label>
                <select @change="updateEmployees" v-model="selectedWorkshop">
                    <option value="" selected disabled>Выберите цех</option>
                    <option v-for="workshop in workshops" :key="workshop" :value="workshop">{{ workshop.name }}</option>
                </select>
    
                <label for="employee">Сотрудник:</label>
                <select  v-model="selectedEmployee">
                    <option value="" selected disabled>Выберите сотрудника</option>
                    <option v-for="employee in employees" :key="employee" :value="employee">{{ employee.name }}</option>
                </select>
                
                <label for="brigade">Бригада:</label>
                <select @change="updateShift"  v-model="selectedBrigade">
                    <option value="" selected disabled>Выберите бригаду</option>
                    <option value="shift1">{{ brigade }}</option>
                    <!-- <option value="shift2">Смена 2 (20:00 - 8:00)</option> -->
                </select>
    
                <label for="shift">Смена:</label>
                <select v-model="selectedShift">
                    <option value='' selected disabled>Выберите смену</option>
                    <option v-for="shift in shifts" :key="shift" :value="shift">{{ shift }}</option>
                </select>
                <button type="submit">Отправить</button>
            </form>
        </div>
    </template>
<script>
import { useCookies } from "vue3-cookies";

export default {
    setup() {
        const { cookies } = useCookies();
        return { cookies };
    },
data() {
    return {
        selectedCity: '',
        selectedWorkshop: '',
        selectedEmployee: '',
        selectedBrigade: '',
        selectedShift: '',
        "cities": [
                {
                    "id": 1,
                    "name": "Москва"
                },
                {
                    "id": 2,
                    "name": "Санкт-Петербург"
                }
            ],
        "workshopsData": [
                {
                    "workshop_id": 1,
                    "name": "Цех 1",
                    "city_id": 1
                },
                {
                    "workshop_id": 2,
                    "name": "Цех 2",
                    "city_id": 1
                },
                {
                    "workshop_id": 3,
                    "name": "Цех 3",
                    "city_id": 2
                },
                {
                    "workshop_id": 4,
                    "name": "Цех 4",
                    "city_id": 2
                }
            ],
        "employeesData": [
                {
                    "name_id": 1,
                    "workshop_id": 1,
                    "name": "Андрей"
                },
                {
                    "name_id": 2,
                    "workshop_id": 1,
                    "name": "Сергей"
                },
                {
                    "name_id": 3,
                    "workshop_id": 2,
                    "name": "Николай"
                },
                {
                    "name_id": 4,
                    "workshop_id": 3,
                    "name": "Анатолий"
                },
                {
                    "name_id": 5,
                    "workshop_id": 4,
                    "name": "Петр"
                },
                {
                    "name_id": 6,
                    "workshop_id": 4,
                    "name": "Владимир"
                }
            ],
        "brigadeData": {
                1: "первая с 8 до 20:00",
                2: "вторая с 20:00 до 8:00"
            }
            ,
        "shiftsData": {
                "1": "первая",
                "2": "вторая"
            }
        }
    },
     methods: {
        
        submit(){
            if (this.selectedCity&&this.selectedWorkshop&& this.selectedEmployee&&this.selectedShift) {
                console.log("запрос отправлен");                
                const selected =[JSON.stringify(this.selectedCity) , JSON.stringify(this.selectedWorkshop) , JSON.stringify(this.selectedEmployee) , JSON.stringify(this.selectedShift), JSON.stringify(this.selectedBrigade)]
                this.cookies.set("selected", selected, 60)
            } else {
                console.log("заполните все опции");
            }
        },
        updateWorkshops() {
            this.selectedWorkshop = '';
            this.selectedEmployee = '';
        },
        updateEmployees() {
            this.selectedEmployee = '';
        }        
    },
    computed: {
        workshops() {
            if(this.selectedCity.id){
                return this.workshopsData.filter(workshop => workshop.city_id === parseInt(this.selectedCity.id)) || [];
            }
            return this.workshopsData;        },
        employees() {   
            if(this.selectedWorkshop.workshop_id)  {
                return this.employeesData.filter(employee => employee.workshop_id === parseInt(this.selectedWorkshop.workshop_id)) || this.employeesData;
            }    
            return this.employeesData;
        },
        brigade() {
            const date = new Date().getHours();
            if (date >= 8 && date < 20) {
                return this.brigadeData[1] || [];
            } else {
                return this.brigadeData[2] || [];
            }
        },
        shifts() {
            return this.shiftsData || [];
        }
    }
}

</script>
<style scoped>
.form {
    max-width: 1024px;
    display: flex;
    justify-content: center;
}

.form form {
    position: relative;
    background-color: rgba(119, 199, 123, 0.24);
    border-radius: 20px;
    padding: 20px;
    display: flex;
    flex-direction: column;
    align-items: start;
    gap: 10px;
}
</style>
