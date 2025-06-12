import React, { useState, useEffect } from 'react';

// Bileşen Verileri (PassMark ve göreceli puanlar)
const cpuData = [
    { id: 'i5-2400', name: 'Intel Core i5-2400', score: 3870, details: '4 Çekirdek, 4 İzlek' },
    { id: 'i7-4790', name: 'Intel Core i7-4790', score: 7266, details: '4 Çekirdek, 8 İzlek' },
    { id: 'r5-2600x', name: 'AMD Ryzen 5 2600X', score: 13800, details: '6 Çekirdek, 12 İzlek' },
    { id: 'i5-10400f', name: 'Intel Core i5-10400F', score: 12127, details: '6 Çekirdek, 12 İzlek' },
    { id: 'r5-3600', name: 'AMD Ryzen 5 3600', score: 17750, details: '6 Çekirdek, 12 İzlek' }, // Yeni
    { id: 'i5-11600k', name: 'Intel Core i5-11600K', score: 21500, details: '6 Çekirdek, 12 İzlek' }, // Yeni
    { id: 'r5-5600x', name: 'AMD Ryzen 5 5600X', score: 21873, details: '6 Çekirdek, 12 İzlek' },
    { id: 'i7-12700k', name: 'Intel Core i7-12700K', score: 34967, details: '12 Çekirdek, 20 İzlek' },
    { id: 'r7-7800x3d', name: 'AMD Ryzen 7 7800X3D', score: 48674, details: '8 Çekirdek, 16 İzlek' },
    { id: 'i9-14900k', name: 'Intel Core i9-14900K', score: 62000, details: '24 Çekirdek, 32 İzlek' },
    { id: 'r9-7950x3d', name: 'AMD Ryzen 9 7950X3D', score: 63500, details: '16 Çekirdek, 32 İzlek' }, // Yeni
];

const gpuData = [
    { id: 'gtx-950', name: 'NVIDIA GeForce GTX 950', score: 5346, details: '2GB GDDR5' },
    { id: 'gtx-960', name: 'NVIDIA GeForce GTX 960', score: 8000, details: '2GB GDDR5' },
    { id: 'rx-570', name: 'AMD Radeon RX 570', score: 12000, details: '8GB GDDR5' },
    { id: 'gtx-1660s', name: 'NVIDIA GeForce GTX 1660 SUPER', score: 12500, details: '6GB GDDR6' },
    { id: 'rtx-2060', name: 'NVIDIA GeForce RTX 2060', score: 14125, details: '6GB GDDR6' },
    { id: 'rx-6600xt', name: 'AMD Radeon RX 6600 XT', score: 18000, details: '8GB GDDR6' },
    { id: 'rtx-3060', name: 'NVIDIA GeForce RTX 3060', score: 19500, details: '12GB GDDR6' }, // Yeni
    { id: 'rx-6700xt', name: 'AMD Radeon RX 6700 XT', score: 22000, details: '12GB GDDR6' }, // Yeni
    { id: 'rtx-3070', name: 'NVIDIA GeForce RTX 3070', score: 22291, details: '8GB GDDR6' },
    { id: 'rtx-3080', name: 'NVIDIA GeForce RTX 3080', score: 25180, details: '10GB GDDR6X' }, // Yeni
    { id: 'rx-6800xt', name: 'AMD Radeon RX 6800 XT', score: 29000, details: '16GB GDDR6' }, // Yeni
    { id: 'rtx-4070', name: 'NVIDIA GeForce RTX 4070', score: 30000, details: '12GB GDDR6X' }, // Yeni
    { id: 'rtx-4080', name: 'NVIDIA GeForce RTX 4080', score: 34440, details: '16GB GDDR6X' },
    { id: 'rtx-4090', name: 'NVIDIA GeForce RTX 4090', score: 42000, details: '24GB GDDR6X' }, // Yeni
];

