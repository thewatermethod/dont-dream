<template>
    <form id="character-sheet">
        <div class="form-group">
            <label for="name">Character name</label>
            <input type="text" name="name" id="name">
        </div>

        <div class="form-group">
            <label for="playbook">Playbook</label>
            <select name="playbook">

            </select>
        </div>
    </form>
</template>

<style scoped>
form {
    margin: 1rem 0;
}

.form-group {
    background: whitesmoke;
    color: #333;
    display: flex;
    gap: 0.5rem;
    padding: 0.5rem;
}

.form-group input[type="text"] {
    border: 0;
    border-bottom: 2px solid tomato;
    outline: none;
}

.form-group input[type="text"]:focus,
.form-group input[type="text"]:active {
    border-bottom: 3px solid darkblue;
}

@media (min-width: 768px) {
    .form-group input[type="text"] {
        min-width: 33vw;
    }
}
</style>