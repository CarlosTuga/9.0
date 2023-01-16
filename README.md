# PS4 9.00 Kernel Exploit
---
## Resumo
Neste projeto você encontrará uma implementação que tenta aproveitar um bug do sistema de arquivos do Playstation 4 no firmware 9.00.
O bug foi encontrado ao diferenciar os kernels 9.00 e 9.03. Isso exigirá uma unidade com um sistema de arquivos exfat modificado. Ativá-lo com sucesso permitirá que você execute código arbitrário como kernel, para permitir o jailbreak e modificações no nível do kernel no sistema. iniciará o inicializador de carga normal (na porta 9020).
## Patches Incluídos
Os seguintes patches são aplicados ao kernel:
1) Permitir mapeamento de memória RWX (leitura-gravação-execução) (mmap / mprotect)
2) Instrução syscall permitida em qualquer lugar
3) Resolução Dinâmica (`sys_dynlib_dlsym`) permitida a partir de qualquer processo
4) Chamada de sistema personalizada #11 (`kexec()`) para executar código arbitrário no modo kernel
5) Permita que usuários não privilegiados chamem `setuid(0)` com sucesso. Funciona como uma verificação de status, funciona como uma escalação de privilégios.
6) (`sys_dynlib_load_prx`) correção
7) Desativa o sysVeri
## Short how-to
Essa exploração é diferente das anteriores, em que eram baseadas exclusivamente em software. Acionar a vulnerabilidade requer conectar um dispositivo USB especialmente formatado no momento certo. No repositório você encontrará um arquivo .img. Você pode gravar este .img em um USB usando algo como Win32DiskImager.

**Observação: isso limpará a unidade USB, verifique se você selecionou a unidade correta e se está tudo bem com ela antes de fazer isso**

![](https://i.imgur.com/qpiVQGo.png)

Ao executar o exploit no PS4, espere até que apareça um alerta com "Insira o USB agora. Não feche a caixa de diálogo até que a notificação apareça, remova o USB após fechá-lo". Conforme indicado na caixa de diálogo, insira o USB e aguarde até que a notificação "formato de disco não suportado" apareça e, em seguida, feche o alerta com "OK".

Pode levar um minuto para a exploração ser executada e a animação giratória na página pode congelar - tudo bem, deixe continuar até que um erro seja exibido ou seja bem-sucedido e exiba "Aguardando carga útil".

## Notas Extras
- Desconecte o USB antes de um ciclo de (re)inicialização ou você correrá o risco de corromper a pilha do kernel na inicialização.
- O navegador pode tentá-lo a fechar a página prematuramente, não faça isso.
- O círculo de carregamento pode congelar enquanto o exploit do webkit está sendo ativado, isso ainda não significa que o exploit falhou.
- O bug é anterior ao firmware 1.00, portanto, 1.00-9.00 deve ser explorável usando a mesma estratégia (você precisará de um exploit e gadgets de usuário diferente).
- Você pode substituir o carregador por uma carga útil específica para carregar coisas diretamente, em vez de fazê-lo por meio de soquetes.
- Este bug funciona em certos firmwares PS5, porém não há nenhuma estratégia conhecida para explorá-lo no momento. Usar esse bug contra o PS5 blind não seria aconselhável.
- Por favor, não abra questões para me dizer que não há nenhuma... nem tente me obrigar a fazer o dever de casa para você.
- Este repositório não fornece nada além dos patches iniciais do kernel que permitem executar cargas úteis.
Se você encontrar problemas com determinadas cargas úteis, informe seus problemas aos desenvolvedores dessas cargas úteis por qualquer meio que eles disponibilizem para você.
- O nome do repositório é uma fusão das palavras 'ps4' e '[OOB](https://cwe.mitre.org/data/definitions/787.html)', sendo esta última o tipo de vulnerabilidade desta implementação tentativas de explorar, qualquer outra interpretação é mera coincidência e não intencional.
- Como afirmado anteriormente, este bug foi encontrado ao diferenciar os kernels 9.00 e 9.03, isso implica que o bug foi corrigido no 9.03.