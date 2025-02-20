<script>
    import BurgerButton from "./BurgerButton.svelte";
    import DropdownButton from "./DropdownButton.svelte";
    import logo from "../../assets/img/logo.png";
</script>

<header class="header">
    <div class="header__left-section">
        <div class="header__burger-button-wrapper"><BurgerButton /></div>
        <img src={logo} alt="Logo" class="header__logo">
        <p class="header__text">Система управления проектной документацией проекта ВСМ-1</p>
    </div>

    <div class="header__right-section">
        <DropdownButton label="Объекты" />
        <DropdownButton label="Регионы" />
        <DropdownButton label="Этапы" />
        <DropdownButton label="Слои" />
    </div>
</header>

<style>
    .header {
        width: 100%;
        height: 100%;
        background: var(--light-bg-color);
        border-bottom: 1px solid var(--accent-color);
        display: flex;
        padding: 0 24px;
        display: flex;
        justify-content: space-between;
        gap: 24px;
    }

    .header__text {
        font-family: "Raleway Semibold";
        font-size: 1em;
    }

    .header__left-section, .header__right-section {
        display: flex;
        align-items: center;
    }

    .header__left-section {
        flex: 0 1 auto;
        gap: 16px;
    }

    .header__right-section {
        flex: 0 0 auto;
        justify-content: end;
    }

    .header__burger-button-wrapper {
        height: 36px;
        width: 36px;
        min-width: 36px;
    }

    .header__logo {
        height: 36px;
        width: 36px;
    }

    @media (max-width: 770px) {
        .header__text {
            display: none;
        }
    }
</style>