<template v-slot:default>
            <div id="page-content-wrapper">
        <Admin-nav/>
                <div class="container-fluid">
        <div class="text-center d-flex justify-content-between align-items-center">
        <h1 class="text-center">Dania</h1>

        <a class="btn btn-dark m-4" role="button" data-bs-toggle="modal" data-bs-target="#AddMealModal">Dodaj nowe</a>           
        </div>
<table class="d-flex flex-column align-items-center table">
  <tbody>
      <thead >
    <tr>
      <th class="el" scope="col"><p>ID</p></th>
      <th class="el" scope="col"><p>Danie</p></th>
      <th class="el" scope="col"><p>Cena</p></th>
      <th class="el" scope="col"><p>Status</p></th>
      <th class="el" scope="col"><p>Edytuj</p></th>
      <th class="el" scope="col"><p>Usuń</p></th>
    </tr>
  </thead>
    <tr 
    v-for="meal in meals"
    :key="meal.MealId"
    >
    <th class="pt-3">{{ meal.MealId}}</th>
    <td class="price pt-3">{{ meal.Name }}</td>
    <td class="pt-3">{{ meal.Price}}zł</td>
    <td class="pt-3">{{ meal.StatusName }}</td>

    <td>
    <img
     src="../../../src/assets/mycollection/png/others/edit.png"
     class="img-fluid mt-2 ms-2" alt="Responsive image"
     style="width:32px"
     v-on:click="() => $router.push({name: 'meals.edit', params: {MealId: meal.MealId}})"
     />
    </td>
    <td>
    <img v-on:click="deleteMeal(meal.MealId)" src="../../../src/assets/mycollection/png/others/trash.png" class="img-fluid mt-2 ms-2" alt="Responsive image" style="width:32px">
    </td>
    </tr>

  </tbody>
</table>
           <div class="modal fade" id="AddMealModal" tabindex="-1" aria-labelledby="AddMealModallabel" aria-hidden="true">
                <div class="modal-dialog">
                    <div class="modal-content">
                        <form @submit.stop.prevent="saveMeal">
                            <div class="modal-header">
                                <h5 class="modal-title" id="AddMealModallabel">Dodanie dania</h5>
                                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                            </div>
                            <div class="modal-body">
                                <div class="form-floating mb-3">
                                    <input type="text" class="form-control" v-model="form.Name" id="Name" placeholder="Nazwa">
                                    <label for="Name">Nazwa</label>
                                </div>
                                <div class="form-floating mb-3">
                                    <input type="text" class="form-control" v-model="form.Price" id="Price" placeholder="Cena">
                                    <label for="Price">Cena</label>
                                </div>
                            <div class="form-group mb-3">
                            <label for="Description">Opis</label>
                            <textarea type="text" class="form-control" v-model="form.Description" id="Description" rows="15"></textarea>
                            </div>
                                <div class="form-floating mb-3">
                                    <select class="form-select" id="Status" v-model="form.Status">
                                        <option value="">-- Wybierz --</option>
                                        <option value="1">Aktywny</option>
                                        <option value="0">Nieaktywny</option>
                                    </select>
                                    <label for="Status">Status</label>
                                </div>
                                <div class="form-floating mb-3">
                                    <select class="form-select" id="Status" v-model="form.Category">
                                       <template v-for="category in categories" :key="category.id">
                                        <option :value="category.id"> {{ category.Name }} </option>
                                       </template>
                                    </select>
                                    <label for="Status">Kategoria</label>
                                </div>
                            </div>
                            <div class="modal-footer">
                              <button type="submit" class="btn btn-success" data-bs-dismiss="modal" >Dodaj</button>
                            </div>
                        </form>
                    </div>
                </div>
           </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import useCategories from "../../composables/Categories.js"
import useMeals from "../../composables/Meals.js"
import AdminNav from "../../components/admin/AdminNav.vue"

import { onMounted, reactive } from "vue"


   export default {
     components: {AdminNav},
        setup() {
             const { categories, getCategories, getThisCategory, category} = useCategories()
            const { meals, getMeals, getThisMeal, meal, storeMeal, errors } = useMeals()
           onMounted(getCategories)
            
            
            const formC = reactive({
                Name: ''
            })

            
            onMounted(getCategories)
           
            onMounted(getMeals)
             const form = reactive({
                Name: '',
                Status: '',
                Price: '',
                Description: '',
                Category: '',
            })

            const saveMeal = async () => {
                await storeMeal({...form});
                await getMeals();
            }

            const deleteMeal = async (MealId) => {
                if(!window.confirm('Czy napewno usunąć tą pozycję?')){
                    return
                }
                
                await axios.delete('/meals/' + MealId);
                await getMeals();
            }
            onMounted(getMeals)
            return {
                formC,
               getThisCategory,
                category,
                categories,
              getThisMeal,
                meal,
                meals,
                errors,
                deleteMeal,
                saveMeal,
                form
            }
        }
    }

</script>

<style>
th.el p {
margin-left: 1vw;
color:black;
font-weight: bold;
}

#page-content-wrapper {
    width: 100%;
}
</style>
