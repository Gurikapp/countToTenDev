<header></header>

<main>
	<slot />
</main>

<footer></footer>

<style lang="scss">
    @import '/src/routes/nullstyle.scss';
    main{
        background-image: url('/images/design/TimelineBack.png');
        background-repeat: no-repeat;
        height: 200vh;
    }
</style>