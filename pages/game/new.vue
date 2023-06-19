<template>
    <h1>Create a new game</h1>
    <form id="create-new-game" action="/api/new-game">
        <div class="form-group">
            <label for="game-name">Game Name</label>
            <input type="text" id="game-name" name="game-name" />
        </div>

        <input type="hidden" name="userId" value="1" />

        <div class="form-group">
            <input type="submit" value="Create Game" />
        </div>
    </form>
</template>