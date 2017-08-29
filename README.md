# UIs confiáveis usando tipos

Você consegue garantir que sua UI leva em conta
**todos** os estados em que ela poderia estar?

- `undefined`
- `active` 
- `inactive` 
- `disabled` 
- `loading`

Cada variação, cada combinação…

Exemplo:
```json
{
  "name": "Acme Inc.",
  "status": "active",
  "about": "An incorporated company",
  "contact": {
    "email": "ceo@acme.com",
    "name": "The CEO"
  }
}
```

Tipos:
```elm
type alias Company = {
  name: String
  status: String,
  about: String,
  contact: {
    email: String,
    name: String
  }
}
```

Tipos na verdade:
```elm
type alias Company = {
  name: String
  status: CompanyStatus,
  about: Maybe String,
  contact: Maybe Contact
}

type CompanyStatus 
  = Active
  | Inactive
  | Deleted
  | BlackFlagged

type alias Contact = {
  email: String,
  name: String
}
```


Renderizar corretamente dada qualquer combinação de estados
é muito importante para manter sua UI estável e funcionando.


Perceba também o uso de Union Types para ser mais específico.
`status` não é qualquer `String` possível, por exemplo.



Ter um compilador te ajudando a ter certeza de que você
lenvou em conta todas as opções é muito prático.




O estado da sua aplicação pode sempre ser consistente também





Imagine um slider de imagens:

```
+-----------------------+
|      --------         |
| ----------            |
|   ----        ------- |
|     --------------    |
|  --        -----------|
|-----------------------|
|--0--|  1  |  2  |  3  |
+-----------------------+
```

Sempre há uma imagem selecionada:
```
+-----------------------+
|      --------         |
| ----------            |
|   ----        ------- |
|     --------------    |
|  --        -----------|
|-----------------------|
|  0  |  1  |--2--|  3  |
+-----------------------+
```


Um tipo simples para esse estado seria:
```elm
type alias Model = {
  images: List Image,
  selectedImage: Int
}
```



Mas...


E se `images` for `[]`?


E se `selectedImage` for um índice que não existe em `images`?


Podemos utilizar um tipo mais específico para nos proteger
dessas situações:

```elm
type alias Slider = {
  previous: List Image,
  selected: Image,
  next: List Image
}

type alias Model = {
  slider: Slider
}
```

Dessa forma:
- Sempre temos uma imagem
- Sempre temos uma imagem selecionada

Seria garantido que a nossa UI estaria consistente!



E o que acontece enquanto essas imagens ainda não carregaram?

```elm
type alias Model {
  slider: Maybe Slider
}
```

Bom, isso já ajuda, mas existem outros estados em vez de
apenas `Nothing`.

```elm
type RemoteData error a
  = NotAsked
  | Loading
  | Failure error
  | Success a
```
Fonte: http://package.elm-lang.org/packages/krisajenkins/remotedata

Com esse tipo, sempre que temos um dado que será carregado,
teremos que lidar com cada um dos estados.


Definir corretamente os tipos é útil tanto para conseguir
mapear corretamente os dados utilizados na sua aplicação,
quanto ter certeza que todos levarão em conta todos os
estados possíveis.


Elm consegue garantir isso pois é **puro e estaticamente tipado**.


Da próxima vez que estiver definindo os estados da sua aplicação:
- Tome cuidado com tipos muito abrangentes como `String`
- Evite tipos espalhados em duas chaves de um Record
- Verifique se o `Maybe` realmente expressa a realidade do seu estado

Referência: [Make Impossible States Impossible](https://www.youtube.com/watch?v=IcgmSRJHu_8)

Obrigado!
