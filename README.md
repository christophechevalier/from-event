[![MIT](https://img.shields.io/packagist/l/doctrine/orm.svg?style=flat-square)]()
[![commitizen](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg?style=flat-square)]()
[![PRs](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)]()
[![styled with prettier](https://img.shields.io/badge/styled_with-prettier-ff69b4.svg?style=flat-square)](https://github.com/prettier/prettier)
[![All Contributors](https://img.shields.io/badge/all_contributors-1-orange.svg?style=flat-square)](#contributors-)
[![ngneat](https://img.shields.io/badge/@-ngneat-383636?style=flat-square&labelColor=8f68d4)](https://github.com/ngneat/)

> ViewChild and FromEvent — a Match Made in Angular Heaven

Turn `ViewChild` and `ContentChild` queries into an RxJS stream.

## Installation
`npm install @ngneat/from-event`

## Usage
Use the `FromEvent` decorator with `ViewChild` or `ContentChild`. Note that it expects to get `ElementRef`.

```ts
import { FromEvent } from '@ngneat/from-event';

@Component({
  selector: 'my-btn',
  template: `
    <button #button>
      <ng-content></ng-content>
    </button>
  `
})
class ButtonComponent implements AfterViewInit {
  @FromEvent('click')
  @ViewChild('button') 
  click$: Observable<MouseEvent>;

  ngAfterViewInit() {
    this.click$.subscribe(console.log);
  }
}
```

A common example is using it with `switchMap()`:

```ts
import { FromEvent } from '@ngneat/from-event';

@Component({
  selector: 'my-comp',
  template: `
    <button #trigger>Trigger</button>
  `
})
class MyComponent implements AfterViewInit {
  @FromEvent('click')
  @ViewChild('trigger') 
  trigger$: Observable<MouseEvent>;

  ngAfterViewInit() {
    this.trigger$.pipe(switchMap(() => service.doSomething())).subscribe(result => {
      // Do something with the result
    });
  }
}
```


Use the `FromEvents` decorator with `ViewChildren` or `ContentChildren`. Note that it expects to get `ElementRef`.

```ts
import { FromEvents } from '@ngneat/from-event';

@Component({
  template: `
    <my-btn id="1">Click One</my-btn>
    <my-btn id="2">Click Two</my-btn>
    <my-btn id="3">Click Three</my-btn>
  `
})
class HostComponent implements AfterViewInit, OnDestroy {
  @FromEvents('click')
  @ViewChildren(ButtonComponent, { read: ElementRef }) 
  clicks$: Observable<MouseEvent>;

  ngAfterViewInit() {
    this.clicks$.subscribe(e => console.log(e.target));
  }
}
```

Have fun, and don't forget to unsubscribe. If you work with **Ivy**, you can do it with [until-destroy](https://github.com/ngneat/until-destroy).

## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="https://www.netbasal.com"><img src="https://avatars1.githubusercontent.com/u/6745730?v=4" width="100px;" alt=""/><br /><sub><b>Netanel Basal</b></sub></a><br /><a href="https://github.com/@ngneat/from-event/commits?author=NetanelBasal" title="Code">💻</a></td>
  </tr>
</table>

<!-- markdownlint-enable -->
<!-- prettier-ignore-end -->
<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!
