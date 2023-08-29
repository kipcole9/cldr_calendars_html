# Cldr Calendar Formatting

[ex_cldr_calendars_format](https://hex.pm/packages/ex_cldr_calendars_format) provides a framework for formatting [calendars](https://hex.pm/packages/ex_cldr_calendars) for output.  It includes example formatters for HTML (`Cldr.Calendar.Formatter.HTML.Basic`), HTML for week-based calendars (`Cldr.Calendar.Formatter.HTML.Week`) and Markdown (`Cldr.Calendar.Formatter.Markdown`).
These examples may be used as-is but supporting the development of formatters that meet specific requirements is a design objective which is implemented via the `Cldr.Calendar.Formatter` behaviour.

## Getting Started

A formatted calendar is generated via `Cldr.Calendar.Format.year/2` or `Cldr.Calendar.Format.month/3`.  For example, to generate a simple HTML calendar for April 2019:

```elixir
iex> Cldr.Calendar.Format.month 2019, 4
"<table class=\"cldr_calendar\">\n  <caption>April 2019</caption\n  <th>\n    <td class=\"day_name, weekday\">Mon</td>\n    <td class=\"day_name, weekday\">Tue</td>\n    <td class=\"day_name, weekday\">Wed</td>\n...."
```

From this example we can deduce that:

* The default calendar is `Cldr.Calendar.Gregorian` which is compatible with the standard `Calendar.ISO` but includes additional functionality to support the notion of weeks.

* That the default calendar has a week that starts on Monday (not all calendars start the week on Monday)

* That the default locale is "en" (English).  Calendars can be localised to any locale supported by [ex_cldr](https://hex.pm/packages/ex_cldr)

* That the default formatter is `Cldr.Calendar.Formatter.HTML.Basic`. Any formatter can be invoked and additional formatters `Cldr.Calendar.Formatter.HTML.Week` and `Cldr.Calendar.Formatter.Markdown` are included.

### Formatting a week-based calendar

Week-based calendars, like [ISO Week]() and [National Retail Federation](), don't have months in the traditional sense however they can be formatted in a month-like fashion.  The formatter `Cldr.Calendar.Formatter.HTML.Week` is different from `Cldr.Calendar.Formatter.HTML.Basic` in that is includes the week number as well as the dates.  It also converts each date into the `Cldr.Calendar.Gregorian` calendar for familiar formatting.  For example:

```elixir
iex> Cldr.Calendar.Format.month 2019, 4, calendar: Cldr.Calendar.NRF, formatter: Cldr.Calendar.Formatter.HTML.Week
"<table class=\"cldr_calendar\">\n  <caption>April 2019</caption\n  <th>\n    <td> </td>\n    <td class=\"day_name, weekend\">Sun</td>\n    <td class=\"day_name, weekday\">Mon</td>\n    <td class=\"day_name, weekday\">Tue</td>\n    <td class=\"day_name, weekday\">Wed</td>\n    <td class=\"day_name, weekday\">Thu</td>\n    <td class=\"day_name, weekday\">Fri</td>\n    <td class=\"day_name, weekend\">Sat</td>\n\n  </th>\n  <tr>\n    <td class=\"week\">W14</td>\n ... "
```

This example shows that the first day of the week for the `Cldr.Calendar.NRF` calendar is Sunday and that the week number is included.

### Formatting Markdown

The `Cldr.Calendar.Formatter.Markdown` formatter is a simple formatter.  Using the simple example for April 2019 the example shows:
```elixir
iex> Cldr.Calendar.Format.month 2019, 4, formatter: Cldr.Calendar.Formatter.Markdown
"### April 2019\n\nMon | Tue | Wed | Thu | Fri | Sat | Sun\n :---:  |  :---:  |  :---:  |  :---:  |  :---:  |  :---:  |  :---: \n**1** | **2** | **3** | **4** | **5** | **6** | **7**\n**8** | **9** | **10** | **11** | **12** | **13** | **14**\n**15** | **16** | **17** | **18** | **19** | **20** | **21**\n**22** | **23** | **24** | **25** | **26** | **27** | **28**\n**29** | **30** | 1 | 2 | 3 | 4 | 5\n6 | 7 | 8 | 9 | 10 | 11 | 12\n"
```
Which renders as (heading omitted):

Mon | Tue | Wed | Thu | Fri | Sat | Sun
 :---:  |  :---:  |  :---:  |  :---:  |  :---:  |  :---:  |  :---:
**1** | **2** | **3** | **4** | **5** | **6** | **7**
**8** | **9** | **10** | **11** | **12** | **13** | **14**
**15** | **16** | **17** | **18** | **19** | **20** | **21**
**22** | **23** | **24** | **25** | **26** | **27** | **28**
**29** | **30** | 1 | 2 | 3 | 4 | 5
6 | 7 | 8 | 9 | 10 | 11 | 12

### Calendar localization

Since [ex_cldr_calendars_format](https://hex.pm/packages/ex_cldr_calendars_format) is built upon [ex_cldr](https://hex.pm/packages/ex_cldr), calendars can be localised.  Localised formatting understands the weekend and weekdays for a given locale or territory and can use the default of native number system (which may use localised characters for numbers).  For example, using the locale for Saudi Arabia we can see:

```elixir
iex> markdown = Cldr.Calendar.Format.month 2019, 4, formatter: Cldr.Calendar.Formatter.Markdown, locale: "ar-SA"
"### أبريل ٢٠١٩\n\nالاثنين | الثلاثاء | الأربعاء | الخميس | الجمعة | السبت | الأحد\n :---:  |  :---:  |  :---:  |  :---:  |  :---:  |  :---:  |  :---: \n**١** | **٢** | **٣** | **٤** | **٥** | **٦** | **٧**\n**٨** | **٩** | **١٠** | **١١** | **١٢** | **١٣** | **١٤**\n**١٥** | **١٦** | **١٧** | **١٨** | **١٩** | **٢٠** | **٢١**\n**٢٢** | **٢٣** | **٢٤** | **٢٥** | **٢٦** | **٢٧** | **٢٨**\n**٢٩** | **٣٠** | ١ | ٢ | ٣ | ٤ | ٥\n٦ | ٧ | ٨ | ٩ | ١٠ | ١١ | ١٢\n"
```
Which renders as (heading omitted):

الاثنين | الثلاثاء | الأربعاء | الخميس | الجمعة | السبت | الأحد
 :---:  |  :---:  |  :---:  |  :---:  |  :---:  |  :---:  |  :---:
**١** | **٢** | **٣** | **٤** | **٥** | **٦** | **٧**
**٨** | **٩** | **١٠** | **١١** | **١٢** | **١٣** | **١٤**
**١٥** | **١٦** | **١٧** | **١٨** | **١٩** | **٢٠** | **٢١**
**٢٢** | **٢٣** | **٢٤** | **٢٥** | **٢٦** | **٢٧** | **٢٨**
**٢٩** | **٣٠** | ١ | ٢ | ٣ | ٤ | ٥
٦ | ٧ | ٨ | ٩ | ١٠ | ١١ | ١٢

### Configuring a Cldr backend for localization

In order to localize date parts a`backend` module must be defined. This
is a module which hosts the CLDR data for a set of locales. The detailed
information for configuring a `backend` is [documented here](https://hexdocs.pm/ex_cldr/readme.html#configuration).

For a simple configuration the following steps may be used:

1. Create a backend module.

Note that the provider `Cldr.Number` must be configured since it supports localisation of number formatting.

```
defmodule MyApp.Cldr do
  use Cldr,
    locales: ["en", "fr", "jp", "ar"],
    providers: [Cldr.Number]

end
```

2. Optionally configure this backend as the system default in your `config.exs`.
```
config :ex_cldr,
  default_backend: MyApp.Cldr
```

## Installation

This package can be installed by adding `ex_cldr_calendars_format` to your list of dependencies in `mix.exs`:

```elixir
def deps do
  [
    ...
    {:ex_cldr_calendars_format, "~> 0.1.0"}
  ]
end
```

Documentation can be found at [https://hexdocs.pm/ex_cldr_calendars_format](https://hexdocs.pm/ex_cldr_calendars_format).

