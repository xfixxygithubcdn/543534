const fs = require('fs');

// Orjinal SQL dosyasını okuyun
fs.readFile('sql.sql', 'utf8', (err, data) => {
    if (err) {
        console.error('Dosya okunurken bir hata oluştu:', err);
        return;
    }

    const veriler = data.split(';').filter(Boolean); // Sorguları ayır ve boşlukları filtrele

    const grupBuyuklugu = 1000000; // Her bir grup için sorgu sayısı

    for (let i = 0; i < veriler.length; i += grupBuyuklugu) {
        const grupVerileri = veriler.slice(i, i + grupBuyuklugu);
        const yeniDosyaAdi = `grup_${i / grupBuyuklugu + 1}.sql`;

        const grupSql = grupVerileri.join(';\n') + ';';

        fs.writeFileSync(yeniDosyaAdi, grupSql);

        console.log(`${yeniDosyaAdi} dosyası oluşturuldu.`);
    }
});
