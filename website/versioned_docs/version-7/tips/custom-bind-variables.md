---
title: Custom bind variables
description: Altering the SQL for certain RDBMS's
hide_table_of_contents: true
---

:::tip

The specific method described below is not necessary in version 7 or later. The [`numberedParams` option](../utils/export#numbered-parameters) will achieve the same result.

:::

Different SQL flavors have different requirements for bind variable placeholders. Some use a simple `?` character (which is what `formatQuery(query, 'parameterized')` produces by default) and others require a bind variable placeholder to start with `$` followed by a unique string or number.

The ["parameterized_named" export format](../utils/export#named-parameters), along with the [`paramPrefix` option](../utils/export#parameter-prefix), will usually be sufficient to cover the latter case. However, if the default parameter names (e.g. `:fieldName_1`) are not acceptable, you can use the "parameterized" format and replace the "?" with your choice of name.

The following code will produce a SQL string with each bind variable placeholder being numbered from "$1" to "$_n_", where _n_ is the number of bind variables (also, appropriately, the number of elements in the `params` array).

```ts
let i = 0;
const fq = formatQuery(query, 'parameterized');
const fqWithNumberedParams = {
  ...fq,
  sql: fq.sql.replaceAll('?', () => `$${++i}`),
};
```

For example, if `formatQuery(query, "parameterized")` produced the following object:

```json
{
  "sql": "(firstName = ? and lastName = ?)",
  "params": ["Steve", "Vai"]
}
```

...then the code above would assign the following object to the `fqWithNumberedParams` variable:

```json
{
  "sql": "(firstName = $1 and lastName = $2)",
  "params": ["Steve", "Vai"]
}
```
