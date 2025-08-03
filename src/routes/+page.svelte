<script>
    // @ts-ignore
    //@ts-nocheck

    import { onMount, onDestroy } from "svelte";
    import Gmap from "$lib/Gmap.svelte";
    import Countdown from "$lib/Countdown.svelte";
    import { openSlider } from "$lib/stores.js";
    import Masonry from "$lib/Masonry.svelte";
    import Cursor from "$lib/Cursor.svelte";

    let name = "";

    let message = "";
    let fname = "";
    let apelido = "";
    let email = "";
    let dtNasc = "";
    let telf = "";
    let cargo = "";
    let org = "";
    let formfreq = "";
    let recaptchaResponse = "";
    let map;
    let mapContainer;

    const apiKey = "1M2CacdtfRUF25amIMDv";

    let m = { x: 0, y: 0 };
    function handleMousemove(event) {
        //m.x = event.clientX + 50;
        //m.y = event.clientY - 90;
    }

    onMount(() => {
        grecaptcha.ready(() => {
            grecaptcha
                .execute("6LfwFSMpAAAAABX_XBgRUsB26EBYotsSZASIQYSF", {
                    action: "submit",
                })
                .then((response) => {
                    recaptchaResponse = response;
                });
        });
        document
            .querySelectorAll("input:not([type='checkbox'])")
            .forEach(function (input) {
                input.addEventListener("focusin", function () {
                    this.parentNode
                        .querySelector("label")
                        .classList.add("active");
                });

                input.addEventListener("focusout", function () {
                    if (!this.value) {
                        this.parentNode
                            .querySelector("label")
                            .classList.remove("active");
                    }
                });
            });
    });

    onDestroy(() => {
        //map.remove();
    });
    const handleSubmit = async () => {
        const formElement = document.getElementById("registrationForm");
        const formData = new FormData(formElement);

        formData.append("recaptchaResponse", recaptchaResponse);
        const formfreq = "";
        formData.append("formfreq", formfreq);
        try {
            const response = await fetch(
                "https://conferenciaapi.cangalho.pt/",
                {
                    method: "POST",
                    mode: "cors",
                    headers: {
                        // You may not need to set Content-Type explicitly for FormData
                        //"Content-Type": "application/x-www-form-urlencoded",
                    },
                    body: formData,
                },
            );
            console.log(response.ok);
            // JSON.stringify(data)
            if (response.ok) {
                const data = await response.json();
                console.log("Form submitted successfully!");
                if (data.status == "success") {
                    const rowForm = (document.getElementById(
                        "row-form",
                    ).innerHTML =
                        '<div class="row"><div class="col-12"><h5>Inscrição enviada com sucesso!</h5><p>Vai receber um email a confirmar a reserva, caso nao receba, contacte <br>sv@ceacp-ciacademy-cangalho.com</p>' +
                        "</div></div>");
                } else if (
                    data.status == "error01" ||
                    data.status == "error02"
                ) {
                    const rowForm = (document.getElementById(
                        "row-form",
                    ).innerHTML =
                        '<div class="row"><div class="col-12"><h5>Ocorreu um erro</h5><p>Por favor volte a re-carregar a página e tente de novo!</p>' +
                        "</div></div>" +
                        data.message);
                }
                console.log(data.status);
            } else {
                console.error("Form submission failed.");
                console.error(response.status);
            }
        } catch (error) {
            console.error("Error submitting form:", error);
        }
    };
</script>

<!-- <video autoplay  loop muted>
    <source src="https://cursoespecializacao.ceacp.pt/assets/ceacp.mp4" type="video/mp4">
    Your browser does not support the video tag.
