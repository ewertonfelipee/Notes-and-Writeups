# CeWL

CeWL é um gerador de wordlists (listas de palavras) que se destaca em comparação com outras ferramentas disponíveis. Enquanto muitas ferramentas dependem de listas pré-definidas ou ataques comuns baseados em dicionários, o CeWL cria wordlists personalizadas com base no conteúdo de páginas da web. Aqui está o motivo pelo qual o CeWL é único:

1. **Wordlists específicas para o alvo:** O CeWL elabora wordlists especificamente a partir do conteúdo de um site alvo. Isso significa que a lista gerada é automaticamente adaptada ao vocabulário e à terminologia usada naquele site. Essas listas personalizadas podem aumentar a eficiência de tarefas de força bruta.
2. **Profundidade de busca:** O CeWL pode "navegar" por um site até uma profundidade especificada, extraindo palavras não apenas de uma página, mas também de páginas vinculadas até a profundidade definida.
3. **Saídas personalizáveis:** O CeWL oferece várias opções para ajustar a wordlist, como definir um comprimento mínimo de palavras, remover números e incluir meta tags. Esse nível de personalização pode ser vantajoso para atacar credenciais específicas ou vulnerabilidades.
4. **Funcionalidades embutidas:** Embora seu objetivo principal seja a geração de wordlists, o CeWL inclui funcionalidades como enumeração de nomes de usuário a partir de meta tags de autores e extração de e-mails.
5. **Eficiência:** Dada sua personalização, o CeWL pode frequentemente gerar listas mais curtas, mas mais relevantes, em comparação com listas genéricas, tornando os ataques de senhas mais rápidos e precisos.
6. **Integração com outras ferramentas:** Sendo baseado em linha de comando, o CeWL pode ser integrado facilmente em fluxos de trabalho automatizados, e suas saídas podem ser usadas diretamente em outras ferramentas de segurança cibernética.
7. **Ativamente mantido:** O CeWL é mantido e atualizado regularmente. Isso significa que ele permanece relevante e compatível com as necessidades e desafios de segurança atuais.

### **Como Personalizar a Saída para Tarefas Específicas**

O CeWL oferece muitas opções que permitem ajustar a wordlist conforme suas necessidades:

1. **Especificar a profundidade de navegação:** A opção `d` permite definir a profundidade que o CeWL deve explorar. Por exemplo, para navegar dois links profundos: `cewl http://URL -d 2 -w output1.txt`
2. **Definir o comprimento mínimo e máximo das palavras:** Use as opções `m` e `x`, respectivamente. Por exemplo, para obter palavras entre 5 e 10 caracteres: `cewl http://URL -m 5 -x 10 -w output2.txt`
3. **Lidar com autenticação:** Se o site alvo estiver protegido por login, você pode usar o sinalizador `a` para autenticação baseada em formulário.
4. **Extensões personalizadas:** A opção `-with-numbers` adiciona números às palavras, e usar `-extension` permite adicionar extensões personalizadas a cada palavra, sendo útil para força bruta em diretórios ou arquivos.
5. **Seguir links externos:** Por padrão, o CeWL não navega por sites externos, mas usando a opção `-offsite`, você pode permitir que ele o faça.
