# bpezip
compression of short texts using bpe encoding, in javascript

Efficient compression algorithms are essential for optimizing digital storage and data transmission.
Short texts, such as street names, URLs, or brief messages, are notably challenging to compress
effectively using traditional methods. The Byte Pair Encoding (BPE) approach tailored specifically
for short-text compression addresses this problem effectively.

This novel compression method involves training encoding models on specific data categories, in
this instance, street names from 110 different countries. Byte Pair Encoding identifies frequent
pairs of bytes (or characters) within the text corpus and repeatedly merges them into new tokens.
Through iterative enhancement of its vocabulary by merging these frequent pairs, the algorithm
efficiently encodes common sequences into shorter representations.

Technically, the algorithm begins by encoding each character individually. It then counts occurrences
of adjacent byte pairs across the training corpus, identifies the most frequent pair, and merges it
into a single token. This process repeats until reaching a predefined vocabulary limit or frequency
threshold. The result is a vocabulary optimized specifically for the patterns found in the training data.

Training separate models for each country is advantageous because linguistic patterns, naming
conventions, and common substrings differ widely between cultures and languages. Country-specific
BPE models capture and encode patterns unique to each dataset, significantly enhancing compression
efficiency.

An intelligent aspect of this implementation is its flexibility. If a country is not specified during
encoding, the algorithm automatically tries encoding using all available country-specific models,
choosing the smallest output. This approach ensures optimal compression for all inputs. Additionally,
a configurable 'limit' parameter enables users to control the depth of compression, balancing
efficiency and speed.

Another crucial component is the TightInts encoding technique, designed for compressing sequences of
integers through delta encoding and bit-packing. TightInts efficiently reduces the size of internal
integer representations by storing values as differences from a minimum reference value, significantly
reducing data size for numerical arrays.

Advantages of this compression approach include highly efficient representation of short texts,
adaptability to various linguistic datasets, and flexibility through the automatic selection of
the best encoding model. However, potential disadvantages include increased computational overhead
during the training phase and the necessity of maintaining multiple country-specific encoding models,
which may require additional storage and management.

The result is a highly compact representation of short texts, making it ideal for bandwidth-limited
applications such as IoT devices, low-bandwidth networks, or extensive databases containing numerous
small entries. With its adaptive, country-aware, and optimized approach, this BPE-based compression
algorithm provides a significant advancement in data efficiency, especially addressing the challenge
of compressing short textual data.

Example:
```
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
on traning data used (while still testing all of the trained models), or you can select particular model
manually by providing country code.

    let e=bpe_encode(s);            // without parameters - try all models, no limit, slow but best compression
    let e=bpe_encode(s, 30);        // first parameter specifies 30% limit on training data (merged pairs), still tries all models
    let e=bpe_encode(s, 100, 'us'); // uses 100% of training data for US (United States) only
    let e=bpe_encode(s, 10, 'gb');  // uses only initial 10% of training data for GB (United Kingdom) only

Note that, for example, compressing an Italian text with Chinese training data (cn) brings no compression at all.

When the encoded data is bigger than the original, it returns the original text inflated by 1 null byte at the beginning.
So the compressed data is never bigger than 100% of the original plus 1 byte.
