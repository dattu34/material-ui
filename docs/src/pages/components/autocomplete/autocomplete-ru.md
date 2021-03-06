---
title: React-компонент Автозаполнение
components: TextField, Popper, Autocomplete
---

# Autocomplete (Автодополнение)

<p class="description">Автодополнение - это обычный ввод текста, дополненный панелью предлагаемых опций.</p>

Этот виджет используется для установки значения однострочного текстового поля. Он полезен в одном из двух случев:

1. Значение для текстового поля должно быть выбрано из предопределенного набора допустимых значений, например, поле местоположения должно содержать действительное имя местоположения: [поле со списком](#combo-box).
2. Текстовое поле может содержать любое произвольное значение, но целесообразно предлагать пользователю возможные значения. Например, поле поиска может предлагать аналогичные или предыдущие поиски, чтобы сэкономить время пользователя: [free solo](#free-solo).

## Ком-Бог

Значение должно быть выбрано из предопределенного набора допустимых значений.

{{"demo": "pages/components/autocomplete/ComboBox.js"}}

### Песочница

Каждый из следующих примеров демонстрирует одну функцию компонента автозаполнения.

{{"demo": "pages/components/autocomplete/Playground.js"}}

### Выбор страны

Выберите одну страну из 248.

{{"demo": "pages/components/autocomplete/CountrySelect.js"}}

## Бесплатное соло

Set `freeSolo` to true so the textbox can contain any arbitrary value. The prop is designed to cover the primary use case of a search box with suggestions, e.g. Google search.

However, if you intend to use it for a [combo box](#combo-box) like experience (an enhanced version of a select element) we recommend setting `selectOnFocus`.

{{"demo": "pages/components/autocomplete/FreeSolo.js"}}

## Сгруппированные

{{"demo": "pages/components/autocomplete/Grouped.js"}}

## Отключенные опции

{{"demo": "pages/components/autocomplete/DisabledOptions.js"}}

## `useAutocomplete`

For advanced customization use cases, we expose a `useAutocomplete()` hook. It accepts almost the same options as the Autocomplete component minus all the props related to the rendering of JSX. The Autocomplete component uses this hook internally.

```jsx
import useAutocomplete from '@material-ui/lab/useAutocomplete';
```

- 4.5 [4,5 кБ в сжатом виде](/size-snapshot).

{{"demo": "pages/components/autocomplete/UseAutocomplete.js", "defaultCodeOpen": false}}

### Customized hook

{{"demo": "pages/components/autocomplete/CustomizedHook.js"}}

Head to the [Customized Autocomplete](#customized-autocomplete) section for a customization example with the `Autocomplete` component instead of the hook.

## Асинхронные запросы

{{"demo": "pages/components/autocomplete/Asynchronous.js"}}

### Места Google Maps

A customized UI for Google Maps Places Autocomplete.

{{"demo": "pages/components/autocomplete/GoogleMaps.js"}}

For this demo, we need to load the [Google Maps JavaScript](https://developers.google.com/maps/documentation/javascript/tutorial) API.

> ⚠️ Before you can start using the Google Maps JavaScript API, you must sign up and create a billing account.

## Множественные значения

Also known as tags, the user is allowed to enter more than one value.

{{"demo": "pages/components/autocomplete/Tags.js"}}

### Фиксированные опции

In the event that you need to lock certain tag so that they can't be removed in the interface, you can set the chips disabled.

{{"demo": "pages/components/autocomplete/FixedTags.js"}}

### Чекбоксы

{{"demo": "pages/components/autocomplete/CheckboxesTags.js"}}

## Размеры

Fancy smaller inputs? Use the `size` prop.

{{"demo": "pages/components/autocomplete/Sizes.js"}}

## Customized Autocomplete

This demo reproduces the GitHub's label picker:

{{"demo": "pages/components/autocomplete/GitHubLabel.js"}}

Head to the [Customized hook](#customized-hook) section for a customization example with the `useAutocomplete` hook instead of the component.

## Highlights

The following demo relies on [autosuggest-highlight](https://github.com/moroshko/autosuggest-highlight), a small (1 kB) utility for highlighting text in autosuggest and autocomplete components.

{{"demo": "pages/components/autocomplete/Highlights.js"}}

## Пользовательский фильтр

The component exposes a factory to create a filter method that can provided to the `filerOption` prop. You can use it to change the default option filter behavior.

```js
import { createFilterOptions } from '@material-ui/lab/Autocomplete';
```

It supports the following options:

1. `config` (*Object* [optional]): 
  - `config.ignoreAccents` (*Boolean* [optional]): Defaults to `true`. Remove diacritics.
  - `config.ignoreCase` (*Boolean* [optional]): Defaults to `true`. Lowercase everything.
  - `config.matchFrom` (*'any' | 'start'* [optional]): Defaults to `'any'`.
  - `config.stringify` (*Func* [optional]): Defaults to `JSON.stringify`.
  - `config.trim` (*Boolean* [optional]): По умолчанию - `false`. Remove trailing spaces.

In the following demo, the options need to start with the query prefix:

```js
const filterOptions = createFilterOptions({
  matchFrom: 'start',
  stringify: option => option.title,
});

<Autocomplete filterOptions={filterOptions} />
```

{{"demo": "pages/components/autocomplete/Filter.js", "defaultCodeOpen": false}}

### Дополнительные параметры

For richer filtering mechanisms, like fuzzy matching, it's recommended to look at [match-sorter](https://github.com/kentcdodds/match-sorter). Например:

```jsx
import matchSorter from 'match-sorter';

const filterOptions = (options, { inputValue }) =>
  matchSorter(options, inputValue);

<Autocomplete filterOptions={filterOptions} />
```

## Виртуализация

Search within 10,000 randomly generated options. The list is virtualized thanks to [react-window](https://github.com/bvaughn/react-window).

{{"demo": "pages/components/autocomplete/Virtualize.js"}}

## Ограничения

### autocomplete/autofill

The browsers have heuristics to help the users fill the form inputs. However, it can harm the UX of the component.

By default, the component disable the **autocomplete** feature (remembering what the user has typed for a given field in a previous session) with the `autoComplete="off"` attribute.

However, in addition to remembering past entered values, the browser might also propose **autofill** suggestions (saved login, address, or payment details). In the event you want the avoid autofill, you can try the following:

- Name the input without leaking any information the browser can use. e.g. `id="field1"` instead of `id="country"`. If you leave the id empty, the component uses a random id.
- Set `autoComplete="new-password"`: 
        jsx
        <TextField
        {...params}
        inputProps={{
          ...params.inputProps,
          autoComplete: 'new-password',
        }}
        />

### iOS VoiceOver

VoiceOver on iOS Safari doesn't support the `aria-owns` attribute very well. You can work around the issue with the `disablePortal` prop.

### TypeScript

To fully take advantage of type inference, you need to set the `multiple` prop to `undefined`, `false` or `true`. See [this discussion](https://github.com/mui-org/material-ui/pull/18854#discussion_r364215153) for more details. TypeScript might solve this bug in the future.

## Доступность

(WAI-ARIA: https://www.w3.org/TR/wai-aria-practices/#combobox)

We encourage the usage of a label for the textbox. The component implements the WAI-ARIA authoring practices.