# Sessão 2: Refatorando código usando o design pattern mais importante na minha opinião - IoC/DI

opa, minha gente,

no nosso segundo encontro sincrono ([gravação aqui](https://drive.google.com/file/d/1C1wBWZGMgz5M-AYLSzN8o0onN_U93SdQ/view)) discutimos sobre o design pattern mais importante nesse mundo backend: **IoC/DI (Inversion of Control e Dependency Injection)** 😎

🎯 Por que eu acho MUITO importante dominá-lo? Por 4 motivos:

1. **adoção e maturidade**: a maioria dos frameworks no mundo Java usam e abusam de IoC/DI, como Spring, Micronaut e Hibernate, e esperam que você desenvolvedor(a) sinta-se confortável com ele;
2. **simplicidade e foco no que importa**: as vantagens oferecidas pelo pattern dentro de um container IoC/DI (como Spring) simplificam a vida ao ponto de os demais patterns serem auxiliares e até menos importantes;
3. **separação de responsabilidade**: ele ajuda a identificar, extrair e distribuir as reponsabilidades na aplicação de forma natural;
4. **testabilidade**: ao distribuir melhor as responsabilidades nós ganhamos de cara código mais testável (testable code);

Para entender seu potencial, na última sessão nós refatoramos um Repository com **muitas responsabilidades** para tirar o máximo proveito da IoC e DI do Springão da massa 💪🏻

O código original do repository foi este (repare na quantidade de código duplicado e boiler-plate):

```java
@Repository
public class LivroRepository {

    public Livro save(Livro livro) {
        EntityManager manager = null;
        EntityTransaction transaction = null;
        try {
            manager = JpaUtils.createNewEntityManager();
            transaction = manager.getTransaction();
            transaction.begin();

            manager.persist(livro);

            transaction.commit();
            return livro;

        } catch (Exception e) {
            if (transaction != null && transaction.isActive()) {
                transaction.rollback();
            }
            throw new RuntimeException(e);
        } finally {
            if (manager != null) {
                manager.close();
            }
        }
    }

    public void delete(Livro antigo) {
        EntityManager manager = null;
        EntityTransaction transaction = null;
        try {
            manager = JpaUtils.createNewEntityManager();
            transaction = manager.getTransaction();
            transaction.begin();

            manager.remove(antigo);

            transaction.commit();
        } catch (Exception e) {
            if (transaction != null && transaction.isActive()) {
                transaction.rollback();
            }
            throw new RuntimeException(e);
        } finally {
            if (manager != null) {
                manager.close();
            }
        }
    }

    public Optional<Livro> findById(Long id) {
        EntityManager manager = null;
        try {
            manager = JpaUtils.createNewEntityManager();
            Livro livro = manager.find(Livro.class, id);

            return Optional
                    .ofNullable(livro);
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            if (manager != null) {
                manager.close();
            }
        }
    }
}
```

E após a refatoração obtivemos o seguinte resultado:

```java
@Repository
@Transactional
public class LivroRepository {

    private EntityManager manager;

    // paramos de resolver nossas dependências e passamos a declará-las, isso é DI (Dependency Injection)
    public LivroRepository(EntityManager manager) {
        this.manager = manager;
    }

    @Transactional // invertemos o controle aqui, o Spring é quem cuida do controle transacional
    public Livro save(Livro livro) {
        return manager.persist(livro);
    }

    public void delete(Livro antigo) {
        manager.remove(antigo);
    }

    @Transactional(readOnly=true)
    public Optional<Livro> findById(Long id) {
        Livro livro = manager.find(Livro.class, id);
        return Optional
                .ofNullable(livro);
    }
}
```

A partir do momento que você entende o poder de IoC/DI você aprende a tirar proveito do pattern para extrair o melhor do seu framework IoC/DI, que no nosso caso foi o Spring 😎

**Tem dúvidas, críticas ou sugestões?** Basta adicionar comentarios nessa thread, assim podemos discutir e aprendermos juntos 💪🏻

Bons estudos e até a próxima sessão! 👊🏻
