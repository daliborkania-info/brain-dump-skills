# brain-dump - dostaň projekt z hlavy a udrž ho tam, kde patří

[English version: README.md](README.md)

Tři skilly pro Claude Code / Cowork (a jakýkoli nástroj podporující standard Agent Skills), které z tebe vytáhnou všechno, co o projektu víš, srovnají to do živé dokumentace a tu dokumentaci drží v souladu s realitou po celou dobu, co na projektu pracuješ. Postavené na principu virálního skillu `grill-me` od Matta Pococka, ale doplněné o to, co mu chybělo: paměť, persistenci a kontrolu nad tím, co se kdy zapíše.

Pozn.: samotné skilly jsou v angličtině (spouštěcí fráze i instrukce). Konverzaci ale vedou v jazyce, kterým mluvíš ty - odpovídat můžeš česky, jen úvodní spouštěcí fráze jsou anglicky.

## Proč to chceš

Znáš to: nápad je v hlavě křišťálově jasný, ale jakmile ho začneš realizovat, polovina detailů se vypaří a druhá polovina si začne protiřečit. AI asistent se navíc vrhne do práce dřív, než pochopí, co vlastně chceš. brain-dump obrací role - ne ty vysvětluješ jemu, ale on se tě ptá, jednu otázku po druhé, ke každé rovnou navrhne odpověď (stačí říct "ano"), a všechno průběžně zapisuje do tří souborů, které se stanou pamětí projektu. Po pár sezeních ti AI dokončuje myšlenky dřív, než je doříkáš.

## Co umí - tři skilly

**brain-dump-init** - založení nebo převzetí projektu.
Buď máš čerstvý nápad (necháš ho volně vysypat, pak tě skill prožene tématy), nebo nasazuješ brain-dump na už běžící projekt - pak ti ho zmapuje, vytáhne roztroušené myšlenky z README, poznámek, TODO/FIXME komentářů a commitů do návrhu dokumentace, a projde to s tebou: sedí to? co chybí?

**brain-dump-add** - doplnění myšlenky kdykoli za běhu.
Napadne tě něco uprostřed projektu. Skill to nejdřív bezpečně zachytí (ať se to neztratí), pak ti ukáže přesný dopad do celého projektu - co ta myšlenka mění, s čím koliduje - a teprve po tvém schválení to promítne. Riziková rozhodnutí schvaluješ jednotlivě, drobnosti dávkou.

**brain-dump-sync** - srovnání dokumentace s realitou.
Po čase se kód a dokumentace rozejdou. Skill najde rozpory (i mezi dokumenty navzájem), ukáže ti je s návrhem opravy, ale nikdy nerozhodne za tebe, která strana je správně - to víš jen ty.

## Co vznikne - tři soubory v projektu

`PROJECT.md` (vize, zaměření, cíle, otevřené otázky), `CONTEXT.md` (glosář pojmů - kompatibilní s grill-with-docs) a `decisions.md` (rozhodnutí včetně zamítnutých variant a důvodů). Za týden ti na otázku "proč jsme tohle neudělali" odpoví záznam, ne tvoje paměť.

## Jak na to

Skilly dodržují [otevřený standard Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview): složka se souborem `SKILL.md` a souborem v `references/`. Díky tomu je lze přenášet mezi více nástroji.

### Claude Code (doporučeno)

1. Zkopíruj tři složky skillů do adresáře se skilly:
   - osobní (všechny projekty): `~/.claude/skills/`
   - jen pro projekt: `.claude/skills/` v kořeni repozitáře
   Výsledek je tedy např. `~/.claude/skills/brain-dump-init/SKILL.md`.
2. Restartuj Claude Code (nebo spusť nové sezení), ať je načte. Ověř příkazem `/skills`.
3. Používej je podle záměru - není třeba psát název skillu:
   - Nový projekt: `Mám nápad na projekt, pojďme ho vysypat (brain-dump).`
   - Existující repo: `Zmapuj tenhle projekt a nasaď na něj brain-dump.`
   - Myšlenka za běhu: `Napadlo mě - měli bychom umožnit export do CSV. Zapracuj to do projektu.`
   - Rozejité dokumenty: `Srovnej dokumentaci s aktuálním stavem projektu.`

### Claude.ai / Claude Desktop (Cowork)

1. Zazipuj každou složku skillu (soubory `.skill` v této složce jsou už zazipované a připravené).
2. V aplikaci: Settings > Customize > Skills > **+** > Create skill, a nahraj soubor. Claude přečte `SKILL.md` a ukáže shrnutí. Skill zapni přepínačem.
3. Ověř, že je povolené spouštění kódu: Settings > Capabilities (Free/Pro/Max) nebo Organization settings > Skills (Team/Enterprise).
4. Pak s ním mluv stejně jako výše ("Mám nápad na projekt, pojďme ho vysypat").

### Další nástroje kompatibilní s Agent Skills

Standard Agent Skills přijaly i nástroje mimo Claude. Stejné složky `SKILL.md` fungují - každý nástroj má vlastní místo pro instalaci - mimo jiné v **OpenAI Codex CLI**, **Cursoru**, **Gemini CLI** a **GitHub Copilotu**. Mechanika se liší podle nástroje (kam složku umístit, jak ji zapnout), takže nahlédni do dokumentace daného nástroje k "skills" / "custom instructions", ale obsah skillu je identický a nevyžaduje žádné úpravy. Příklad po instalaci v kterémkoli z nich:

> "Adopt brain-dump on this repo - map it and review the documentation with me."

Pozn.: spouštěcí klíčová slova jsou anglicky. Skilly přesto vedou konverzaci v jazyce, kterým mluvíš (pravidlo Language), takže odpovídat můžeš česky; anglicky jsou jen úvodní spouštěcí fráze. Pokud chceš spouštění v češtině, přidej české fráze do řádku `description:` v každém `SKILL.md`.

## V čem je jiný

Nikdy ti nepřepíše rozhodnutí bez svolení - zachycení nápadu a jeho promítnutí do projektu jsou dva oddělené kroky, mezi nimi vždy uvidíš přesný dopad a schválíš ho. Nezahltí tě nekonečným vyslýcháním (checkpointy místo stovek otázek). A nesahá ti na hotovou práci - jen navrhuje, ty rozhoduješ.

Nejlepší pro sólo vývojáře a malé týmy, kde se zadání rodí průběžně v jedné hlavě. Na rychlý jednorázový úkol je zbytečný; pro projekt, který žije týdny a měsíce, je to pojistka proti tomu, že ti vlastní nápad uteče mezi prsty.

## Poděkování

Postavené na vyslýchacím vzoru z [`grill-me`](https://github.com/mattpocock/skills) a na myšlence ubiquitous language / CONTEXT.md z `grill-with-docs`, obojí od Matta Pococka. brain-dump přidává rozdělení zachycení/dopad/schválení, obousměrnou propagaci a adopční režim pro existující projekty.
