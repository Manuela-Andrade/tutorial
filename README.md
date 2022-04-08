<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">
    <script src = "js/validacao.js" type = "text/javascript"></script>
    <title>Cadastro</title>
</head>
<body>
<?php include_once 'topo.php'?>

<div class = "container">
<br>
<br>
<form class = "form-group" action = "gravar.php" method = "post" onsubmit = "return validarFormulario()">
    <b>Nome:</b><br>
    <input type = "text" name = "nome" placeholder="Nome" id = "nome" class = "form-control">
    <input type = "text" name = "perfil" value="user" hidden="true">
    </br>
    <b>E-mail:</b><br>
    <input type = "text" placeholder="E-mail" name = "email" id = "email" class = "form-control">
    </br>
    <b>Senha:</b><br>
    <input type="password" placeholder="Senha" id="password" required class = "form-control">
    </br>
    <b>Confirmar Senha:</b><br>
    <input type="password" placeholder="Confirme Senha" id="confirm_password" required class = "form-control">
        </br>
        <b>Cep:</b><br>
        <input name="cep" type="text" id="cep" value="" class = "form-control" size="10" maxlength="9"
               onblur="pesquisacep(this.value);" /></label><br />
        <b>Rua:</b>
        <input name="rua" type="text" id="rua" size="60" class = "form-control"><br/>
        <b>Bairro:</b>
        <input name="bairro" type="text" id="bairro" size="40" class = "form-control"><br />
        <b>Cidade:</b>
        <input name="cidade" type="text" id="cidade" size="40" class = "form-control"><br />
        <b>Estado:</b>
        <input name="uf" type="text" id="uf" size="2" class = "form-control"><br />
        <b>IBGE:</b>
        <input name="ibge" type="text" id="ibge" size="8" class = "form-control"><br />
    <script>
    
    function limpa_formulário_cep() {
            //Limpa valores do formulário de cep.
            document.getElementById('rua').value=("");
            document.getElementById('bairro').value=("");
            document.getElementById('cidade').value=("");
            document.getElementById('uf').value=("");
            document.getElementById('ibge').value=("");
    }

    function meu_callback(conteudo) {
        if (!("erro" in conteudo)) {
            //Atualiza os campos com os valores.
            document.getElementById('rua').value=(conteudo.logradouro);
            document.getElementById('bairro').value=(conteudo.bairro);
            document.getElementById('cidade').value=(conteudo.localidade);
            document.getElementById('uf').value=(conteudo.uf);
            document.getElementById('ibge').value=(conteudo.ibge);
        } //end if.
        else {
            //CEP não Encontrado.
            limpa_formulário_cep();
            alert("CEP não encontrado.");
        }
    }
        
    function pesquisacep(valor) {

        //Nova variável "cep" somente com dígitos.
        var cep = valor.replace(/\D/g, '');

        //Verifica se campo cep possui valor informado.
        if (cep != "") {

            //Expressão regular para validar o CEP.
            var validacep = /^[0-9]{8}$/;

            //Valida o formato do CEP.
            if(validacep.test(cep)) {

                //Preenche os campos com "..." enquanto consulta webservice.
                document.getElementById('rua').value="...";
                document.getElementById('bairro').value="...";
                document.getElementById('cidade').value="...";
                document.getElementById('uf').value="...";
                document.getElementById('ibge').value="...";

                //Cria um elemento javascript.
                var script = document.createElement('script');

                //Sincroniza com o callback.
                script.src = 'https://viacep.com.br/ws/'+ cep + '/json/?callback=meu_callback';

                //Insere script no documento e carrega o conteúdo.
                document.body.appendChild(script);

            } //end if.
            else {
                //cep é inválido.
                limpa_formulário_cep();
                alert("Formato de CEP inválido.");
            }
        } //end if.
        else {
            //cep sem valor, limpa formulário.
            limpa_formulário_cep();
        }
    };

    </script>
    </br>
    <br>
    <input type = "submit" value = "Cadastrar Cliente" class = "btn btn-primary">
    <input type = "reset" value = "Limpar Cadastro" class = "btn btn-danger">
    <!-- <input type = "button" value = "Voltar" class = "btn btn-info" onClick="JavaScript:window.history.back();"/>
    voltar -->


    
    </form>
    <!-- Imprimir às mensagens de erro -->
    <div id = "resposta"></div>
</div>
<?php include_once 'rodape.php'; ?>

</script>
</body>
</html>
