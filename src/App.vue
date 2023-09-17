<script setup>
import Chip from './Chip.vue';
import MovieCard from './MovieCard.vue';
import { items } from './movies.json';
import { computed, markRaw, reactive, ref, shallowReactive, shallowRef } from 'vue';
import Rating from './Rating.vue';
import ActionBar from './ActionBar.vue';
import AddMovieModal from './AddMovieModal.vue';

/*
 This is an Icon that you can use to represent the stars if you like
 otherwise you could just use a simple ⭐️ emoji, or * character.
*/
import { StarIcon } from '@heroicons/vue/24/solid';
import { CATEGORIES } from './utils/constants';

const movies = ref(items);
const isModalVisible = ref(false);
const isEditing = ref(false);
const newMovie = ref({
    name: '',
    genres: [],
    description: '',
    image: '',
    inTheaters: false,
    rating: 0,
});
const selectedMovieIndex = shallowRef(-1);

const averageRating = computed(() =>
    (
        movies.value.reduce((total, movie) => {
            total += movie.rating;
            return total;
        }, 0) / movies.value.length
    ).toFixed(2)
);

function editMovie(movie, index) {
    newMovie.value = JSON.parse(JSON.stringify(movie));
    isModalVisible.value = true;
    isEditing.value = true;
    selectedMovieIndex.value = index;
}

function updateMovieRating(value, index) {
    movies.value[index].rating = value;
}

function closeModal() {
    isModalVisible.value = false;
}
function showAddMovieModal() {
    isModalVisible.value = true;
}

function resetRatings() {
    movies.value.forEach((movie) => (movie.rating = 0));
}

function removeMovie(index) {
    movies.value.splice(index, 1);
}

function handleMovieCategoryChange(category) {
    const categoryIndex = newMovie.value.genres.findIndex((value) => value === category);
    if (categoryIndex !== -1) {
        newMovie.value.genres.splice(categoryIndex, 1);
    } else {
        newMovie.value.genres.push(category);
    }
}

function addNewMovie() {
    if (isEditing.value) {
        movies.value[selectedMovieIndex.value] = newMovie.value;
    } else {
        movies.value.push(newMovie.value);
    }

    newMovie.value = {
        name: '',
        description: '',
        genres: [],
        inTheaters: false,
        rating: 0,
    };
    isEditing.value = false;
    isModalVisible.value = false;
    selectedMovieIndex.value = -1;
}
</script>

<template>
    <!-- This is where your template goes	-->
    <div class="flex flex-col gap-8 p-8">
        <ActionBar>
            <button class="p-4 bg-yellow-500 rounded font-bold" @click="showAddMovieModal">
                Add Movie
            </button>
            <button class="p-4 bg-yellow-500 rounded font-bold" @click="resetRatings">
                Reset Ratings
            </button>
        </ActionBar>
        <div class="font-bold text-white">
            Total movies {{ movies.length }} / Average rating: {{ averageRating }}
        </div>
        <div class="flex gap-8 flex-wrap">
            <MovieCard v-for="(movie, index) in movies" class="rounded relative">
                <div
                    class="absolute w-[48px] h-[48px] top-0 right-0"
                    :class="movie.rating ? 'text-amber-300' : 'text-gray-300'">
                    <StarIcon class="absolute star-icon" />
                    <div
                        class="absolute w-[48px] h-[48px] flex items-center justify-center text-black">
                        {{ movie.rating ?? '-' }}
                    </div>
                </div>
                <template v-slot:banner>
                    <img class="w-full h-full rounded" :src="movie.image" />
                </template>
                <div>{{ movie.name }}</div>
                <template v-slot:genre>
                    <Chip v-for="genre in movie.genres" :label="genre" />
                </template>
                <template v-slot:description>
                    <div>{{ movie.description }}</div>
                </template>
                <template v-slot:rating>
                    <div class="flex gap-4 items-center">
                        <span v-if="movie.rating">Rating ({{ movie.rating }} / 5)</span>
                        <span v-else>Rating (N/A)</span>
                        <div class="flex">
                            <Rating
                                :icon="StarIcon"
                                :rating="movie.rating"
                                @changed="updateMovieRating($event, index)" />
                        </div>
                    </div>
                </template>
                <template v-slot:actions>
                    <button
                        class="p-2 bg-yellow-500 rounded font-bold"
                        @click="editMovie(movie, index)">
                        Edit
                    </button>
                    <button class="p-2 bg-yellow-500 rounded font-bold" @click="removeMovie(index)">
                        Remove
                    </button>
                </template>
            </MovieCard>
        </div>
    </div>
    <AddMovieModal :is-visible="isModalVisible">
        <div class="flex flex-col gap-4 p-8">
            <input type="text" placeholder="Name" v-model="newMovie.name" />
            <div>{{ newMovie.genres.join(', ') }}</div>
            <select multiple @change="handleMovieCategoryChange($event.target.value)">
                <option disabled selected>Select a category</option>
                <option
                    v-for="category in CATEGORIES"
                    :key="category"
                    :value="category"
                    :class="{ 'bg-yellow-500': newMovie.genres.includes(category) }">
                    {{ category }}
                </option>
            </select>
            <textarea placeholder="description" v-model="newMovie.description"></textarea>
            <input type="text" placeholder="Banner" v-model="newMovie.image" />
            <div class="flex gap-2">
                <input type="checkbox" v-model="newMovie.inTheaters" /><span>In Theaters</span>
            </div>
        </div>
        <template v-slot:actions>
            <div class="flex">
                <button
                    class="mr-auto ml-8 bg-yellow-500 p-4 rounded w-[100px] font-bold"
                    @click="closeModal">
                    Cancel
                </button>
                <button
                    class="ml-auto mr-8 bg-yellow-500 p-4 rounded w-[100px] font-bold"
                    @click="addNewMovie">
                    {{ isEditing ? 'Edit' : 'Add' }}
                </button>
            </div>
        </template>
    </AddMovieModal>
</template>
<style scoped></style>
