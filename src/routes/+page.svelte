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
    let confNif = "";
    let formfreq = "";
    let recaptchaResponse = "";
    let tematica =
        "Os desafios das compras públicas, em especial o planeamento (Lisboa)";
    let map;
    let mapContainer;

    const apiKey = "1M2CacdtfRUF25amIMDv";

    let m = { x: 0, y: 0 };
    function handleMousemove(event) {
        //m.x = event.clientX + 50;
        //m.y = event.clientY - 90;
    }

    let showWhiteLogo = true;

    function smoothScroll(event) {
        event.preventDefault();
        const targetId = event.currentTarget.getAttribute("href");
        const targetElement = document.querySelector(targetId);
        if (targetElement) {
            targetElement.scrollIntoView({
                behavior: "smooth",
                block: "start"
            });
        }
    }

    onMount(() => {
        function handleScroll() {
            const section2 = document.querySelector(".section-2");
            if (section2) {
                const section2Top = section2.getBoundingClientRect().top;
                showWhiteLogo = section2Top > 100;
            }
        }

        window.addEventListener("scroll", handleScroll);

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

        return () => {
            window.removeEventListener("scroll", handleScroll);
        };
    });

    onDestroy(() => {
        // Any other cleanup can go here.
        // The scroll listener is already cleaned up by the return function in onMount.
    });
    const handleSubmit = async () => {
        const formElement = document.getElementById("registrationForm");
        const formData = new FormData(formElement);

        formData.append("recaptchaResponse", recaptchaResponse);
        const formfreq = document.getElementById("demo_opt_1").checked
            ? "Sim"
            : "Não";
        formData.append("formfreq", formfreq);
        formData.append("tematica", tematica);
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
                        '<div class="row"><div class="col-12"><h5>Inscrição enviada com sucesso!</h5><p>Vai receber um email a confirmar a reserva, caso nao receba, contacte <br>formacao@ceacp.pt</p>' +
                        "</div></div>");
                } else if (
                    data.status == "error01" ||
                    data.status == "error02"
                ) {
                    const rowForm = (document.getElementById(
                        "row-form",
                    ).innerHTML =
                        '<div class="row"><div class="col-12"><h5>Ocorreu um erro</h5><p>Por favor volte a re-carregar a página e tente de novo!</p>' +
                        "</div></div>");
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
    <section class="hero-section">
        <div class="hero-video-background">
            <video
                autoplay
                loop
                muted
                playsinline
                src="https://cursoespecializacao.ceacp.pt/assets/ceacp.mp4"
            ></video>
            <div class="hero-overlay"></div>
        </div>

        <div class="hero-content">
            <div class="hero-logo">
                <!-- Desktop Logo Structure -->
                <div class="logo-desktop-wrapper">
                    <div class="logo-container">
                        <img src="CAPACITAR PARA TRANSFORMAR.png" alt="" style="visibility: hidden; width: 350px;" />
                        <div class="fading-logo-wrapper">
                            <img src="ceacp-cia-new-white.png" class="logo-main" class:visible={showWhiteLogo} alt="Logo White" />
                            <img src="CAPACITAR PARA TRANSFORMAR.png" class="logo-main" class:visible={!showWhiteLogo} alt="Logo Color" />
                        </div>
                    </div>
                </div>
                <!-- Mobile Logo Structure -->
                <div class="logo-mobile-wrapper">
                     <div class="logo-container">
                        <img src="CAPACITAR PARA TRANSFORMAR.png" alt="" style="visibility: hidden; width: 250px;" />
                        <div class="fading-logo-wrapper">
                            <img src="ceacp-cia-new-white.png" class="logo-mobile" class:visible={showWhiteLogo} alt="Logo Mobile White" />
                            <img src="CAPACITAR PARA TRANSFORMAR.png" class="logo-mobile" class:visible={!showWhiteLogo} alt="Logo Mobile Color" />
                        </div>
                    </div>
                </div>
            </div>

            <div class="hero-main-text">
                <h1 class="animate-on-load" style="color:white;">
                    CONFERÊNCIA 2025 LISBOA
                </h1>
                <p class="subtitle animate-on-load">
                    Planear as Compras Públicas e evitar ilegalidades
                </p>
                <p class="sub-subtitle animate-on-load">
                    Como o planeamento gera melhores decisões e resultados no setor público
                </p>
            </div>

            <div class="hero-countdown animate-on-load">
                 <Countdown
                    targetDate={new Date("2025-10-30T10:00:00").getTime()}
                    containerClass="hero-countdown-container"
                    blockClass="hero-countdown-block"
                    numberClass="hero-countdown-number"
                    labelClass="hero-countdown-label"
                />
            </div>

            <a href="#form-section" class="cta-button animate-on-load" on:click={smoothScroll}>Faça a Pré-Inscrição</a>

            <div class="hero-partners animate-on-load">
                <p>Numa organização conjunta:</p>
                <span>CEACP / CIAcademy®</span>
                <span>CANGALHO</span>
                <span>AAFDL</span>
                <span>DOWER</span>
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
                                    <p class="p-calendar">Dia 30 de Outubro</p>
                                    <p class="p-calendar">das 14h30 às 17h</p>
                                </div>
                                <div class="col-12 p-0 m-0">
                                    <div
                                        style="width:100%; height:2px;background-color: #999;margin-top: 2vh;"
                                    ></div>
                                    <p
                                        class="p-calendar pt-3"
                                        style="text-transform: uppercase; font-weight: 500; font-size: 13px;"
                                    >
                                        Local: Lisboa<br />Auditório da
                                        Faculdade de Direito da Universidade de
                                        Lisboa
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
                                            href="mailto:formacao@ceacp.pt"
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
                                                    style="filter: invert(1);width: 80%;"
                                                />
                                            </a>

                                            <a href="https://www.dower.pt/">
                                                <img
                                                    src="/dower-logo.jpeg"
                                                    class="img-conf"
                                                    alt="parceiro cangalho"
                                                    style="margin:0;"
                                                />
                                            </a>

                                            <a href="https://aafdl.pt/">
                                                <img
                                                    src="./logo_aafdl-840x693_1.png"
                                                    class="img-conf"
                                                    style="width:100%"
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
                                CONFERÊNCIA 2025: Planeamento de Compras
                                Públicas: a chave do sucesso
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
                        <div class="table-div">
                            <h5 class="title-text tit-sticky">Programa</h5>
                            <table class="table">
                                <tbody>
                                    <tr>
                                        <th scope="row">14h30</th>
                                        <td> Receção participantes </td>
                                    </tr>
                                    <tr>
                                        <th scope="row">15h00</th>
                                        <td>
                                            Sessão de Abertura<br />
                                            <b>João Pedro Freitas</b> –
                                            Tesoureiro da <b>AAFDL Editora</b>
                                        </td>
                                    </tr>
                                    <tr>
                                        <th scope="row">15h10</th>
                                        <td>
                                            A importância do planeamento de
                                            compras públicas para evitar
                                            ilegalidades<br />
                                            <b>Carlos José Batalhão</b>
                                        </td>
                                    </tr>
                                    <tr>
                                        <th scope="row">15h30</th>
                                        <td>
                                            Compras sustentáveis e planeamento
                                            de compras públicas<br />
                                            <b>Pedro Santos Azevedo</b>
                                        </td>
                                    </tr>
                                    <tr>
                                        <th scope="row">15h50</th>
                                        <td> Coffee-break </td>
                                    </tr>
                                    <tr>
                                        <th scope="row">16h10</th>
                                        <td>
                                            Estratégia Europeia para a
                                            contratação pública e o papel do
                                            planeamento de compras públicas<br
                                            />
                                            <b>Fernando Batista</b>
                                        </td>
                                    </tr>
                                    <tr>
                                        <th scope="row">16h30</th>
                                        <td>
                                            Breves notas “Porque falham as
                                            entidades adjudicantes no
                                            Planeamento de Compras Públicas”<br
                                            />
                                            <b>Paulo Madeira</b>
                                        </td>
                                    </tr>
                                    <tr>
                                        <th scope="row">16h50</th>
                                        <td> Debate </td>
                                    </tr>
                                    <tr>
                                        <th scope="row">17h00</th>
                                        <td> Encerramento </td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>

            <div
                class="container mt-5"
                style="background-color: #f0f0f0;margin: 0 0px 0 0px; padding: 0 25px 25px 25px;background-color: #f0f0f09c;
    box-shadow: 0 4px 12px #00000014;
    border: none !important;border-radius: 5px;padding-right: calc(var(--bs-gutter-x) * 0.5);
    padding-left: calc(var(--bs-gutter-x) * 0.5);"
            >
                <div class="row">
                    <div class="col-12 p-0">
                        <h1 style="margin: 50px 0; position: relative;">
                            Oradores
                        </h1>
                    </div>
                </div>

                <!-- Flex row para ≥ md, empilhado abaixo -->
                <div
                    class="d-flex flex-column flex-md-row align-items-start gap-4"
                    style="padding: 10px;"
                >
                    <!-- Coluna 1: Card limitado a 280px -->
                    <div class="flex-shrink-0 card-mobile" style="">
                        <div class="card border-0 rounded-0">
                            <div class="position-relative">
                                <img
                                    src="./pm-16-9.png"
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
                            <div class="card-body bg-white">
                                <p class="card-text cargo-desc">
                                    Autor do livro <strong
                                        >"Guia Prático - Planeamento de Compras
                                        Públicas"</strong
                                    >, SG do Governo
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
                                ><b
                                    >"Guia Prático - Planeamento de Compras
                                    Públicas"</b
                                ></em
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
                <div class="row d-flex flex-wrap">
                    <div class="col-md-12">
                        <h1
                            style="margin:50px 0 50px 0;position:relative;"
                        ></h1>
                    </div>

                    <!--- novos -->

                    <div class="col-12 col-md-6 col-lg-4 d-flex">
                        <div
                            class="card border-0 rounded-0 mb-5"
                            style="background: linear-gradient(to bottom, #f2f2f2, #ffffff);"
                        >
                            <div class="position-relative">
                                <img
                                    src="./joao_pedro_freitas.jpg"
                                    class="card-img-top img-filter rounded-0"
                                    alt="Pedro Neves de Sousa"
                                    style="filter: brightness(0.8);"
                                />
                                <div class="cut-card">
                                    <h5
                                        class="card-title"
                                        style="text-shadow: 2px 4px 0 #000000;"
                                    >
                                        João Pedro Freitas
                                    </h5>
                                </div>
                            </div>
                            <div class="card-body">
                                <p class="card-text cargo-desc">
                                    Receção | <strong>Moderação</strong>,
                                    Tesoureiro <strong>AFFDL</strong>
                                </p>
                            </div>
                        </div>
                    </div>

                    <div class="col-12 col-md-6 col-lg-4 d-flex">
                        <div
                            class="card border-0 rounded-0 mb-5"
                            style="background: linear-gradient(to bottom, #f2f2f2, #ffffff);"
                        >
                            <div class="position-relative">
                                <img
                                    src="./CarlosJB.jpeg"
                                    class="card-img-top img-filter rounded-0"
                                    alt="Carlos José Batalhão "
                                />
                                <div class="cut-card">
                                    <h5
                                        class="card-title"
                                        style="text-shadow: 2px 4px 0 #000000;"
                                    >
                                        Carlos José Batalhão
                                    </h5>
                                </div>
                            </div>
                            <div class="card-body">
                                <p class="card-text cargo-desc">
                                    Orador | Conselheiro e Formador <strong
                                        >CEACP</strong
                                    >, Advogado <strong>DOWER</strong>
                                </p>
                            </div>
                        </div>
                    </div>
                    <div class="col-12 col-md-6 col-lg-4 d-flex">
                        <div
                            class="card border-0 rounded-0 mb-5"
                            style="background: linear-gradient(to bottom, #f2f2f2, #ffffff);"
                        >
                            <div class="position-relative">
                                <img
                                    src="./fern.jpg"
                                    class="card-img-top img-filter rounded-0"
                                    alt="Carlos José Batalhão "
                                    style="filter: brightness(0.8);"
                                />
                                <div class="cut-card">
                                    <h5
                                        class="card-title"
                                        style="text-shadow: 2px 4px 0 #000000;"
                                    >
                                        Fernando Batista
                                    </h5>
                                </div>
                            </div>
                            <div class="card-body">
                                <p class="card-text cargo-desc">
                                    Orador | Formador <strong>CEACP</strong>,
                                    Presidente do <strong>IMPIC</strong>
                                </p>
                            </div>
                        </div>
                    </div>
                    <div class="col-12 col-md-6 col-lg-4 d-flex">
                        <div
                            class="card border-0 rounded-0 mb-5"
                            style="background: linear-gradient(to bottom, #f2f2f2, #ffffff);"
                        >
                            <div class="position-relative">
                                <img
                                    src="./pedro santos azevedo.jpg"
                                    class="card-img-top img-filter rounded-0"
                                    alt="Carlos José Batalhão "
                                    style="filter: brightness(0.5);"
                                />
                                <div class="cut-card">
                                    <h5
                                        class="card-title"
                                        style="text-shadow: 2px 4px 0 #000000;"
                                    >
                                        Pedro Santos Azevedo
                                    </h5>
                                </div>
                            </div>
                            <div class="card-body">
                                <p class="card-text cargo-desc">
                                    Orador | Formador <strong>CEACP</strong>,
                                    Advogado
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <section class="form-ins" id="form-section">
        <div class="row max-500">
            <div class="col-12 text-intro">
                <h1
                    class=" "
                    style="
            margin-bottom:5vh;  margin-top:20vh;"
                >
                    Formulário de pré-inscrição
                </h1>

                <div
                    class="inscricao-info"
                    style="text-align: left; margin-top: 2rem; padding: 20px;"
                >
                     <h4 style="font-size: 1.3rem; font-weight: 600;">Junte-se a nós!</h4>
                    
                    <h4 style="font-size: 1.3rem; font-weight: 600;">
                        Conferência - Planeamento de Compras Públicas: a chave
                        do sucesso
                    </h4>
                    <p style="margin-top: 1rem;">
                        Como o planeamento gera melhores decisões e resultados
                        no setor público
                    </p>

                    <h5
                        style="font-size: 1.3rem; font-weight: 600; margin-top: 2rem;"
                    >
                        Valor da Inscrição
                    </h5>
                    <ul
                        style="list-style: none; padding-left: 0; margin-top: 1rem;font-size: 0.9rem;line-height: 180%;"
                    >
                        <li
                            style="padding-left: 1.5em; text-indent: -1.5em; margin-bottom: 0.5rem;"
                        >
                            — Formandos CEACP: 30 € (valor já com IVA incluído)
                            -Inscrições Prioritárias
                        </li>
                        <li
                            style="padding-left: 1.5em; text-indent: -1.5em; margin-bottom: 0.5rem;"
                        >
                            — Estudantes da Faculdade de Direito da Universidade
                            de Lisboa (FDUL): 13 €
                        </li>
                        <li
                            style="padding-left: 1.5em; text-indent: -1.5em; margin-bottom: 0.5rem;"
                        >
                            — Outras Inscrições: 30 € (valor já com IVA
                            incluído)
                        </li>
                    </ul>
                    <p style="margin-top: 1rem;">
                        Os formandos CEACP e os associados ATAM beneficiam de
                        prioridade nas inscrições.
                    </p>

                    <h5
                        style="font-size: 1.3rem; font-weight: 600; margin-top: 2rem;"
                    >
                        A inscrição garante:
                    </h5>
                    <ul
                        style="list-style: none; padding-left: 0; margin-top: 1rem;font-size: 0.9rem; line-height: 180%;"
                    >
                        <li
                            style="padding-left: 1.5em; text-indent: -1.5em; margin-bottom: 0.5rem;"
                        >
                            — Acesso integral à Conferência;
                        </li>
                        <li
                            style="padding-left: 1.5em; text-indent: -1.5em; margin-bottom: 0.5rem;"
                        >
                            — Certificado de participação;
                        </li>
                        <li
                            style="padding-left: 1.5em; text-indent: -1.5em; margin-bottom: 0.5rem;"
                        >
                            — Oferta do livro “Guia Prático de Planeamento de
                            Compras Públicas”.
                        </li>
                    </ul>
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
                            <label for="confNif">NIF</label>
                            <input
                                type="text"
                                id="confNif"
                                name="confNif"
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
                        <div class="col-sm-6">
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
                            <div
                                class=" d-flex align-items-center"
                                style="margin-left:15px;"
                            >
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
    /* --- HERO SECTION STYLES --- */
    .hero-section {
        position: relative;
        height: 100vh;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        text-align: center;
        color: #fff;
        overflow: hidden;
    }
    .hero-video-background {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -2;
    }
    .hero-video-background video {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }
    .hero-overlay {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.6);
        z-index: -1;
    }
    .hero-content {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 2rem;
        padding: 2rem;
        max-width: 900px;
    }
    .hero-logo {
        margin-bottom: 1rem;
    }
    .hero-main-text h1 {
        font-size: 3.5rem;
        font-weight: 700;
        letter-spacing: 0.05em;
        text-transform: uppercase;
        margin: 0;
    }
    .hero-main-text .subtitle {
        font-size: 1.5rem;
        font-weight: 300;
        margin-top: 0.5rem;
        opacity: 0.9;
    }
     .hero-main-text .sub-subtitle {
        font-size: 1rem;
        font-weight: 300;
        margin-top: 0.5rem;
        opacity: 0.9;
    }
    .hero-countdown {
        /* This is the outer container for animation delay */
    }
    :global(.hero-countdown-container) {
        display: flex;
        gap: 1.5rem;
        justify-content: center;
    }
    :global(.hero-countdown-block) {
        display: flex;
        flex-direction: column;
        align-items: center;
        min-width: 80px;
    }
    :global(.hero-countdown-number) {
        font-size: 3rem;
        font-weight: 700;
        line-height: 1;
    }
    :global(.hero-countdown-label) {
        font-size: 0.9rem;
        text-transform: uppercase;
        opacity: 0.8;
        margin-top: 0.5rem;
    }
    .cta-button {
        background-color: var(--primary-color, #e0a800); /* A gold-like color */
        color: #000;
        padding: 1rem 2.5rem;
        border-radius: 50px;
        text-decoration: none;
        font-weight: 700;
        font-size: 1.1rem;
        transition: background-color 0.3s ease, transform 0.3s ease;
    }
    .cta-button:hover {
        background-color: #fff;
        transform: translateY(-3px);
    }
    .hero-partners {
        margin-top: 1rem;
    }
    .hero-partners p {
        margin-bottom: 0.5rem;
        opacity: 0.8;
    }
    .hero-partners span {
        margin: 0 0.75rem;
        font-weight: 500;
    }

    /* --- ANIMATIONS --- */
    .animate-on-load {
        opacity: 0;
        transform: translateY(20px);
        animation: fadeInUp 0.8s ease forwards;
    }
    .hero-main-text h1 { animation-delay: 0.2s; }
    .hero-main-text .subtitle { animation-delay: 0.4s; }
    .hero-main-text .sub-subtitle { animation-delay: 0.6s; }
    .hero-countdown { animation-delay: 0.8s; }
    .cta-button { animation-delay: 1s; }
    .hero-partners { animation-delay: 1.2s; }

    @keyframes fadeInUp {
        to {
            opacity: 1;
            transform: translateY(0);
        }
    }

    /* --- FADING LOGO STYLES (from previous step) --- */
    .logo-container {
        position: relative;
        display: inline-block; /* This makes the container behave like an image for layout purposes */
        /* The container is now sized by the invisible image inside it */
    }
    .fading-logo-wrapper {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
    }
    .logo-main {
        position: absolute;
        top: 0;
        left: 0;
        width: 350px;
        opacity: 0;
        transition: opacity 0.5s ease-in-out;
    }
     .logo-mobile {
        position: absolute;
        top: 0;
        left: 0;
        width: 250px;
        opacity: 0;
        transition: opacity 0.5s ease-in-out;
    }
    .logo-main.visible, .logo-mobile.visible {
        opacity: 1;
    }
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
        background-color: transparent;
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
        border: 1px solid #000;
        height: 20vh;
        outline: none;
        caret-color: var(--bs-body-color);
        padding: 10px;
        color: var(--bs-body-color);
    }
    .table-div {
        padding: var(--bs-card-spacer-y) var(--bs-card-spacer-x);
    }
    .table td,
    .table th {
        margin: 10px 0;
        background-color: transparent;
        padding: 10px 0;
        max-width: 600px;
        min-width: 100px;
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
        border: solid 1px var(--bs-body-color);
    }

    .css-checkbox:checked + label {
        background-image: url("data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9Im5vIj8+CjxzdmcKICAgeG1sbnM6ZGM9Imh0dHA6Ly9wdxJybC5vcmcvZGMvZWxlbWVudHMvMS4xLyIKICAgeG1sbnM6Y2M9Imh0dHA6Ly9jcmVhdGl2ZWNvbW1vbnMub3JnL25zIyIKICAgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIgogICB4bWxuczpzdmc9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIgogICB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciCiAgIHZlcnNpb249IjEuMSIKICAgaWQ9InN2ZzIiCiAgIHZpZXdCb3g9IjAgMCAxMi4zMDIgMTIuMzAyIgogICBoZWlnaHQ9IjEyLjMwMiIKICAgd2lkdGg9IjEyLjMwMiI+CiAgPG1ldGFkYXRhCiAgICAgaWQ9Im1ldGFkYXRhMTQiPgogICAgPHJkZjpSREY+CiAgICAgIDxjYzpXb3JrCiAgICAgICAgIHJkZjphYm91dD0iIj4KICAgICAgICA8ZGM6Zm9ybWF0PmltYWdlL3N2Zyt4bWw8L2RjOmZvcm1hdD4KICAgICAgICA8ZGM6dHlwZQogICAgICAgICAgIHJkZjpyZXNvdXJjZT0iaHR0cDovL3B1cmwub3JnL2RjL2RjbWl0eXBlL1N0aWxsSW1hZ2UiIC8+CiAgICAgIDwvY2M6V29yaz4KICAgIDwvcmRmOlJERj4KICA8L21ldGFkYXRhPgogIDxkZWZzCiAgICAgaWQ9ImRlZnMxMiIgLz4KICA8cGF0aCAgICAgZD0iTSAxMS40NjUgMS44MjIgQyAxMS4xMzIgMS40ODkgMTAuNjA5IDEuNDg5IDEwLjI3NSAxLjgyMiBMIDQuNTUgNy41NDcgTCAyLjAzMSA1MDIgQyAxLjY5OCA0LjY2OSAxLjE3NSA0LjY2OSAwLjg0MiA1LjAwMyBDIDAuNTA4IDUuMzM3IDAuNTA4IDUuODU5IDAuODQyIDYuMTkzIEwgMy4zOTMgOC43NDMgQyAzLjcyNyA5LjA3NyA0LjI1IDkuMDc3IDQuNTg0IDguNzQzIEwgMTEuNDY1IDEuODI4MiIgCiAgICAgc3R5bGU9ImZpbGw6IzAwMzM2NjtmaWxsLW9wYWNpdHk6MSIKICAgaWQ9InBhdGg0IiAvPgo8L3N2Zz4K");
    }
    .conference-switch {
        background-color: var(--bs-body-bg);
        color: var(--bs-body-color);
        text-align: center;
        padding: 10px 0;
        border-bottom: 1px solid #ddd;
    }
    .conference-switch p {
        margin: 0;
        font-size: 1rem;
    }
    .conference-switch a {
        font-weight: bold;
        text-decoration: underline;
    }

    /* --- RESPONSIVE STYLES for MOBILE --- */
    /* --- RESPONSIVE LOGO VISIBILITY --- */
    .logo-mobile-wrapper {
        display: none;
    }

    @media (max-width: 768px) {
        .logo-desktop-wrapper {
            display: none;
        }
        .logo-mobile-wrapper {
            display: block;
        }

        .hero-main-text h1 {
            font-size: 2.2rem;
        }
        .hero-main-text .subtitle {
            font-size: 1.1rem;
        }
        .hero-main-text .sub-subtitle {
            font-size: 0.9rem;
        }
        .hero-content {
            gap: 1.5rem;
            padding: 1rem;
        }
        :global(.hero-countdown-container) {
            gap: 1rem;
        }
        :global(.hero-countdown-number) {
            font-size: 2rem;
        }
        :global(.hero-countdown-label) {
            font-size: 0.7rem;
        }
        .cta-button {
            padding: 0.8rem 2rem;
            font-size: 1rem;
        }
        .hero-partners span {
            display: block;
            margin: 0.25rem 0;
        }
    }
</style>
