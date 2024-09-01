## Desafio ScriptBash 
Sistemas Computacionais - UniSenai-SC

Aluno: Mauricio Oliveira Neres - 1794941

## Pré-requisitos
-  Máquina com sistema operacional LINUX
-  Editor de texto para criar scripts BASH

## Cenário Proposto
Parte 1 - Gerenciamento de permissões de diretórios

Crie um script chamado manage_permissions.sh que:

• Receba dois argumentos: o nome do projeto e uma lista de usuários.

• Crie um grupo com o nome do projeto, se ainda não existir.

• Crie um diretório dentro de /shared com o nome do projeto, se ainda não existir.

• Adicione os usuários especificados ao grupo do projeto.

• Defina as permissões do diretório do projeto para que apenas os membros do grupo possam acessar, criar e modificar arquivos dentro dele.

Parte 2 - Relatório diário de arquivos modificados

Crie um script chamado daily_report.sh que:

• Gere um relatório com a lista de todos os arquivos dentro de /shared que foram criados ou modificados nas últimas 24 horas.

• Inclua no relatório o caminho completo de cada arquivo e o nome do subdiretório do projeto a que pertence.

• Salve o relatório em um arquivo chamado report_YYYY-MM-DD.txt no diretório /var/reports, onde YYYY-MM-DD corresponde à data atual.

## Código da Parte 1
Foi criado um diretório /shared para que fosse possível os novos diretórios dos novos processos da empresa.

O código foi utilizado para otimizar a empresa no processo da criação de diretórios para novos projetos, para adicionar corretamente os usuários aos grupos sem ter um trabalho exaustivo e com possibilidade de falhas. O script automatiza todo o processo e trás mais segurança para a empresa, pois uma vez bem configurado basta usar do Script que tudo sai corretamente.

Antes, para você editar/criar um script em BASH você deve utilizar o seguinte comando:
```bash
vim script1.sh
```
Para torná-lo executável pelos usuários deve-se usar o comando:
```bash
chmod +x script1.sh
```
E então, em um caso mais geral o usuário executa o Script em casos mais gerais com o comando:
```bash
./script1.sh
```
claro, para executar o comando o usuário deve estar dentro do diretório /shared criado, ou especificar o caminho para realizar o comando.

Segue o código da Parte 1 (está comentado para melhor entendimento do funcionamento do código):
```bash
#!/bin/bash
#funções para o funcionamento do codigo:
usuario() { #faz uma verificação para ver se o usuario ja existe
	id "$1" &>/dev/null
}
diretorio() { #faz uma verificação para ver se o diretorio existe
	if [ ! -d "$1" ]; then 
	sudo mkdir -p "$1" #se nao existir é criado
	echo "Diretório '$1' criado com sucesso."
else 
	echo "Diretório '$1' já existe."
fi
}	
grupo(){  #faz uma verificação para ver se o grupo ja existe
	if getent group "$1" &>/dev/null; then
	echo "Grupo '$nome_projeto' já existe."
else   #se nao existir é criado o grupo
	sudo groupadd "$nome_projeto" 
	echo "Grupo '$nome_projeto' criado com sucesso."
fi	
}
#fim das funções
#-----------------------------------------------------------------------------------
if [ "$#" -lt 2 ]; then #foi passado dois ou mais atributos para o uso do script, sendo um o nome do projeto, e o outro atributo(s) a lista de N usuarios a ser adicionado
	echo "Uso: $0 <nome_projeto> <usuario1 usuario2 ... usuarioN>"
	exit 1 
fi
#atribuição dos nomes as respectivas variaveis
nome_projeto=$1
diretorio "$nome_projeto"
grupo "$nome_projeto"

shift
lista_usuarios="$@"
echo "Nome do Projeto: $nome_projeto"
echo "Lista de Usuários: $lista_usuarios"

for usuario in $lista_usuarios; do #verifica a lista de usuarios quais existem e quais nao para adicionar no grupo do projeto
	if usuario "$usuario"; then
	echo "Usuário '$usuario' existe."
	sudo usermod -aG "$nome_projeto" "$usuario"
        echo "Usuário '$usuario' adicionado ao grupo '$nome_projeto'."
    else
        echo "Usuário '$usuario' não existe."
    fi
done
#associa grupo ao diretorio pelo nome
sudo chown :$nome_projeto "$nome_projeto"
sudo chmod 770 "$nome_projeto" #define as permissões para o adm e o grupo ter acesso, ler e escrever no diretorio

```
Todos os testes com o código foram realizados 
