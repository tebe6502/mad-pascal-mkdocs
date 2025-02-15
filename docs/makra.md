#

## [Makra](https://www.freepascal.org/docs-html/prog/progse5.html)

**MP** pozwala na używanie makr, podobnie jak **FPC**, z tym tylko wyjątkiem że makra zawsze są włączone.

```delphi
 {$macro on}
 {$macro off}
 {$macro+}
 {$macro-} 
```

Dyrektywa `{$macro on}` jest wymagana przez **FPC**, w **MP** jest zachowana tylko w celu zgodności.

### Definiowanie makra

```delphi
{$define label := expression}

{$define label(par0, par1 ... par7) := expression}
```

Aby definicja została rozpoznana jako makro po nazwie etykiety i ewentualnej liście `(par0..par7)` parametrów musi wystąpić symbol przypisania `:=`.


```delphi
{$define new_proc :=

procedure test;
begin

 writeln('ok');

end;
}


  new_proc


begin

 test;

end.
```

```delphi
{$define sum_xi

 :=

 x:=x+i;
 }
 
begin

 sum_xi;
 
end;
```

```delphi
{$define WIDTH := 80}
{$define LEN   :=   ( WIDTH + 10 )}

var a: byte;

begin

 a := len * 20;

end.
```

Makra z parametrami obsługiwane są przez **MP** ale nie przez **FPC**.

```delphi
{$define SIGN_MASK := $8000}
{$define SIGNED_INF_VALUE(x) := ((x and SIGN_MASK) or $7C00)}

var a: byte = 11;

begin

 writeln( SIGNED_INF_VALUE(a shl 15) );

end.
```
