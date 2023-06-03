# [Vue Paper Dashboard](https://cristijora.github.io/vue-paper-dashboard/)

> Admin dashboard based on paper dashboard UI template + vue-router

This project is a vue version of [Paper-dashboard](https://www.creative-tim.com/product/paper-dashboard)
designed for vue js.The dashboard includes vue-router

Check the [Live Demo here](https://cristijora.github.io/vue-paper-dashboard).

[Nuxt Version (outdated Bootstrap 3)](https://github.com/cristijora/vue-paper-dashboard-nuxt)
![](http://i.imgur.com/3iC1hOs.gif)

## Documentation

Link to [Documentation](http://vuejs.creative-tim.com/vue-paper-dashboard/documentation/)

## Build Setup

### install dependencies

```
npm install
```

### serve with hot reload at localhost:8080

```
npm run dev
```

### build for production with minification

```
npm run build
```

### lint

```
npm run lint
```

## Contribution guide

- Fork the repository
- `npm install` or `yarn install`
- Make changes
- Open Pull Request

For detailed explanation on how things work, checkout the [guide](https://github.com/vuejs/vue-cli/blob/dev/docs/README.md)

- [CHANGELOG](./CHANGELOG.md)
- [version-badge](https://img.shields.io/badge/version-1.0.1-blue.svg)
- [license-badge](https://img.shields.io/badge/license-MIT-blue.svg)

## License

[MIT](https://github.com/creativetimofficial/vue-paper-dashboard/blob/master/LICENSE.md)

vue ( na pasta pages que é onde estão feitas as alterações mais significativas) 
-dashboard:  estatísticas de número de reservas campos, do dinheiro feito com reservas de aulas e campos, do número de aulas, do numero de utilizadores,estatisticas do número de raquetes por utilizador, estatisticas do campo e horas mais escolhidas pelos utiizadores para reserva campo
-perfil: dados do administrdor
-lista de utilizadores de todos os que estão registados
-reserva campo aceita ou rejeita as reservas
-reserva aula: aceita ou não as reservas e atribui um campo
-mapa de campos: conforme as reservas aceites, coloca no campo certo
-mapa de aulas: conforme as aulas aceites, coloca no campo atribuído essa aula
