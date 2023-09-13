<script setup>
import Chip from './Chip.vue';
import MovieCard from './MovieCard.vue';
import { items } from './movies.json';
import { reactive } from 'vue';
import Rating from './Rating.vue';
import ActionBar from './ActionBar.vue';
import AddMovieModal from './AddMovieModal.vue';
/*
 This is an Icon that you can use to represent the stars if you like
 otherwise you could just use a simple ⭐️ emoji, or * character.
*/
import { StarIcon } from '@heroicons/vue/24/solid';

const movies = reactive(items);

function updateMovieRating(value, index) {
    movies[index].rating = value;
}
</script>

<template>
    <!-- This is where your template goes	-->
    <div class="flex flex-col gap-8 p-8">
        <ActionBar>
            <button class="p-4 bg-yellow-500 rounded font-bold">Add Movie</button>
        </ActionBar>
        <div class="flex gap-8">
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
            </MovieCard>
        </div>
    </div>
    <AddMovieModal />
</template>
<style scoped></style>
