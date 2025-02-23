# CeWL: Gerador de Wordlists Personalizadas

CeWL é um gerador de wordlists (listas de palavras) que se destaca em comparação com outras ferramentas disponíveis. Enquanto muitas ferramentas dependem de listas pré-definidas ou ataques comuns baseados em dicionários, o CeWL cria wordlists personalizadas com base no conteúdo de páginas da web. Aqui está o motivo pelo qual o CeWL é único:

## 🔍 Principais Características

1. **Wordlists específicas para o alvo**  
   O CeWL elabora wordlists especificamente a partir do conteúdo de um site alvo. Isso significa que a lista gerada é automaticamente adaptada ao vocabulário e à terminologia usada naquele site. Essas listas personalizadas podem aumentar a eficiência de tarefas de força bruta.

2. **Profundidade de busca**  
   O CeWL pode "navegar" por um site até uma profundidade especificada, extraindo palavras não apenas de uma página, mas também de páginas vinculadas até a profundidade definida.

3. **Saídas personalizáveis**  
   O CeWL oferece várias opções para ajustar a wordlist, como definir um comprimento mínimo de palavras, remover números e incluir meta tags. Esse nível de personalização pode ser vantajoso para atacar credenciais específicas ou vulnerabilidades.

4. **Funcionalidades embutidas**  
   Além da geração de wordlists, o CeWL também pode enumerar nomes de usuários a partir de meta tags de autores e extrair e-mails.

5. **Eficiência**  
   Com sua personalização, o CeWL pode gerar listas mais curtas e relevantes em comparação com listas genéricas, tornando os ataques de senha mais rápidos e precisos.

6. **Integração com outras ferramentas**  
   Por ser uma ferramenta de linha de comando, o CeWL pode ser facilmente integrado a fluxos de trabalho automatizados, e suas saídas podem ser usadas em outras ferramentas de segurança cibernética.

7. **Ativamente mantido**  
   O CeWL é mantido e atualizado regularmente, garantindo relevância e compatibilidade com os desafios de segurança atuais.

---

## 🎯 Como Personalizar a Saída para Tarefas Específicas

O CeWL permite diversas personalizações para ajustar a wordlist às suas necessidades específicas:

### 🔗 Definir profundidade de navegação
Use a opção `-d` para definir a profundidade de exploração do site. Por exemplo, para navegar até 2 níveis de profundidade:

```bash
cewl http://URL -d 2 -w output1.txt
