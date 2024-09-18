import React, { useState, useMemo } from 'react';
import { BarChart, Bar, XAxis, YAxis, Tooltip, CartesianGrid, Legend, ResponsiveContainer, ReferenceLine } from 'recharts';
import { Card, CardHeader, CardContent, CardDescription } from '@/components/ui/card';
import { Input } from '@/components/ui/input';

const rawData = [
  { name: "xAI (expected)", powerMin: 125, powerMax: 175, daysMin: 60, daysMax: 90, date: "2025-01-01" },
  { name: "GPT-5 (expected)", powerMin: 60, powerMax: 90, daysMin: 60, daysMax: 90, date: "2025-06-01" },
  { name: "Llama 3-70B", power: 24.71, days: 30, date: "2024-04-18" },
  { name: "Llama 3.1-405B", power: 24.69, days: 30, date: "2024-07-23" },
  { name: "Gemini 1.0 Ultra", power: 23.33, days: 30, date: "2023-12-06" },
  { name: "GPT-4", power: 22.15, days: 30, date: "2023-03-15" },
  { name: "Amazon Titan", power: 12.17, days: 30, date: "2023-09-28" },
  { name: "MegaScale (Production)", power: 10.85, days: 30, date: "2024-02-23" },
  { name: "Inflection-2", power: 7.73, days: 30, date: "2023-11-22" },
  { name: "GPT-3 175B", power: 5.59, days: 30, date: "2020-05-28" },
  { name: "Gopher (280B)", power: 4.10, days: 30, date: "2021-12-08" },
  { name: "Megatron-Turing NLG 530B", power: 3.99, days: 30, date: "2021-10-11" },
  { name: "Falcon-180B", power: 3.62, days: 30, date: "2023-09-06" },
  { name: "AlphaZero", power: 3.21, days: 30, date: "2017-12-05" },
  { name: "PaLM (540B)", power: 2.62, days: 30, date: "2022-04-04" },
  { name: "ERNIE 3.0 Titan", power: 1.32, days: 30, date: "2021-12-23" },
  { name: "Meena", power: 1.03, days: 30, date: "2020-01-28" }
];

const AIEnergyDashboard = () => {
  const [threshold, setThreshold] = useState(75);
  const [days, setDays] = useState(30);

  const energyThreshold = threshold * 24 * days;

  const processedData = useMemo(() => {
    return rawData.map(model => {
      const energyMin = model.powerMin 
        ? model.powerMin * 24 * model.daysMin 
        : model.power * 24 * days;
      const energyMax = model.powerMax 
        ? model.powerMax * 24 * model.daysMax 
        : model.power * 24 * days;
      return {
        ...model,
        energyMin,
        energyMax,
        exceedsThreshold: energyMax > energyThreshold
      };
    }).sort((a, b) => b.energyMax - a.energyMax);
  }, [days, energyThreshold]);

  const CustomTooltip = ({ active, payload, label }) => {
    if (active && payload && payload.length) {
      const data = payload[0].payload;
      return (
        <div className="bg-white p-4 border rounded shadow-lg">
          <p className="font-bold">{data.name}</p>
          <p>Energy: {data.energyMin.toFixed(2)} - {data.energyMax.toFixed(2)} MWh</p>
          <p>Power: {data.powerMin || data.power} - {data.powerMax || data.power} MW</p>
          <p>Days: {data.daysMin || days} - {data.daysMax || days}</p>
          <p>Date: {data.date}</p>
          <p className="font-semibold">
            {data.exceedsThreshold ? "Exceeds" : "Below"} Threshold
          </p>
        </div>
      );
    }
    return null;
  };

  return (
    <Card className="w-full max-w-4xl mx-auto">
      <CardHeader>
        <h2 className="text-2xl font-bold">AI Model Energy Consumption Dashboard</h2>
        <CardDescription>
          Explore the energy consumption of AI models based on different thresholds and time periods.
        </CardDescription>
      </CardHeader>
      <CardContent>
        <div className="flex gap-4 mb-4">
          <div>
            <label htmlFor="threshold" className="block text-sm font-medium text-gray-700">Power Threshold (MW)</label>
            <Input
              id="threshold"
              type="number"
              value={threshold}
              onChange={(e) => setThreshold(Number(e.target.value))}
              className="mt-1"
            />
          </div>
          <div>
            <label htmlFor="days" className="block text-sm font-medium text-gray-700">Number of Days</label>
            <Input
              id="days"
              type="number"
              value={days}
              onChange={(e) => setDays(Number(e.target.value))}
              className="mt-1"
            />
          </div>
        </div>
        <p className="mb-4">
          Energy Threshold: {energyThreshold.toFixed(2)} MWh over {days} days
        </p>
        <ResponsiveContainer width="100%" height={500}>
          <BarChart
            data={processedData}
            layout="vertical"
            margin={{ top: 5, right: 30, left: 150, bottom: 5 }}
          >
            <CartesianGrid strokeDasharray="3 3" />
            <XAxis type="number" label={{ value: 'Energy Consumption (MWh)', position: 'insideBottom', offset: -5 }} />
            <YAxis dataKey="name" type="category" width={140} />
            <Tooltip content={<CustomTooltip />} />
            <Legend />
            <Bar dataKey="energyMin" fill="#8884d8" name="Min Energy" />
            <Bar dataKey="energyMax" fill="#82ca9d" name="Max Energy" />
            <ReferenceLine x={energyThreshold} stroke="red" label="Threshold" />
          </BarChart>
        </ResponsiveContainer>
      </CardContent>
    </Card>
  );
};

export default AIEnergyDashboard;