const ramData = [
    { id: '8gb-ddr3-1333', name: '8GB DDR3 1333MHz', score: 25 },
    { id: '16gb-ddr3-1600', name: '16GB DDR3 1600MHz', score: 40 },
    { id: '8gb-ddr4-2666', name: '8GB DDR4 2666MHz', score: 50 },
    { id: '16gb-ddr4-3200', name: '16GB DDR4 3200MHz', score: 70 },
    { id: '32gb-ddr4-3200', name: '32GB DDR4 3200MHz', score: 90 },
    { id: '32gb-ddr5-6000', name: '32GB DDR5 6000MHz', score: 100 },
    { id: '64gb-ddr5-6000', name: '64GB DDR5 6000MHz', score: 110 },
];

const storageData = [
    { id: '500gb-hdd', name: '500GB HDD', score: 10 },
    { id: '128gb-ssd-500gb-hdd', name: '128GB SSD + 500GB HDD', score: 50 },
    { id: '256gb-ssd', name: '256GB SSD', score: 60 },
    { id: '500gb-ssd', name: '500GB SSD', score: 85 },
    { id: '1tb-nvme', name: '1TB NVMe SSD', score: 100 },
    { id: '2tb-nvme', name: '2TB NVMe SSD', score: 110 },
    { id: '4tb-nvme', name: '4TB NVMe SSD', score: 120 }, // Yeni
];

const psuData = [
    { id: '400w', name: '400W', score: 45 },
    { id: '550w', name: '550W', score: 60 },
    { id: '650w', name: '650W', score: 70 },
    { id: '750w', name: '750W', score: 80 },
    { id: '850w', name: '850W', score: 90 },
    { id: '1000w', name: '1000W', score: 100 },
    { id: '1200w', name: '1200W', score: 110 }, // Yeni
];

// Maksimum puanları dinamik olarak hesaplar (çubuk yüksekliklerini oranlamak için)
const calculateMaxScores = () => {
    return {
        cpu: Math.max(...cpuData.map(c => c.score)),
        gpu: Math.max(...gpuData.map(g => g.score)),
        ram: Math.max(...ramData.map(r => r.score)),
        storage: Math.max(...storageData.map(s => s.score)),
        psu: Math.max(...psuData.map(p => p.score)),
    };
};

const maxScores = calculateMaxScores();

// Darboğaz hesaplama fonksiyonu
const calculateBottleneck = (cpuScore, gpuScore) => {
    if (!cpuScore || !gpuScore) {
        return { percentage: null, explanation: "Hesaplamak için hem CPU hem de GPU seçilmelidir." };
    }

    const diff = Math.abs(cpuScore - gpuScore);
    const maxVal = Math.max(cpuScore, gpuScore);

    if (maxVal === 0) {
        return { percentage: 0, explanation: "Puanlar sıfır olduğu için darboğaz hesaplanamadı." };
    }

    const percentage = (diff / maxVal) * 100;
    let explanation = "";
    let bottleneckComponent = "";

    if (cpuScore < gpuScore) {
        bottleneckComponent = "İşlemci (CPU)";
    } else if (gpuScore < cpuScore) {
        bottleneckComponent = "Ekran Kartı (GPU)";
    } else {
        bottleneckComponent = "Dengeli";
    }

    if (percentage < 5) {
        explanation = "Çok az veya hiç darboğaz yok. Sistem oldukça dengeli.";
    } else if (percentage < 15) {
        explanation = "Minimal darboğaz. Çoğu senaryoda fark edilmez ancak küçük bir bileşen dengesizliği var.";
    } else if (percentage < 25) {
        explanation = "Orta seviye darboğaz. Bazı oyunlarda veya uygulamalarda hissedilebilir bir performans düşüşü yaşanabilir.";
    } else {
        explanation = "Yüksek darboğaz. Seçilen " + bottleneckComponent + " diğer bileşeni önemli ölçüde kısıtlayabilir, performans büyük ölçüde etkilenebilir.";
    }

    return {
        percentage: percentage.toFixed(1),
        explanation: `${bottleneckComponent} odaklı darboğaz: ${explanation}`,
    };
};

