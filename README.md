**bpezip**

__Compression of short texts using pre-trained BPE encoding (JavaScript)__

Most compression methods only work well on long inputs. BPEZip also works on very short texts (a few bytes).
It uses pre-trained Byte Pair Encoding (BPE) vocabularies, which replace common patterns with shorter tokens.
This makes it useful for things like short phrases or especially street names, which were used for training.

**How it works**

When you compress text:

- Each character is replaced with tokens from a pre-trained BPE vocabulary.
- Common character sequences are stored as single tokens, making the result shorter.
- The encoder can use models trained for specific countries to match language patterns.
- If no country is specified, it tries all models and picks the one that gives the smallest result.

There is also a TightInts feature that compresses sequences of numbers even further by storing differences instead of full values.

Note: Models are labeled by country code instead of language code, because they were trained on street names from those countries.

**Advantages**

- Works well for very short texts.
- Adapts to many languages.
- Automatically picks the best model if no country is given.
- If the compressed result is larger than the original, it just returns the original text with one extra byte.

Example:
```
<head>
<meta charset="utf-8">
</head>

<script src=bpezip.js></script>
<script>

    function testit(s)
    {
      let e=bpe_encode(s);
      let d=bpe_encode(s,100,'x');
      console.log("Unpacked:",d.length,"Packed:",e.length,
                  "Compression:",Math.floor(e.length*100/d.length).toFixed(0)+"%",bpe_decode(e));
    }

    testit('Emily Watson, 47 Wattle Crescent, 2602 Ainslie');
    testit('蒋青莲, 云岩区燕雀巷17号碧霄楼, 550002 贵阳');
    testit('Rigmor Kjeldsen, Møllestien 14, 8000 Aarhus C');
    testit('عائشة محروس, زقاق المدابغ ٣, ١١٦٢٢ الجيزة');
    testit('Unto Kuusisto, Paimenenpolku 7 A 4, 66950 Jurva');
    testit('Léontine Perrault, 4 Impasse des Rossignols, 17110 Saint-Georges-de-Didonne');
    testit('Ulrich Wiesengrund, Finkenrain 9, 79199 Kirchzarten');
    testit('Zsombor Holló, Rákóczi-rét 3, 3300 Eger');
    testit('Helga Skaptadóttir, Ránargata 14, 101 Reykjavík');
    testit('र  ुक  ्म  िण  ी च  ौह  ान, 12 खज  ूर म  ार  ्ग, 305001 अजम  ेर');
    testit('Ratna Pramudita, Gg. Sedap Malam 4, 80235 Denpasar');
    testit('水野 ちとせ, 京都市左京区鹿ケ谷桜谷町25-7, 606-8443 京都');
    testit('زاہد حسین, گلی برسات ۵۲, ۵۴۰۰۰ لاہور');
    testit('Maite Cifuentes, Calle Camarones 6234, C1419 Buenos Aires');
    testit('Wouter Baks, Noorderdwarsstraat 2, 3513 Utrecht');
    testit('Tomáš Konečný, Lesní 14, 739 81 Bystřice nad Olší');

</script>
```

This produces output like this

```
Unpacked: 47 Packed: 25 Compression: 53% Emily Watson, 47 Wattle Crescent, 2602 Ainslie
Unpacked: 59 Packed: 46 Compression: 77% 蒋青莲, 云岩区燕雀巷17号碧霄楼, 550002 贵阳
Unpacked: 47 Packed: 28 Compression: 59% Rigmor Kjeldsen, Møllestien 14, 8000 Aarhus C
Unpacked: 75 Packed: 41 Compression: 54% عائشة محروس, زقاق المدابغ ٣, ١١٦٢٢ الجيزة
Unpacked: 48 Packed: 30 Compression: 62% Unto Kuusisto, Paimenenpolku 7 A 4, 66950 Jurva
Unpacked: 77 Packed: 36 Compression: 46% Léontine Perrault, 4 Impasse des Rossignols, 17110 Saint-Georges-de-Didonne
Unpacked: 52 Packed: 29 Compression: 55% Ulrich Wiesengrund, Finkenrain 9, 79199 Kirchzarten
Unpacked: 44 Packed: 26 Compression: 59% Zsombor Holló, Rákóczi-rét 3, 3300 Eger
Unpacked: 51 Packed: 35 Compression: 68% Helga Skaptadóttir, Ránargata 14, 101 Reykjavík
Unpacked: 108 Packed: 69 Compression: 63% र ुक ्म िण ी च ौह ान, 12 खज ूर म ार ्ग, 305001 अजम ेर
Unpacked: 51 Packed: 33 Compression: 64% Ratna Pramudita, Gg. Sedap Malam 4, 80235 Denpasar
Unpacked: 76 Packed: 43 Compression: 56% 水野 ちとせ, 京都市左京区鹿ケ谷桜谷町25-7, 606-8443 京都
Unpacked: 65 Packed: 45 Compression: 69% زاہد حسین, گلی برسات ۵۲, ۵۴۰۰۰ لاہور
Unpacked: 58 Packed: 35 Compression: 60% Maite Cifuentes, Calle Camarones 6234, C1419 Buenos Aires
Unpacked: 48 Packed: 26 Compression: 54% Wouter Baks, Noorderdwarsstraat 2, 3513 Utrecht
Unpacked: 58 Packed: 36 Compression: 62% Tomáš Konečný, Lesní 14, 739 81 Bystřice nad Olší

```
# useful hints and options

Without other arguments, bpe_encode tries all availabled training data to see which one produces smallest result.
This adds significant cost on processing speed. To improve compression speed, you can either specify lower limit
on traning data used (providing as a percentage, while still testing all of the trained models), or you can
select particular model manually by providing country code. Or both.

    let e=bpe_encode(s);            // without parameters - try all models, no limit, slow but best compression
    let e=bpe_encode(s, 30);        // first parameter specifies 30% limit on training data (merged pairs), still tries all models
    let e=bpe_encode(s, 100, 'us'); // uses 100% of training data for US (United States) only
    let e=bpe_encode(s, 10, 'gb');  // uses only initial 10% of training data for GB (United Kingdom) only

Note that, for example, compressing an Italian text with Chinese training data (cn) brings no compression at all.

When the encoded data is bigger than the original, it returns the original text inflated by 1 null byte at the beginning.
So the compressed data is never bigger than 100% of the original plus 1 byte.

# Training

If you want to train your own models, you can do so by using the train() function.
This code is included in bpezip.js but is commented out. See the code section around line 300.
(it assumes you run it with nodejs and loads texts for training from corpus.txt)
