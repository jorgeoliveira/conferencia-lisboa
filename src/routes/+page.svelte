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
        const formfreq = document.getElementById("formfreq").checked
            ? "Sim"
            : "Não";
        formData.append("formfreq", formfreq);
        try {
            const response = await fetch(
                "https://conferenciaapi.cangalho.pt/api/",
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
                        "</div></div>"+data.message);
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
    <div class="header" style="">
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
                    targetDate={new Date("2025-05-05T09:30:00").getTime()}
                />
            </div>
        </div>
    </div>
    <section
        class="d-flex justify-content-center align-items-center text-hero"
        style="height:70vh;"
    >
        <div
            class="container row"
            style="text-align: center;position: sticky;
        top: 20%;"
        >
            <div class="col-12 col-md-12 col-lg-8 d-flex flex-column">
                <h1
                    class="hero"
                    style="line-height: 75%;
                letter-spacing: 0.05em;"
                >
                    WorkShop<br /><span
                        style="font-size: 1rem;line-height:120%;">—<br /></span
                    >
                    <span style="font-size: 1.7rem;line-height:120%;"
                        >Contratação Publica para Iniciantes</span
                    >
                </h1>
            </div>
            <div
                class="col-12 col-md-12 col-lg-4 d-flex align-items-center justify-content-center"
            >
                <p class="hero" style="width:100%;">
                    Numa organização conjunta do Direito Descomplicado, <strong>CANGALHO</strong>
                    e <br />
                    Consórcio <strong>CEACP</strong> e <br />
                    <strong style="white-space:nowrap;">CIAcademy®</strong>
                </p>
            </div>
        </div>
    </section>
    <div class="container-full section-2 bg-pattern" style="">
        <div class="container">
            <div class="row mt-5">
                <div class="col-12 col-md-3 order-2 order-md-1">
                    <div
                        class="card border-0"
                        style="position: sticky;top: 20px;"
                    >
                        <div
                            class="card-body"
                            style="background: linear-gradient(to bottom, #f2f2f2, #ffffff);background: rgba(22, 81, 83, 1);color: #fff !important;padding-top:50px;"
                        >
                            <div class="row row-center-v">
                                <div class="col-12 p-0 m-0">
                                    <h5
                                        style="letter-spacing: 0.1em;
                               
                            "
                                    >
                                        Evento
                                    </h5>
                                </div>
                                <div class="col-4 p-0 m-0">
                                    <!-- svelte-ignore a11y-missing-attribute -->
                                    <img
                                        src="https://www.ceacp.pt/wp-content/uploads/2021/05/clock-1-150x150.png"
                                        style="width: 50px;
                               padding: 2vh 0;mix-blend-mode: plus-lighter;"
                                    />
                                </div>
                                <div class="col-8 p-0 m-0">
                                    <p class="p-calendar">5 de maio 2025</p>
                                    <p class="p-calendar">09:30 - 12:30</p>
                                </div>
                                <div class="col-12 p-0 m-0">
                                    <div
                                        style="width:100%; height:2px;background-color: #999;margin-top: 2vh;"
                                    ></div>
                                    <p
                                        class="p-calendar pt-3"
                                        style="    text-transform: uppercase;
                                        font-weight: 500;
                                        font-size: 13px;mix-blend-mode: plus-lighter;"
                                    >
                                        Plataforma: ZOOM
                                    </p>
                                    <div
                                        style="width:100%; height:2px;background-color: #999;margin-top: 2vh;"
                                    ></div>
                                    <p
                                        class="p-calendar pt-3"
                                        style="    text-transform: uppercase;
                                font-weight: 500;
                                font-size: 13px;filter: brightness(2.5);"
                                    >
                                        Contactos
                                    </p>
                                    <div
                                        class="d-flex mt-2"
                                        style="width: 100%;;"
                                    >
                                        <a
                                            href="mailto:sv@ceacp-ciacademy-cangalho.com"
                                            target="_blank"
                                        >
                                            <img
                                                src="icons8-at-sign-100.png"
                                                width="50px"
                                                alt=""
                                                style="cursor: pointer;filter: brightness(3.5);"
                                            />
                                        </a>
                                        <!--    <a
                                        href="https://www.linkedin.com/company/cia-competence-institute-and-academy/?viewAsMember=true"
                                        target="_blank"
                                    >
                                    <img src="icons8-linkedin-100.png" alt="" style="margin-left:15px;cursor: pointer;width: 35px; margin-top: 7px;filter: brightness(3.5); ">
                                    </a> -->
                                    </div>
                                    <!-- 
                                    <div
                                        style="width:100%; height:2px;background-color: #999;margin-top: 2vh;"
                                    ></div>

                                    <p
                                        class="p-calendar pt-3"
                                        style="    text-transform: uppercase;
                                    font-weight: 500;
                                    font-size: 13px;"
                                    >
                                        Consultar o programa em pdf:
                                    </p>
                                    <div
                                        class="d-flex mt-2"
                                        style="width: 100%;;"
                                    >
                                        <a
                                            href="conferencia-oe2025.pdf"
                                            target="_blank"
                                        >
                                            <img
                                                src="icons8-pdf-64.png"
                                                width="40px"
                                                alt=""
                                                style="cursor: pointer;filter: brightness(4.5);margin-left: 5px;"
                                            />
                                        </a>
                                    </div>
                                      -->
                                    <div
                                        style="width:100%; height:2px;background-color: #999;margin-top: 2vh;"
                                    ></div>
                                    <div
                                        class="he-ro"
                                        style="color:white;font-weight: 500;display:block;font-size: 13px;
                                    text-transform: uppercase;text--align: center;"
                                    >
                                        <br />
                                        Promotores:
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
                                           
                                        </div>
                                        Parceiros:
                                        <div
                                            style="display:flex;justify-content:center;align-items:center;flex-direction: column;text-align:center;margin-top:5vh;"
                                        >
                                            <a href="https://www.ceacp.pt">
                                                <img
                                                    src="/ceacp.png"
                                                    class="img-conf"
                                                    alt="ceacp"
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
                                Workshop | Contratação Pública em Debate:<br />
                                "DESAFIOS E OPORTUNIDADES PARA OS FUTUROS JURISTAS"
                            </h5>
                            <p>
                                A contratação pública está no centro das
                                decisões que moldam a vida coletiva, desde a
                                construção de infraestruturas até à prestação de
                                serviços essenciais. Compreender este domínio é
                                hoje indispensável para qualquer jurista que
                                pretenda atuar com responsabilidade e visão
                                estratégica no setor público ou em assessoria a
                                entidades privadas.
                            </p>

                            <p>
                                Pensado especialmente para estudantes de
                                Direito, este workshop propõe uma abordagem
                                introdutória, acessível e prática à contratação
                                pública, através de uma conversa informal com
                                dois especialistas reconhecidos na área, que
                                partilharão experiências, desafios do dia a dia
                                e conselhos úteis para quem pretende enveredar
                                por esta área profissional.
                            </p>

                            <p>
                                Será uma oportunidade única para discutir, de
                                forma descontraída mas tecnicamente rigorosa,
                                temas como: <br />— os princípios fundamentais
                                do Código dos Contratos Públicos <br />— os
                                erros mais comuns nos Contratos Públicosbr
                                <br />— os desafios da transparência e da
                                concorrência <br />— e as competências jurídicas
                                mais valorizadas no setor.
                            </p>
                            <p>
                                Mais do que uma aula, este workshop será um
                                espaço de diálogo, aproximação à prática e
                                inspiração para quem ambiciona fazer a diferença
                                na gestão pública.
                            </p>
                            <h5 class="title-text tit-sticky">Programa</h5>
                            <table class="table">
                                <tbody>
                                    <tr>
                                        <th scope="row">9h30</th>
                                        <td>
                                            <span style="font-style: italic;">
                                                Abertura e Considerações Gerais
                                                sobre o Atual Quadro da
                                                Transparência – Regime Geral da
                                                Prevenção da Corrupção
                                            </span><br /><br />
                                            <b>Sérgio Fonseca</b> – CANGALHO<br
                                            />
                                            <b>Eduardo Castro Marques</b> – DOWER
                                            LAW FIRM
                                        </td>
                                    </tr>
                                    <tr>
                                        <th scope="row">10h00</th>
                                        <td>
                                            <span style="font-style: italic;">
                                                Entidades Públicas – A boa
                                                gestão pública e os conflitos de
                                                interesses: da implementação à
                                                fiscalização
                                            </span><br /><br />
                                            <b
                                                >Moderador: Carlos José Batalhão</b
                                            >
                                            – Sócio Departamento Público da DOWER
                                            LAW FIRM<br />
                                            <b>Luís Filipe Mota Almeida</b> –
                                            Assessor Jurídico e Parlamentar na
                                            Assembleia da República<br />
                                            <b>Professor Pedro Cruz e Silva</b> –
                                            Professor Universidade Lusófona
                                        </td>
                                    </tr>
                                    <tr>
                                        <th scope="row">11h00</th>
                                        <td>
                                            <span style="font-style: italic;">
                                                Entidades Privadas – Impactos
                                                financeiros: Estão as empresas
                                                portuguesas preparadas para
                                                estas obrigações?
                                            </span><br /><br />
                                            <b>Moderadora: Ana Filipa Urbano</b>
                                            – Sócia Departamento Público da DOWER
                                            LAW FIRM<br />
                                            <b>Miguel Pinto</b> –
                                            Vice-Presidente do Conselho de
                                            Administração da AEP<br />
                                            <b>Mário Tavares da Silva</b> –
                                            Ex-membro do Conselho de Prevenção
                                            da Corrupção. Associado do
                                            Observatório de Economia e Gestão de
                                            Fraude<br />
                                            <b>Isaura Morais</b> – Deputada Assembleia
                                            da República (PSD)
                                        </td>
                                    </tr>
                                    <tr>
                                        <th scope="row">12h00</th>
                                        <td>
                                            <span style="font-style: italic;">
                                                Debate geral / perguntas e
                                                respostas
                                            </span>
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>

            <div class="row d-flex flex-wrap">
                <div class="col-md-12">
                    <h1 style="margin:50px 0 50px 0;position:relative;">
                        Oradores
                    </h1>
                </div>

                <!--- novos -->

                <div class="col-12 col-md-6 col-lg-4 d-flex">
                    <div
                        class="card border-0 rounded-0 mb-5"
                        style="background: linear-gradient(to bottom, #f2f2f2, #ffffff);"
                    >
                        <div class="position-relative">
                            <img
                                src="./sergio-martins.jpg"
                                class="card-img-top img-filter rounded-0"
                                alt="Sérgio Fonseca"
                            />
                            <div class="cut-card">
                                <h5
                                    class="card-title"
                                    style="text-shadow: 2px 4px 0 #000000;"
                                >
                                    Sérgio Fonseca
                                </h5>
                            </div>
                        </div>
                        <div class="card-body">
                            <p class="card-text cargo-desc">
                                Representante da <strong>CANGALHO</strong>
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
                                src="./j-baltasar.jpg"
                                class="card-img-top img-filter rounded-0"
                                alt="Eduardo Castro Marques"
                                
                            />
                            <div class="cut-card">
                                <h5
                                    class="card-title"
                                    style="text-shadow: 2px 4px 0 #000000;"
                                >
                                    Jorge Baltasar
                                </h5>
                            </div>
                        </div>
                        <div class="card-body">
                            <p class="card-text cargo-desc">
                                Consultor em Contratação Pública | <strong>Formador</strong>
                            </p>
                        </div>
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
                        Junte-se a nós nesta jornada, promovendo esclarecimentos
                        sobre o tema:<br />Transparência nas Organizações:<br />
                        OS DESAFIOS DAS ENTIDADES PÚBLICAS E PRIVADAS.
                    </p>
                    <p class="text-intro">
                        Prioridade nas inscrições:<br /><br />
<!--                         &nbsp;&nbsp;— Parceiros e Formandos CEACP & CIAcademy & Cangalho<br
                        />
                        &nbsp;&nbsp;— Parceiros DOWER<br />
                        &nbsp;&nbsp;— Parceiros SUCH <br />
                        &nbsp;&nbsp;— Alunos do Curso de Especialização de Compras<br
                        />
                        &nbsp;&nbsp;— Restantes inscrições<br /> -->
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
                        <div class="col-sm-6">
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
                            <div id="inputPreview2">
                                <input
                                    name="formfreq"
                                    id="formfreq"
                                    type="checkbox"
                                    class="css-checkbox"
                                />
                                <label for="formfreq"></label>
                            </div>
                            <div style="margin-left:15px;">
                                Deseja certificado?<br /><br />
                                O certificado de presença tem um custo de 10€, devendo
                                ser solicitados por email com o comprovativo de pagamento
                                para o seguinte email:<br /><a
                                    href="mailto:sv@ceacp-ciacademy-cangalho.com"
                                    class="mailto"
                                    >sv@ceacp-ciacademy-cangalho.com</a
                                ><br /><br /><i
                                    >(Para os alunos do 10º Curso de
                                    Especialização em Compras e Contratação
                                    Pública o certificado está incluído.)</i
                                ><br /><br />
                                <strong>Dados para pagamento:</strong><br />
                                CIA – Competence Institute and Academy, Uni. Lda.<br
                                />
                                Nipc 514 201 479<br />
                                IBAN
                                <strong>PT50 0007 0000 0075 0034 7502 3</strong
                                ><br />
                            </div>
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
    b,
    strong {
        font-weight: 700;
    }
    p {
        font--size: 0.9rem;
        font-family: "Poppins";
        line-height: 180%;
    }
    .card-img-top {
        transition: transform 0.3s ease-in-out;
        filter: brightness(1.3);
    }
    .img-filter {
    }
    .card:hover .card-img-top {
        transform: translateY(-5px);
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
    }
    .special-btn {
        letter-spacing: 0.1em;
        /* margin-bottom: 5vh; */
        font-family: "Playfair Display", serif;
        line-height: 1.7rem;
        margin: 2rem;
        text-align: center;
        border: 2px solid;
        padding: 1rem;
        max-width: 300px;
        margin: 0 auto;
        margin-top: 3rem;
        margin-bottom: 2rem;
        font-family: "Poppins";
    }
    .tit-sticky {
        position: sticky;
        top: 0px;
        background-color: white;
        width: 100%;
        max-width: 100%;
        padding: 10px 0;
    }
    .cut-card {
        position: absolute;
        bottom: 0px;
        width: 100%;
        background-color: #00000054;
        padding: 10px 20px 15px 20px;
        color: #fff;
        height: 25%;
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
        background-color: #ffffff00;
        border: 1px solid white;
        height: 20vh;
        outline: none;
        caret-color: white;
        padding: 10px;
        color: #fff;
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
        color: white;
        text-underline-offset: 7px;
    }
    @media screen and (max-width: 480px) {
        .img-conf {
            width: 70%;
            margin-bottom: 5vh;
        }
    }
</style>