// Çubuk bileşeni
const ComparisonBar = ({ label, score1, score2, maxScore, unit = '' }) => {
    const height1 = (score1 / maxScore) * 100;
    const height2 = (score2 / maxScore) * 100;

    return (
        <div className="mb-6">
            <h3 className="text-lg font-semibold text-gray-700 mb-2">{label}</h3>
            <div className="flex justify-between items-end h-36 bg-gray-100 rounded-lg overflow-hidden p-2">
                {/* Sistem 1 Çubuğu */}
                <div className="flex-1 flex flex-col justify-end items-center mx-2 relative">
                    <div
                        className="w-full bg-indigo-500 rounded-t-md transition-all duration-500 ease-out flex items-center justify-center text-white font-bold text-sm"
                        style={{ height: `${Math.min(height1, 100)}%`, minHeight: '20px' }}
                    >
                        {score1}{unit}
                    </div>
                </div>

                {/* Sistem 2 Çubuğu */}
                <div className="flex-1 flex flex-col justify-end items-center mx-2 relative">
                    <div
                        className="w-full bg-emerald-500 rounded-t-md transition-all duration-500 ease-out flex items-center justify-center text-white font-bold text-sm"
                        style={{ height: `${Math.min(height2, 100)}%`, minHeight: '20px' }}
                    >
                        {score2}{unit}
                    </div>
                </div>
            </div>
        </div>
    );
};

