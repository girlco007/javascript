# Przewodnik stylistyczny React/JSX od Airbnb

*Przeważnie rozsądne podejście do React i JSX*

## Spis treści

  1. [Podstawowe zasady](#basic-rules)
  1. [Class kontra `React.createClass` kontra bezklasowe](#class-vs-reactcreateclass-vs-stateless)
  1. [Nazewnictwo](#naming)
  1. [Deklaracje](#declaration)
  1. [Wyrównanie](#alignment)
  1. [Cudzysłowy](#quotes)
  1. [Spacjonowanie](#spacing)
  1. [Właściwości - Props](#props)
  1. [Nawiasy](#parentheses)
  1. [Znaczniki](#tags)
  1. [Metody](#methods)
  1. [Porządkowanie](#ordering)
  1. [`isMounted`](#ismounted)

## Podstawowe zasady

  Plik powinien zawierć tylko jeden komponent.
  - Jednak dopuszcza się liczne komponenty w pojedynczym pliku gdy są [komponentami bezstanowymi lub czystymi, ](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions). eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - Zawsze używaj skaładni JSX.
  - Nie używaj `React.createElement`, chyba że inicjujesz aplikację z pliku, który nie jest JSX.

## Class kontra `React.createClass` kontra bezstanowe

  - Jeżeli masz wewnętrzny stan i/albo odnośniki (refs), wtedy skłaniaj się ku wykorzystaniu `class extends React.Component` zamiast `React.createClass`, chyba że posiadasz dobry powód żeby użyć wstawek (mixins). eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

    ```jsx
    // źle
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // dobrze
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
    ```

    Jeśli nie masz stanu (state) lub odwołań (refs), wtedy zamiast klas wybierz normalną funkcję (funkcję anonimową ):

    ```jsx
    // źle
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // źle (poleganie na inferencji (wnioskowaniu?) nazwy funkcji jest odradzane )
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // dobrze
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
    ```

## Nazewnictwo

  - **Rozszrzenia**: Dla komponentów React używaj rozszerzenia `.jsx`.
  - **Nazwy plików**: Dla nazw plików używaj PascalCase np.: `ReservationCard.jsx`.
  - **Nazywanie struktur**: Używaj PascalCase dla komponentów React i camelCase dla ich instancji. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```jsx
    // źle
    import reservationCard from './ReservationCard';

    // dobrze
    import ReservationCard from './ReservationCard';

    // źle
    const ReservationItem = <ReservationCard />;

    // dobrze
    const reservationItem = <ReservationCard />;
    ```

  - **Nazywanie komponentów**: Używaj nazw plików tak jak nazw komponentów. Na przykład odnośnik dla pliku `ReservationCard.jsx` powinien nazywać się  `ReservationCard`. Jakkolwiek, dla komponentów podstawowych (root components) w katalogu, używaj `index.jsx` jako nazwy pliku i nazwy katalogu jako nazwy komponentu:

    ```jsx
    // źle
    import Footer from './Footer/Footer';

    // źle
    import Footer from './Footer/index';

    // dobrze
    import Footer from './Footer';
    ```

## Deklaracje

  - Nie używaj `displayName` dla nazw komponentów. W zamian nazwij komponent po jego odnośniku.

    ```jsx
    // źle
    export default React.createClass({
      displayName: 'ReservationCard',
      // kod
    });

    // dobrze
    export default class ReservationCard extends React.Component {
    }
    ```

## Wyrównanie

  - Podążaj za tymi stylami wyrównań dla składni JSX. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // źle
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // dobrze
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />
    // jeśli właściwości (props) mieszczą się w jednej lini wtedy utrzymaj je w jednej lini
    <Foo bar="bar" />

    // dzieci wyrównane są normalnie
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux />
    </Foo>
    ```

## Cudzysłowy

  - Zawsze używaj podwójnych cudzysłowów (`"`) dla atrybutów JSX i apostrofów (`'`) dla pozostałego kodu JS. eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)

  > Dlaczego? Atrybuty JSX [nie mogą zawierać znaku modyfikacji przed apostrofami](http://eslint.org/docs/rules/jsx-quotes), węc cudzysłowy sprawiają że pisownia wyrazów takich `"don't"` jest łatwiesza w zapisie.
  > Typowe atrybuty HTML także używają cudzysłowów zamiast apostofów, więc atrybuty JSX odzwierciedlają niniejszą konwencję.

    ```jsx
    // źle
    <Foo bar='bar' />

    // dobrze
    <Foo bar="bar" />

    // źle
    <Foo style={{ left: "20px" }} />

    // dobrze
    <Foo style={{ left: '20px' }} />
    ```

## Spacjonowanie

  - Zawsze dodawaj pojedynczą spacje w samo-zamykanych tagach.

    ```jsx
    // źle
    <Foo/>

    // bardzo źle
    <Foo                 />

    // źle
    <Foo
     />

    // dobrze
    <Foo />
    ```

  - Nie okładaj nawiasów klamrowych JSX (`{}`) spacjami. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

    ```jsx
    // źle
    <Foo bar={ baz } />

    // dobrze
    <Foo bar={baz} />
    ```

## Właściwości - Props

  - Zawsze używaj camelCase for nazw własciwości.

    ```jsx
    // źle
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // dobrze
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

  - Unikaj ustawaienia wartości właściwości gdy wyraźnie przypisane jest jej `true`. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```jsx
    // źle
    <Foo
      hidden={true}
    />

    // dobrze
    <Foo
      hidden
    />
    ```

  - Zawsze dodawaj alternatywny opis (`alt`) dla znaczników `<img>`. Jeśli obraz ma właściwości prezentacyjne wtedy `alt` może pozostać pustym ciągiem albo, znacznik `<img>` powinien posiadać oznaczenie `role="presentation"`. eslint: [`jsx-a11y/img-has-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-has-alt.md)

    ```jsx
    // źle
    <img src="hello.jpg" />

    // dobrze
    <img src="hello.jpg" alt="Me waving hello" />

    // dobrze
    <img src="hello.jpg" alt="" />

    // dobrze
    <img src="hello.jpg" role="presentation" />
    ```

  - Nie używaj słów takich jak: "obraz" albo "fotografia" we właściwościach `<img>` i `alt`. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

  > Dlaczego? Czytniki ekranu ogłoszają już elementy ` img` jako obrazy, więc nie ma potrzeby, aby umieszczać tę informację w tekście alt.

    ```jsx
    // źle
    <img src="hello.jpg" alt="Obraz przedsawiający machającą osobe" />

    // dobrze
    <img src="hello.jpg" alt="Machająca osbę" />
    ```

  - Używaj tylko poprawne, nie abstrakcyjne atrybutu `role` [ARIA role](https://www.w3.org/TR/wai-aria/roles#role_definitions). eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

    ```jsx
    // źle - nie jest rolą ARIA
    <div role="datepicker" />

    // źle - abstrakcyjna ARIA role
    <div role="range" />

    // dobrze
    <div role="button" />
    ```

  - Nie używaj `accessKey` na elementach. eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

  > Dlaczego? Niespójność pomiędzy skrótami klawiszowymi a komendami klawiszowymi wykorzystywanymi przez ludzi używających czytniki ekranu i klawiatury komplikuje dostępność.

  ```jsx
  // źle
  <div accessKey="h" />

  // dobrze
  <div />
  ```

  - Unikaj używania indeksu tablicy jako klucza (`key`) właściwości, preferuj używanie unikalnego identyficatora (ID). ([dlaczego?](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318))

  ```jsx
  // źle
  {todos.map((todo, index) =>
    <Todo
      {...todo}
      key={index}
    />
  )}

  // dobrze
  {todos.map((todo) =>
    <Todo
      {...todo}
      key={todo.id}
    />
  )}
  ```

## Nawiasy

  - W przypadku gdy tagów JSX jest więcej niż jeden, wtedy umieść je wewnątrz nawiasów (okrągłych). eslint: [`react/wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/wrap-multilines.md)

    ```jsx
    // źle
    render() {
      return <MyComponent className="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // dobrze
    render() {
      return (
        <MyComponent className="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // dobrze, w przypadku jednej lini
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

## Znaczniki

  - Zawsze używaj samo-zamykających sie tagów w przypadku gdy nie zawierają one żadnych elementów potomnych. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```jsx
    // źle
    <Foo className="stuff"></Foo>

    // dobrze
    <Foo className="stuff" />
    ```

  - W przypadku gdy twój komponent ma właściwości na wielu liniach, wtedy umieść zamknięcie tagu na osobnej lini. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // źle
    <Foo
      bar="bar"
      baz="baz" />

    // dobrze
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## Metody

  - Używaj anonimowych funkcji żeby lokalne zmienne zakmnąć wewnątrz zasięgu.

    ```jsx
    function ItemList(props) {
      return (
        <ul>
          {props.items.map((item, index) => (
            <Item
              key={item.key}
              onClick={() => doSomethingWith(item.name, index)}
            />
          ))}
        </ul>
      );
    }
    ```

  - Powiąrz obsługę zdarzeń dla metody renderowania w konstruktorze. eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

  > Dlaczego? Przywołanie metody łaczenia w ścieżce renderowania tworzy nową funkcję dla każdego renderowania.

    ```jsx
    // źle
    class extends React.Component {
      onClickDiv() {
        // zrób coś
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />
      }
    }

    // dobrze
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // zrób coś
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }
    ```

  - Nie używaj podkreśleń do oznaczenia wewnętrznych metod komponentów React.

    ```jsx
    // źle
    React.createClass({
      _onClickSubmit() {
        // zrób coś
      },

      // pozostały kod
    });

    // dobrze
    class extends React.Component {
      onClickSubmit() {
        // zrób coś
      }

      // pozostały kod
    }
    ```

  - Upewnij się by zawsze zwracać wartość w metodach `render`. eslint: [`require-render-return`](https://github.com/yannickcr/eslint-plugin-react/pull/502)

    ```jsx
    // źle
    render() {
      (<div />);
    }

    // dobrze
    render() {
      return (<div />);
    }
    ```

## Porządkowanie

  - Kolejność dla `class extends React.Component`:

  1. opcjonalne methody `static`
  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers albo eventHandlers* tak `onClickSubmit()` albo `onChangeDescription()`
  1. *metody `get` dla `render`* tak `getSelectReason()` albo `getFooterContent()`
  1. *Opcjonalne metody renderowania* tak `renderNavigation()` albo `renderProfilePicture()`
  1. `render`

  - Jak definiować `propTypes`, `defaultProps`, `contextTypes`, itp.

    ```jsx
    import React, { PropTypes } from 'react';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - Kolejność dla `React.createClass`: eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

  1. `displayName`
  1. `propTypes`
  1. `contextTypes`
  1. `childContextTypes`
  1. `mixins`
  1. `statics`
  1. `defaultProps`
  1. `getDefaultProps`
  1. `getInitialState`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers albo eventHandlers* tak `onClickSubmit()` albo `onChangeDescription()`
  1. *metody `get` dla `render`* like `getSelectReason()` albo `getFooterContent()`
  1. *Opcjonalne metdoy renderowania* like `renderNavigation()` albo `renderProfilePicture()`
  1. `render`

## `isMounted`

  - Nie używaj `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > Dlaczego? [`isMounted` jest antywzorcem][anti-pattern], nie jest dostępny gdy używane sa klasy ES6 i wkrótce zostanie oficijanie wycofany.

  [antywzorzec]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

## Tłumaczenia

  Niniejszy przewodnik do JSX/React jest także dostępny w innych wersjach językowych:

  - ![en](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/United-Kingdom.png) **Angielski**: [airbnb/javascript](https://github.com/airbnb/javascript/tree/master/react)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chiński (Uproszczony)**: [JasonBoy/javascript](https://github.com/JasonBoy/javascript/tree/master/react)


**[⬆ powrót na górę](#table-of-contents)**
