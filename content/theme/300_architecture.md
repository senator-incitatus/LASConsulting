Architecture
===============

### Strukur av LESS-koden

Min kod ligger ofta, men inte alltid, i någon sorts "kronologisk" ordning. Det som händer först i koden, händer ofta först i CSS:en också. Dels underlättar det när jag ska hitta något, dels är det lite ett sådant arbetsflöde jag har.

Jag har utnyttjat att LESS kan använda variabler, samt även att det går att nesta regler. Nu på slutet gjorde jag några hotfixes där jag specifierade på samma rad, men så gott jag kan har jag försökt att utnyttja LESS inbyggda funktioner.

Jag gillar när det är lätt att få en överblick, och när det finns någon sorts logisk struktur. Jag tror även att det är lättare att lämna över ett projekt till någon annan om det finns en tydlig struktur, vettiga klassnamn, och korrekt med indentering.

### Mina LESS-moduler

Jag har följande LESS-moduler:

* figure
* font-awesome
* footer
* grid-helpers
* header
* layout
* lib
* main
* media-queries
* normalize
* responsive-menu
* typographic
* vertical

Samt för färgsättningen av mitt custom-tema (gradient):

* gradient_colors
* gradient_lib

De flesta förklarar sig själva. Layout är den modul som till stor del styr det vertikla griddet - hur många kolumner ett element ska sträcka sig över. Lib, library, är en samlingsmodul som mest är till för att lagra variabler, bland annat fonter och paddingen som ser till att innehållet inte överlappar med footern. Jag hade gärna utnyttjat mitt library mer och lagt fler saker där. Main har ett lite mer obskyrt namn, men styr över själva innehållet - hur ska en h4 se ut? Eller en ul?

Dessa sammanställs sedan i modules.less, och mina två teman - base och gradient - laddar modules.less och sedan eventuella moduler som påverkar färgsättningen. Base laddar inte in något extra alls, men gradient läser av gradient_colors.less samt gradient_lib.less. Färgsättningen ligger i mappen color_modules, och består alltid av två filer: ett library med variabler för färger och dylikt, och en fil som petar ut färgerna på rätt ställen.

Jag har gärna lite fler men lite mindre moduler. Det är lättare att se vad som ligger var - "aha, där ligger allt som har med footern att göra, där ligger headern, där ligger övergripande layout...". Jag tycker att det är oerhört smidigt att kompilera alla layout-element och sedan bara lägga på färger eller små fix på den grunden.

### Ett par exempel

Här kommer ett par exempel.

**header.less** innehåller ett litet CSS-hack, eller vad man ska kalla det.

~~~~
.route- {
    .inner-wrap-header {
        height: 100vh;
    }

    .inner-wrap-header::after {
        content: "There are secrets to success";
        display: block;
        margin: 50vh auto 0 auto;
        color: #000;
        text-align: center;
        text-transform: uppercase;
        font-family: @quoteFont;
        #hgrid .fontSize(@fontSizeH7);
    }
~~~~

Det här är (delar av) koden som gör att förstasidan har en heltäckande bild med ett citat på.

I **main.less** finns koden som gör att jag använder carets som list-ikon.

~~~~
ul:not(.rm-desktop):not(.rm-small):not(.rm-mobile) {
    list-style-type: none;
    li::before {
        content: "\f0da";
        font-family: "FontAwesome";
        width: 10px;
        height: 10px;
        margin-right: 15px;
        font-weight: normal;
    }
}
~~~~

Det läggs som sagt inte på header-menyn, och i gradient_colors.less finns även en regel som säger att det första itemet i navigationslistan inte ska ha en caret.

Sist men inte minst vill jag också peka ut **media-queries.less**, där jag jobbat med att sidan ska se så bra ut som möjligt över de flesta skärmbreddar. Jag har lagt till några breddar som tidigare låg lite i gränslandet mellan övergångar. Media-queries jobbar med att skifta kolumner, ändra textstorlek, håll footern på plats, med mera.

I övrigt är det mina layout-moduler som helt enkelt gör grovjobbet: footer, header, layout, och main.