function App() {
    // Sistem 1 Bileşenleri
    const [s1CpuId, setS1CpuId] = useState('');
    const [s1GpuId, setS1GpuId] = useState('');
    const [s1RamId, setS1RamId] = useState('');
    const [s1StorageId, setS1StorageId] = useState('');
    const [s1PsuId, setS1PsuId] = useState('');

    // Sistem 2 Bileşenleri
    const [s2CpuId, setS2CpuId] = useState('');
    const [s2GpuId, setS2GpuId] = useState('');
    const [s2RamId, setS2RamId] = useState('');
    const [s2StorageId, setS2StorageId] = useState('');
    const [s2PsuId, setS2PsuId] = useState('');

    const [showComparison, setShowComparison] = useState(false);
    const [errorMessage, setErrorMessage] = useState('');

    // Seçilen bileşenlerin verilerini al
    const getComponent = (id, data) => data.find(item => item.id === id);

    const s1Cpu = getComponent(s1CpuId, cpuData);
    const s1Gpu = getComponent(s1GpuId, gpuData);
    const s1Ram = getComponent(s1RamId, ramData);
    const s1Storage = getComponent(s1StorageId, storageData);
    const s1Psu = getComponent(s1PsuId, psuData);

    const s2Cpu = getComponent(s2CpuId, cpuData);
    const s2Gpu = getComponent(s2GpuId, gpuData);
    const s2Ram = getComponent(s2RamId, ramData);
    const s2Storage = getComponent(s2StorageId, storageData);
    const s2Psu = getComponent(s2PsuId, psuData);

    const handleCompare = () => {
        // En azından CPU ve GPU seçili olmalı ki anlamlı bir karşılaştırma yapılabilsin
        if (!s1Cpu || !s1Gpu || !s2Cpu || !s2Gpu) {
            setErrorMessage("Lütfen her iki sistem için de İşlemci ve Ekran Kartı seçimi yapın.");
            setShowComparison(false);
            return;
        }
        setErrorMessage('');
        setShowComparison(true);
    };

    // Darboğaz hesaplamaları
    const s1Bottleneck = calculateBottleneck(s1Cpu?.score, s1Gpu?.score);
    const s2Bottleneck = calculateBottleneck(s2Cpu?.score, s2Gpu?.score);

    return (
        <div className="min-h-screen bg-gray-100 flex items-center justify-center p-6 font-inter">
            <div className="bg-white rounded-xl shadow-2xl p-8 max-w-6xl w-full">
                <h1 className="text-4xl font-extrabold text-center text-gray-900 mb-8">Özel Sistem Karşılaştırma Aracı</h1>

                {/* Sistem Seçim Alanı */}
                <div className="flex flex-col md:flex-row justify-center items-start gap-8 mb-8">
                    {/* Sistem 1 Bileşen Seçimi */}
                    <div className="w-full md:w-1/2 bg-indigo-50 p-6 rounded-lg shadow-md">
                        <h2 className="text-2xl font-bold text-indigo-700 mb-6 text-center">Sistem 1</h2>
                        {[
                            { label: 'İşlemci (CPU)', data: cpuData, value: s1CpuId, onChange: setS1CpuId },
                            { label: 'Ekran Kartı (GPU)', data: gpuData, value: s1GpuId, onChange: setS1GpuId },
                            { label: 'Bellek (RAM)', data: ramData, value: s1RamId, onChange: setS1RamId },
                            { label: 'Depolama', data: storageData, value: s1StorageId, onChange: setS1StorageId },
                            { label: 'Güç Kaynağı (PSU)', data: psuData, value: s1PsuId, onChange: setS1PsuId },
                        ].map((field) => (
                            <div className="mb-4" key={field.label}>
                                <label htmlFor={`s1-${field.label}`} className="block text-md font-medium text-gray-700 mb-1">
                                    {field.label}:
                                </label>
                                <select
                                    id={`s1-${field.label}`}
                                    className="block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-md rounded-md shadow-sm"
                                    value={field.value}
                                    onChange={(e) => {
                                        field.onChange(e.target.value);
                                        setShowComparison(false); // Seçim değişince karşılaştırmayı sıfırla
                                        setErrorMessage('');
                                    }}
                                >
                                    <option value="">-- Seçin --</option>
                                    {field.data.map((item) => (
                                        <option key={item.id} value={item.id}>
                                            {item.name} {item.details ? `(${item.details})` : ''}
                                        </option>
                                    ))}
                                </select>
                            </div>
                        ))}
                    </div>

                    {/* Sistem 2 Bileşen Seçimi */}
                    <div className="w-full md:w-1/2 bg-emerald-50 p-6 rounded-lg shadow-md">
                        <h2 className="text-2xl font-bold text-emerald-700 mb-6 text-center">Sistem 2</h2>
                        {[
                            { label: 'İşlemci (CPU)', data: cpuData, value: s2CpuId, onChange: setS2CpuId },
                            { label: 'Ekran Kartı (GPU)', data: gpuData, value: s2GpuId, onChange: setS2GpuId },
                            { label: 'Bellek (RAM)', data: ramData, value: s2RamId, onChange: setS2RamId },
                            { label: 'Depolama', data: storageData, value: s2StorageId, onChange: setS2StorageId },
                            { label: 'Güç Kaynağı (PSU)', data: psuData, value: s2PsuId, onChange: setS2PsuId },
                        ].map((field) => (
                            <div className="mb-4" key={field.label}>
                                <label htmlFor={`s2-${field.label}`} className="block text-md font-medium text-gray-700 mb-1">
                                    {field.label}:
                                </label>
                                <select
                                    id={`s2-${field.label}`}
                                    className="block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-emerald-500 focus:border-emerald-500 sm:text-md rounded-md shadow-sm"
                                    value={field.value}
                                    onChange={(e) => {
                                        field.onChange(e.target.value);
                                        setShowComparison(false); // Seçim değişince karşılaştırmayı sıfırla
                                        setErrorMessage('');
                                    }}
                                >
                                    <option value="">-- Seçin --</option>
                                    {field.data.map((item) => (
                                        <option key={item.id} value={item.id}>
                                            {item.name} {item.details ? `(${item.details})` : ''}
                                        </option>
                                    ))}
                                </select>
                            </div>
                        ))}
                    </div>
                </div>

                <div className="flex justify-center mb-10">
                    <button
                        onClick={handleCompare}
                        className="px-8 py-4 bg-gradient-to-r from-blue-600 to-indigo-700 text-white font-bold rounded-lg shadow-lg hover:from-blue-700 hover:to-indigo-800 transition transform hover:scale-105 focus:outline-none focus:ring-4 focus:ring-blue-300"
                    >
                        Sistemleri Karşılaştır
                    </button>
                </div>

                {errorMessage && (
                    <div className="mt-8 p-4 bg-red-100 border border-red-400 text-red-700 rounded-md text-center">
                        {errorMessage}
                    </div>
                )}

                {/* Karşılaştırma Sonuçları */}
                {showComparison && (s1Cpu && s1Gpu && s2Cpu && s2Gpu) && (
                    <div className="border-t-2 border-gray-200 pt-8 mt-8">
                        <h2 className="text-3xl font-bold text-center text-gray-800 mb-6">
                            Sistem 1 <span className="text-indigo-500">vs</span> Sistem 2 Performans Karşılaştırması
                        </h2>

                        {/* Genel Bilgiler */}
                        <div className="grid grid-cols-1 md:grid-cols-2 gap-x-8 gap-y-6 mb-8">
                            <div className="bg-indigo-50 p-6 rounded-lg shadow-md flex flex-col justify-between">
                                <h3 className="text-xl font-bold text-indigo-700 mb-4">Sistem 1 Bilgileri</h3>
                                <p className="text-gray-700">
                                    <span className="font-semibold">İşlemci:</span> {s1Cpu?.name} {s1Cpu?.details && `(${s1Cpu.details})`}
                                </p>
                                <p className="text-gray-700">
                                    <span className="font-semibold">Ekran Kartı:</span> {s1Gpu?.name} {s1Gpu?.details && `(${s1Gpu.details})`}
                                </p>
                                <p className="text-gray-700">
                                    <span className="font-semibold">RAM:</span> {s1Ram?.name || 'Seçilmedi'}
                                </p>
                                <p className="text-gray-700">
                                    <span className="font-semibold">Depolama:</span> {s1Storage?.name || 'Seçilmedi'}
                                </p>
                                <p className="text-gray-700">
                                    <span className="font-semibold">PSU:</span> {s1Psu?.name || 'Seçilmedi'}
                                </p>
                            </div>

                            <div className="bg-emerald-50 p-6 rounded-lg shadow-md flex flex-col justify-between">
                                <h3 className="text-xl font-bold text-emerald-700 mb-4">Sistem 2 Bilgileri</h3>
                                <p className="text-gray-700">
                                    <span className="font-semibold">İşlemci:</span> {s2Cpu?.name} {s2Cpu?.details && `(${s2Cpu.details})`}
                                </p>
                                <p className="text-gray-700">
                                    <span className="font-semibold">Ekran Kartı:</span> {s2Gpu?.name} {s2Gpu?.details && `(${s2Gpu.details})`}
                                </p>
                                <p className="text-gray-700">
                                    <span className="font-semibold">RAM:</span> {s2Ram?.name || 'Seçilmedi'}
                                </p>
                                <p className="text-gray-700">
                                    <span className="font-semibold">Depolama:</span> {s2Storage?.name || 'Seçilmedi'}
                                </p>
                                <p className="text-gray-700">
                                    <span className="font-semibold">PSU:</span> {s2Psu?.name || 'Seçilmedi'}
                                </p>
                            </div>
                        </div>


                        {/* Bileşen Bazında Karşılaştırma Barları */}
                        <div className="mt-8">
                            <ComparisonBar
                                label="İşlemci (CPU) Performansı"
                                score1={s1Cpu?.score || 0}
                                score2={s2Cpu?.score || 0}
                                maxScore={maxScores.cpu}
                            />
                            <ComparisonBar
                                label="Ekran Kartı (GPU) Performansı"
                                score1={s1Gpu?.score || 0}
                                score2={s2Gpu?.score || 0}
                                maxScore={maxScores.gpu}
                            />
                            <ComparisonBar
                                label="Bellek (RAM) Performansı"
                                score1={s1Ram?.score || 0}
                                score2={s2Ram?.score || 0}
                                maxScore={maxScores.ram}
                            />
                            <ComparisonBar
                                label="Depolama Performansı"
                                score1={s1Storage?.score || 0}
                                score2={s2Storage?.score || 0}
                                maxScore={maxScores.storage}
                            />
                            <ComparisonBar
                                label="Güç Kaynağı (PSU) Yeterliliği"
                                score1={s1Psu?.score || 0}
                                score2={s2Psu?.score || 0}
                                maxScore={maxScores.psu}
                            />
                        </div>

                        {/* Darboğaz Sonuçları */}
                        <div className="mt-10 p-6 bg-purple-50 border border-purple-200 rounded-lg text-purple-800">
                            <h3 className="text-xl font-semibold mb-3">Darboğaz Analizi:</h3>
                            <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                                <div>
                                    <p className="font-bold text-lg mb-1">Sistem 1 Darboğaz:</p>
                                    {s1Bottleneck.percentage !== null ? (
                                        <>
                                            <p className="text-2xl font-bold text-purple-700">{s1Bottleneck.percentage}%</p>
                                            <p className="text-md">{s1Bottleneck.explanation}</p>
                                        </>
                                    ) : (
                                        <p className="text-md">Darboğaz hesaplamak için İşlemci ve Ekran Kartı seçilmelidir.</p>
                                    )}
                                </div>
                                <div>
                                    <p className="font-bold text-lg mb-1">Sistem 2 Darboğaz:</p>
                                    {s2Bottleneck.percentage !== null ? (
                                        <>
                                            <p className="text-2xl font-bold text-purple-700">{s2Bottleneck.percentage}%</p>
                                            <p className="text-md">{s2Bottleneck.explanation}</p>
                                        </>
                                    ) : (
                                        <p className="text-md">Darboğaz hesaplamak için İşlemci ve Ekran Kartı seçilmelidir.</p>
                                    )}
                                </div>
                            </div>
                        </div>

                        <div className="mt-10 p-6 bg-blue-50 border border-blue-200 rounded-lg text-blue-800">
                            <h3 className="text-xl font-semibold mb-3">Puanlama Hakkında Not:</h3>
                            <p className="mb-2">
                                <span className="font-bold">CPU</span> ve <span className="font-bold">GPU</span> puanları, yaklaşık PassMark benchmark sonuçlarına dayanmaktadır. Bu puanlar, bileşenlerin teorik performansını gösterir ve gerçek dünya performansı farklılık gösterebilir.
                            </p>
                            <p className="mb-2">
                                <span className="font-bold">RAM, Depolama</span> ve <span className="font-bold">PSU</span> için ise sistemler arasındaki
                                <span className="font-bold">göreceli performansı ve güncelliği</span> yansıtan basit puanlar atanmıştır (daha yüksek puan daha iyi anlamına gelir). Bu puanlar doğrudan benchmark testlerinden alınmamıştır, genel bir referans olarak kullanılabilir.
                            </p>
                            <p className="mb-2">
                                <span className="font-bold">Darboğaz yüzdesi</span>, İşlemci ve Ekran Kartı puanları arasındaki farka dayanarak hesaplanır. Daha yüksek bir yüzde, bileşenler arasında daha büyük bir performans dengesizliği olduğunu gösterir. Bu, sistemin genel performansının bir bileşen tarafından kısıtlandığı anlamına gelebilir.
                            </p>
                            <ul className="list-disc list-inside mt-3">
                                <li>Örnek CPU Skorları (Yaklaşık PassMark CPU Mark):
                                    <ul className="list-disc list-inside ml-4">
                                        <li>Intel Core i7-4790: ~7266</li>
                                        <li>AMD Ryzen 5 2600X: ~13800</li>
                                        <li>Intel Core i9-14900K: ~62000</li>
                                    </ul>
                                </li>
                                <li>Örnek GPU Skorları (Yaklaşık PassMark G3D Mark):
                                    <ul className="list-disc list-inside ml-4">
                                        <li>NVIDIA GeForce GTX 960: ~8000</li>
                                        <li>AMD Radeon RX 570: ~12000</li>
                                        <li>NVIDIA GeForce RTX 4080: ~34440</li>
                                    </ul>
                                </li>
                            </ul>
                        </div>
                    </div>
                )}
            </div>
        </div>
    );
}

export default App;
