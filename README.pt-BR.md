# Um pacote personalizado do tzdata

O governo brasileiro decretou que de agora em diante não teremos mais horário de verão. Para que os serviços Java não entrem em DST erroneamente a tabela de tzdata deve ser atualizada.

### O problema

Quando tentei atualizar um serviço usando o último dos [releases da IANA](https://data.iana.org/time-zones/releases/), me deparei com o seguinte erro:

```bash
Using file:tzdata2019b.tar.gz as source for tzdata bundle.
[...]
tzupdater version 2.2.0-b01
JRE tzdata version: tzdata2018e
Downloaded file to /tmp/tz.tmp/tzdata.tar.gz
tzupdater tool would update with tzdata version: tzdata2019b
Compiling TZDB version 2019b
Parsing file: /tmp/tz.tmp/africa
Parsing file: /tmp/tz.tmp/antarctica
Parsing file: /tmp/tz.tmp/asia
Failed: java.lang.Exception: Failed while parsing file '/tmp/tz.tmp/asia' on line 1865 'Rule    Japan   1948    1951    -       Sep     Sat>=8  25:00   0       S'
java.lang.Exception: Failed while parsing file '/tmp/tz.tmp/asia' on line 1865 'Rule    Japan   1948    1951    -       Sep     Sat>=8  25:00   0       S'
        at tools.tzdb.TzdbZoneRulesCompiler.parseFile(TzdbZoneRulesCompiler.java:377)
        at tools.tzdb.TzdbZoneRulesCompiler.compile(TzdbZoneRulesCompiler.java:191)
        at tools.tzdb.TzdbZoneRulesCompiler.<init>(TzdbZoneRulesCompiler.java:307)
        at com.sun.tools.tzupdater.ExternalModule.compileToJSRBinary(ExternalModule.java:153)
        at com.sun.tools.tzupdater.TimezoneUpdater.run(TimezoneUpdater.java:230)
        at com.sun.tools.tzupdater.TimezoneUpdater.main(TimezoneUpdater.java:634)
Caused by: tools.tzdb.DateTimeException: Invalid value for SecondOfDay value: 90000
        at tools.tzdb.ChronoField.checkValidValue(ChronoField.java:173)
        at tools.tzdb.LocalTime.ofSecondOfDay(LocalTime.java:210)
        at tools.tzdb.TzdbZoneRulesCompiler.parseMonthDayTime(TzdbZoneRulesCompiler.java:475)
        at tools.tzdb.TzdbZoneRulesCompiler.parseRuleLine(TzdbZoneRulesCompiler.java:399)
        at tools.tzdb.TzdbZoneRulesCompiler.parseFile(TzdbZoneRulesCompiler.java:354)
        ... 5 more
```

### O Workaround

Como medida de contorno eu fiz esse pacote compilando os dados da asia do [release 2018e](https://data.iana.org/time-zones/releases/tzdata2018e.tar.gz) e os demais do release [release 2019b](https://data.iana.org/time-zones/releases/tzdata2019b.tar.gz).

### Como usar

Executar apontando diretamente para o pacote neste repositório:

```bash
java -jar tzupdater.jar -l https://github.com/dopessoa/tzdata/raw/master/tzdata2019b.tar.gz
```

Baixar em algum lugar e executar com o Java que deve ser atualizado:

```bash
java -jar tzupdater.jar -l file:tzdata2019b_br.tar.gz
```