</video> -->
<div class="blob" style="left:{m.x}px; top:{m.y}px"></div>
<!-- svelte-ignore a11y-no-static-element-interactions 
on:mousemove={handleMousemove}>
<div class="blob" style="left:{m.x}px; top:{m.y}px" ></div>
-->
<div class="wrapper">
    <div class="header">
        <div class="row">
            <div class="col-sm-6">
                <div class="logo">
                    <!-- Your logo image or text goes here -->
                    <img
                        src="logo_gold-2.png"
                        style="width:150px;"
                        alt="Logo"
                    />
                </div>
            </div>
            <div
                class="col-sm-6 d-flex justify-content-end align-items-center"
                style="margin-top: 5vh;padding-right: 9vw!important;"
            >
                <Countdown
                    targetDate={new Date("2025-10-01T10:00:00").getTime()}
                />
            </div>
        </div>
    </div>
    <section
        class="d-flex justify-content-center align-items-center text-hero"
        style="height:70vh;"
    >
        <div
            id="hero"
            class="container row d-flex flex-column justify-content-center align-items-center"
            style="text-align: center;position: sticky;
        top: 20%;"
        >
            <div
                class="col-12 col-md-12 col-lg-8 d-flex flex-column"
                style="width:90%;"
            >
                <h1
                    class="hero"
                    style="line-height: 75%;
                letter-spacing: 0.05em;"
                >
                    CONFERÊNCIA 2025<br /><span
                        style="font-size: 1.6rem;line-height:120%;"
                        >PORTO<br /></span
                    >
                    <p style="font-size: 0.6rem;line-height:120%;">
                        (Presencial)<br />
                    </p>
                    <span style="font-size: 1.7rem;line-height:120%;"
                        >OS DESAFIOS DAS COMPRAS PÚBLICAS EM ESPECIAL O
                        PLANEAMENTO</span
                    >
                </h1>
            </div>
            <div
                class="col-12 col-md-12 col-lg-4 d-flex align-items-center justify-content-cente m-0"
            >
                <p class="hero" style="width:100%;">
                    Numa organização conjunta<br />
                    <strong>Cangalho</strong><br />
                    <strong style="white-space:nowrap;">CIAcademy®</strong>
                </p>
            </div>
        </div>
    </section>
    <div class="container-full section-2 bg-pattern">
        <div id="main-section" class="container">
            <div class="row mt-5 align-items-stretch d-flex">
                <div class="col-12 col-md-3 order-2 order-md-1">
                    <div
                        class="card border-0"
                        style="position: sticky;top: 20px;"
                    >
                        <div
                            class="card-body"
                            style="background: var(--form-color);padding-top:50px;"
                        >
                            <div class="row row-center-v">
                                <div class="col-12 p-0 m-0">
                                    <h5 style="letter-spacing: 0.1em;">
                                        Evento
                                    </h5>
                                </div>
                                <div class="col-4 p-0 m-0">
                                    <!-- svelte-ignore a11y-missing-attribute -->
                                    <img
                                        src="https://www.ceacp.pt/wp-content/uploads/2021/05/clock-1-150x150.png"
                                        style="width: 50px;
                               padding: 2vh 0;"
                                    />
                                </div>
                                <div class="col-8 p-0 m-0">
                                    <p class="p-calendar">Dia 1 de Outubro</p>
                                    <p class="p-calendar">das 10h às 13h</p>
                                </div>
                                <div class="col-12 p-0 m-0">
                                    <div
                                        style="width:100%; height:2px;background-color: #999;margin-top: 2vh;"
                                    ></div>
                                    <p
                                        class="p-calendar pt-3"
                                        style="text-transform: uppercase; font-weight: 500; font-size: 13px;"
                                    >
                                        Local: Porto
                                    </p>
                                    <div
                                        style="width:100%; height:2px;background-color: #999;margin-top: 2vh;"
                                    ></div>
                                    <p
                                        class="p-calendar pt-3"
                                        style="text-transform: uppercase; font-weight: 500; font-size: 13px;"
                                    >
                                        Contactos
                                    </p>
                                    <div
                                        class="d-flex mt-2"
                                        style="width: 100%;"
                                    >
                                        <a
                                            href="mailto:sv@ceacp-ciacademy-cangalho.com"
                                            target="_blank"
                                        >
                                            <img
                                                src="icons8-at-sign-100.png"
                                                width="50px"
                                                alt=""
                                                style="cursor: pointer;"
                                            />
                                        </a>
                                    </div>
                                    <div
                                        style="width:100%; height:2px;background-color: #999;margin-top: 2vh;"
                                    ></div>
                                    <div
                                        class="he-ro"
                                        style="font-weight: 500;display:block;font-size: 13px;
                                    text-transform: uppercase;text-align: center;"
                                    >
                                        <br />

                                        Parceiros:
                                        <div
                                            style="display:flex;justify-content:center;align-items:center;flex-direction: column;text-align:center;margin-top:5vh;"
                                        >
                                            <a href="https://www.cangalho.pt/">
                                                <img
                                                    src="/cangalho-1.png"
                                                    class="img-conf-1"
                                                    alt="parceiro cangalho"
                                                />
                                            </a>
                                            <a
                                                href="https://www.cia-academy.com/"
                                            >
                                                <img
                                                    src="/cia-acad.png"
                                                    class="img-conf"
                                                    alt="parceiro cangalho"
                                                />
                                            </a>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-12 col-md-9 order-2 order-md-1">
                    <div class="card border-0">
                        <div class="card-body">
                            <h5 class="title-text">
                                CONFERÊNCIA 2025: OS DESAFIOS DAS COMPRAS
                                PÚBLICAS, EM ESPECIAL O PLANEAMENTO
                            </h5>
                            <p>
                                A contratação pública é um pilar fundamental da
                                governação e da economia, movimentando vastos
                                recursos e impactando diretamente a qualidade
                                dos serviços prestados aos cidadãos. No entanto,
                                a sua eficácia depende de um planeamento
                                rigoroso e estratégico, uma área que apresenta
                                desafios constantes para as entidades públicas.
                            </p>

                            <p>
                                Esta conferência visa debater os obstáculos e as
                                oportunidades na contratação pública, com um
                                foco especial na importância do planeamento.
                                Analisaremos como um planeamento bem estruturado
                                pode garantir a transparência, a eficiência e a
                                concorrência, prevenindo irregularidades e
                                otimizando o uso dos dinheiros públicos.
                            </p>

                            <p>
                                Teremos a honra de contar com a presença de <strong
                                    >Paulo Madeira</strong
                                >, autor do aclamado livro "Guia Prático -
                                Planeamento de Compras Públicas". A sua vasta
                                experiência e conhecimento na matéria
                                proporcionarão uma visão aprofundada e soluções
                                práticas para os desafios que os profissionais
                                da área enfrentam diariamente.
                            </p>
                            <p>
                                Junte-se a nós para um dia de reflexão e debate
                                sobre o futuro da contratação pública em
                                Portugal.
                            </p>
                        </div>
                    </div>
                </div>
            </div>

            <div
                class="container"
                style="background-color: #f0f0f0;margin: 0 10px 0 10px;"
            >
                <div class="row">
                    <div class="col-12">
                        <h1 style="margin: 50px 0; position: relative;">
                            Oradores
                        </h1>
                    </div>
                </div>

                <!-- Flex row para ≥ md, empilhado abaixo -->
                <div
                    class="d-flex flex-column flex-md-row align-items-start gap-4"
                >
                    <!-- Coluna 1: Card limitado a 280px -->
                    <div class="flex-shrink-0 card-mobile" style="">
                        <div class="card border-0 rounded-0">
                            <div class="position-relative">
                                <img
                                    src="https://www.ceacp-ciacademy.pt/wp-content/uploads/2023/08/pm-209x300.jpg"
                                    class="card-img-top img-filter rounded-0"
                                    alt="Paulo Madeira"
                                />
                                <div class="cut-card">
                                    <h5
                                        class="card-title"
                                        style="text-shadow: 2px 4px 0 #000000;"
                                    >
                                        Paulo Madeira
                                    </h5>
                                </div>
                            </div>
                            <div class="card-body">
                                <p class="card-text cargo-desc">
                                    Autor do livro <strong
                                        >"Guia Prático - Planeamento de Compras
                                        Públicas"</strong
                                    >
                                </p>
                            </div>
                        </div>
                    </div>

                    <!-- Coluna 2: Currículo ocupa o resto -->
                    <div class="flex-grow-1" style="max-width: 800px;">
                        <h4 style="font-weight: bold;">Currículo</h4>
                        <p>
                            Paulo Madeira é especialista em contratação pública
                            com vasta experiência na área das compras públicas
                            estratégicas. É formador em diversas instituições
                            públicas e privadas, tendo contribuído ativamente
                            para a capacitação de técnicos e decisores.
                            <br /><br />
                            É também o autor do livro
                            <em
                                >"Guia Prático - Planeamento de Compras
                                Públicas"</em
                            >, uma obra de referência no setor.
                            <br /><br />
                            Tem desempenhado funções de consultoria em vários organismos
                            da Administração Pública, focando-se na melhoria dos
                            procedimentos de contratação, planeamento estratégico
                            de compras e implementação de boas práticas.
                            <br /><br />
                            A sua abordagem prática e conhecimento aprofundado da
                            legislação e dos desafios do setor fazem dele um orador
                            de excelência.
                        </p>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <section class="form-ins">
        <div class="row max-500">
            <div class="row m-0 p-0">
                <div class="col-12">
                    <h1
                        class=" text-intro"
                        style="
            margin-bottom:5vh;  margin-top:20vh;"
                    >
                        Formulário de inscrição
                    </h1>
                </div>
                <div class="col-12" style="margin: 0vh 0 5vh 0;">
                    <p class=" text-intro">
                        Junte-se a nós para debater os desafios e o futuro da
                        contratação pública.
                    </p>
                </div>
            </div>
            <div id="row-form" class="col-12 form-field">
                <form
                    id="registrationForm"
                    on:submit|preventDefault={handleSubmit}
                >
                    <div class="row m-0 p-0">
                        <div class="col-sm-6">
                            <label for="fname">Nome</label>
                            <input
                                id="fname"
                                type="text"
                                name="fname"
                                required
                                class="cool"
                            />
                        </div>
                        <div class="col-sm-6">
                            <label for="apelido">Apelido</label>
                            <input
                                id="apelido"
                                name="apelido"
                                type="text"
                                class="cool"
                                required
                            />
                        </div>
                        <div class="col-sm-6">
                            <label for="email">Email</label>
                            <input
                                id="email"
                                name="email"
                                type="text"
                                class="cool"
                                required
                            />
                        </div>
                        <div class="col-sm-6">
                            <label for="dtNasc"
                                >Data de Nascimento DD/MM/AAAA</label
                            >
                            <input
                                type="text"
                                class="cool"
                                id="dtNasc"
                                name="dtNasc"
                                required
                            />
                        </div>

                        <div class="col-sm-6">
                            <label for="telf">Contacto Telf.</label>
                            <input
                                id="telf"
                                type="text"
                                name="telf"
                                class="cool"
                                required
                            />
                        </div>
                        <div class="col-sm-6">
                            <label for="cargo">Cargo</label>
                            <input
                                id="cargo"
                                type="text"
                                name="cargo"
                                class="cool"
                                required
                            />
                        </div>
                        <div class="col-sm-12">
                            <label for="org">Organização</label>
                            <input
                                id="org"
                                type="text"
                                name="org"
                                class="cool"
                                required
                            />
                        </div>
                        <div class="col-sm-6" style="display:none;">
                            <label for="entpar"
                                >Ent. Parceira <span style="font-size:0.8rem;">
                                    [Cangalho, Dower ou Outra]</span
                                ></label
                            >
                            <input
                                id="entpar"
                                type="text"
                                name="entpar"
                                class="cool"
                            />
                        </div>
                        <div class="col-sm-12">
                            <p>
                                *Se tiver questões que gostaria de ver
                                selecionadas, use esta caixa de texto
                            </p>
                            <textarea
                                name="questions"
                                id="questions"
                                class="cool txt-area"
                            />
                        </div>

                        <div class="col-sm-12 d-flex" style="margin:0;">
                            <div id="inputPreview">
                                <input
                                    name="cssCheckbox"
                                    id="demo_opt_1"
                                    type="checkbox"
                                    class="css-checkbox"
                                    required
                                />
                                <label for="demo_opt_1"></label>
                            </div>
                            <div style="margin-left:15px;">
                                Li e tomei conhecimento das condições de
                                tratamento dos meus dados pessoais
                            </div>
                        </div>

                        <div id="recaptcha-container"></div>

                        <div class="col-12 d-flex justify-content-end">
                            <button type="submit" class="submit-ceacp"
                                >Enviar inscrição</button
                            >
                        </div>
                        <div class="col-sm-12"></div>
                    </div>
                </form>
            </div>
        </div>
    </section>
    <div class="map-wrap" style="display:none;">
        <a href="https://www.maptiler.com" class="watermark"
            ><img
                src="https://api.maptiler.com/resources/logo.svg"
                alt="MapTiler logo"
            /></a
        >
        <div class="map" bind:this={mapContainer}></div>
    </div>
