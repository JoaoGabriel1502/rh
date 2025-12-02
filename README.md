# rh
const fs = require('fs');
const path = require('path');
const readline = require('readline');

// Criar interface de leitura para capturar dados do usuário
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

// Função para solicitar dados ao usuário e salvar no arquivo
function pedirDados() {
  rl.question('Qual é o seu nome? ', (nome) => {
    rl.question('Qual é a sua idade? ', (idade) => {
      rl.question('Qual é o seu telefone? ', (telefone) => {
        rl.question('Qual é o seu endereço? ', (endereco) => {
          rl.question('Qual é a sua profissão? ', (profissao) => {

            // Validar tipos de dados (idade e telefone)
            if (isNaN(idade) || isNaN(telefone)) {
              console.error('Idade e telefone devem ser números válidos.');
              rl.close();
              return;
            }

            // Criar pasta "Currículos" se não existir
            const curriculosPath = path.join(__dirname, 'Currículos');
            if (!fs.existsSync(curriculosPath)) {
              fs.mkdirSync(curriculosPath);
              console.log('Pasta "Currículos" criada com sucesso.');
            }

            // Criar pasta para a profissão, se não existir
            const profissaoPath = path.join(curriculosPath, profissao);
            if (!fs.existsSync(profissaoPath)) {
              fs.mkdirSync(profissaoPath);
              console.log(`Pasta para a profissão "${profissao}" criada com sucesso.`);
            }

            // Criar o arquivo com o nome do profissional
            const arquivoPath = path.join(profissaoPath, `${nome}.txt`);
            const dados = `
            Nome: ${nome}
            Idade: ${idade}
            Telefone: ${telefone}
            Endereço: ${endereco}
            Profissão: ${profissao}
            `;

            // Salvar os dados no arquivo
            fs.writeFile(arquivoPath, dados, (err) => {
              if (err) {
                console.error('Erro ao salvar os dados no arquivo:', err);
                rl.close();
                return;
              }
              console.log(`Currículo de ${nome} salvo com sucesso em ${arquivoPath}`);

              // Perguntar se deseja cadastrar outro candidato
              rl.question('Deseja cadastrar outro profissional? (s/n) ', (resposta) => {
                if (resposta.toLowerCase() === 's') {
                  pedirDados(); // Chamar a função novamente para novo cadastro
                } else {
                  console.log('Cadastro finalizado.');
                  rl.close();
                }
              });
            });
          });
        });
      });
    });
  });
}

// Iniciar o processo de coleta de dados
pedirDados();
