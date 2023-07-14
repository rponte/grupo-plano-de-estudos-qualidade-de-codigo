# Sessão 1: Refatorando código procedural para OO + fail fast

opa, minha gente,

Nos últimos 30min da nossa primeira sessão sincrona ([gravação aqui](https://drive.google.com/file/d/14pmbKthjdq7sqPp4o9frBeiGbuyksNie/view)), eu apresentei o código de um endpoint de API REST escrito em Java e Spring Boot. O código apesar de funcional e coberto por uma bateria de testes com 100% de cobertura apresentava um código procedural e de dificil leitura e manutenção pelo excesso de `if-else's`.

O código original do controller foi este:

```java
@RestController
public class CancelarIngressoController {

    @Autowired
    private IngressoRepository repository;

    @Transactional
    @PostMapping("/api/v1/ingressos/{id}/cancelar")
    public ResponseEntity<?> cancelar(@PathVariable("id") Long id) {

        Optional<Ingresso> ingressoOpt = repository.findById(id);
        if (ingressoOpt.isPresent()) {

            Ingresso ingresso = ingressoOpt.get();
            if (ingresso.getStatus() == Status.NAO_CONSUMIDO) {

                if (isDentroDoPrazoDeCancelamento(ingresso)) {
                    ingresso.setStatus(Status.CANCELADO);
                    ingresso.setCanceladoEm(LocalDate.now());
                } else {
                    throw new ResponseStatusException(HttpStatus.UNPROCESSABLE_ENTITY, "Ingresso fora do prazo de cancelamento");
                }

            } else {
                throw new ResponseStatusException(HttpStatus.UNPROCESSABLE_ENTITY, "Ingresso já consumido ou cancelado");
            }

            repository.save(ingresso);

            return ResponseEntity
                    .noContent().build();

        } else {
            throw new ResponseStatusException(HttpStatus.NOT_FOUND, "ingresso não encontrado");
        }
    }

    private boolean isDentroDoPrazoDeCancelamento(Ingresso ingresso) {
        LocalDate hoje = LocalDate.now();
        LocalDate dataDoEvento = ingresso.getEvento().getData();
        int prazoEmDias = Period.between(hoje, dataDoEvento).getDays();
        return prazoEmDias >= 1;
    }
}
```

Foi então que utilizando heuristicas simples nós detectamos pontos de melhoria e aplicamos técnicas de OO, "Tell, Don't Ask" e fail fast para acabar com o código refatorado abaixo:

```java
@RestController
public class CancelarIngressoController {

    @Autowired
    private IngressoRepository repository;

    @Transactional
    @PostMapping("/api/v1/ingressos/{id}/cancelar")
    public ResponseEntity<?> cancelar(@PathVariable("id") Long id) {

        Ingresso ingresso = repository.findById(id).orElseThrow(() -> {
            return new ResponseStatusException(HttpStatus.NOT_FOUND, "ingresso não encontrado");
        }); // fail fast aplicado

        ingresso.cancela(); // lógica de negócio movida para dentro da entidade
        repository.save(ingresso);

        return ResponseEntity
                .noContent().build();
    }
}

@Entity
public class Ingresso {

    // código original

    /**
     * Cancela ingresso
     */
    public void cancela() {

        if (!isNaoConsumido()) { // fail fast
            throw new IngressoJaConsumidoException("Ingresso já consumido ou cancelado");
        }

        if (!isDentroDoPrazoDeCancelamento()) { // fail fast
            throw new IngressoForaDoPrazoDeCancelamentoException("Ingresso fora do prazo de cancelamento");
        }

        this.status = Status.CANCELADO;
        this.canceladoEm = LocalDate.now();
    }
}
```

Todas essas técnicas estão dentro do nosso plano de estudos 🥳🥳 Quanto mais você estuda e pratica as técnicas mais fácil fica bater o olho em um código e detectar rapidamente e sem esforço pontos de melhoria!

**Tem dúvidas, críticas ou sugestões?** Basta adicionar comentarios nessa thread, assim podemos discutir e aprendermos juntos 💪🏻

Bons estudos e até a próxima sessão! 👊🏻