</div>
{#if $openSlider}
    <Gmap />
{/if}

<style>
    .wrapper {
        color: var(--bs-body-color);
        overflow: hidden;
    }
    b,
    strong {
        font-weight: 700;
    }
    p {
        font-size: 0.9rem;
        font-family: "Poppins";
        line-height: 180%;
    }
    .card-img-top {
        transition: transform 0.3s ease-in-out;
    }
    .card:hover .card-img-top {
        transform: translateY(-5px);
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }
    .tit-sticky {
        position: sticky;
        top: 0px;
        background-color: var(--bs-body-bg);
        width: 100%;
        max-width: 100%;
        padding: 10px 0;
    }
    .cut-card {
        position: absolute;
        bottom: 0px;
        width: 100%;
        background-color: rgba(0, 0, 0, 0.4);
        padding: 10px 20px 15px 20px;
        color: #fff;
        min-height: 10px;
        display: flex;
        align-items: center;
    }
    .card-title {
        margin: 0;
        font-weight: 300;
    }

    .txt-area,
    .txt-area:focus {
        width: 100%;
        background-color: transparent;
        border: 1px solid var(--bs-border-color);
        height: 20vh;
        outline: none;
        caret-color: var(--bs-body-color);
        padding: 10px;
        color: var(--bs-body-color);
    }
    .table td,
    .table th {
        margin: 10px 0;
    }

    .img-conf {
        width: 80%;
        margin-bottom: 5vh;
    }
    .img-conf-1 {
        width: 90%;
        margin-bottom: 5vh;
    }
    .mailto {
        color: var(--bs-body-color);
        text-underline-offset: 7px;
    }

    .form-ins {
        background-color: #f0f0f0;
        padding: 5vh 5vw;
    }

    .cool {
        color: var(--bs-body-color);
        background-color: #f0f0f0;
        border: 1px solid #000000;
    }

    .submit-ceacp {
        color: black;
        border: 1px solid black;
    }

    #inputPreview {
        border: solid 2px var(--bs-body-color);
    }

    .css-checkbox:checked + label {
        background-image: url("data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9Im5vIj8+CjxzdmcKICAgeG1sbnM6ZGM9Imh0dHA6Ly9wdxJybC5vcmcvZGMvZWxlbWVudHMvMS4xLyIKICAgeG1sbnM6Y2M9Imh0dHA6Ly9jcmVhdGl2ZWNvbW1vbnMub3JnL25zIyIKICAgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIgogICB4bWxuczpzdmc9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIgogICB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciCiAgIHZlcnNpb249IjEuMSIKICAgaWQ9InN2ZzIiCiAgIHZpZXdCb3g9IjAgMCAxMi4zMDIgMTIuMzAyIgogICBoZWlnaHQ9IjEyLjMwMiIKICAgd2lkdGg9IjEyLjMwMiI+CiAgPG1ldGFkYXRhCiAgICAgaWQ9Im1ldGFkYXRhMTQiPgogICAgPHJkZjpSREY+CiAgICAgIDxjYzpXb3JrCiAgICAgICAgIHJkZjphYm91dD0iIj4KICAgICAgICA8ZGM6Zm9ybWF0PmltYWdlL3N2Zyt4bWw8L2RjOmZvcm1hdD4KICAgICAgICA8ZGM6dHlwZQogICAgICAgICAgIHJkZjpyZXNvdXJjZT0iaHR0cDovL3B1cmwub3JnL2RjL2RjbWl0eXBlL1N0aWxsSW1hZ2UiIC8+CiAgICAgIDwvY2M6V29yaz4KICAgIDwvcmRmOlJERj4KICA8L21ldGFkYXRhPgogIDxkZWZzCiAgICAgaWQ9ImRlZnMxMiIgLz4KICA8cGF0aCAgICAgZD0iTSAxMS40NjUgMS44MjIgQyAxMS4xMzIgMS40ODkgMTAuNjA5IDEuNDg5IDEwLjI3NSAxLjgyMiBMIDQuNTUgNy41NDcgTCAyLjAzMSA1MDIgQyAxLjY5OCA0LjY2OSAxLjE3NSA0LjY2OSAwLjg0MiA1LjAwMyBDIDAuNTA4IDUuMzM3IDAuNTA4IDUuODU5IDAuODQyIDYuMTkzIEwgMy4zOTMgOC43NDMgQyAzLjcyNyA5LjA3NyA0LjI1IDkuMDc3IDQuNTg0IDguNzQzIEwgMTEuNDY1IDEuODI4MiIgCiAgICAgc3R5bGU9ImZpbGw6IzAwMzM2NjtmaWxsLW9wYWNpdHk6MSIKICAgaWQ9InBhdGg0IiAvPgo8L3N2Zz4K");
    }
</style>
