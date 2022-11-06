<script>
export default {
    props: ['title']
}
</script>

<template>
    <div id="header">
        <div id="navbar">
            <h1>{{title}}</h1>
            <ul id="nav-links">
                <li class="nav-item"><a href="student.html">Students</a></li>
                <li class="nav-item"><a href="lab.html">Labs</a></li>
            </ul>
        </div>
        <hr>
    </div>
</template>

<style scoped>
    #navbar {
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: end;
    }

    #nav-links {
        display: flex;
        align-items: center;
        list-style: none;

    }

    #navbar h1 {
        margin-right: auto;
    }

    .nav-item {
        font-size: 1.3rem;
        transition: 0.5s ease;
    }

    .nav-item:hover {
        background-color: var(--vt-c-black-mute);
    }

    .nav-item a {
        display: inline-block;
        padding: 10px 15px;
        text-decoration: none;
        color: var(--vt-c-text-dark-2);
    }

    hr {
        margin: 0 0 2rem 0;
    }
</style>