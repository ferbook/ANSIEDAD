import React, { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import { BarChart, Bar, XAxis, YAxis, Tooltip, ResponsiveContainer } from "recharts";

const emotions = [
  { label: "Alegría", color: "bg-yellow-300" },
  { label: "Tristeza", color: "bg-blue-300" },
  { label: "Ira", color: "bg-red-400" },
  { label: "Ansiedad", color: "bg-purple-300" },
  { label: "Calma", color: "bg-green-300" },
  { label: "Amor", color: "bg-pink-300" },
  { label: "Confusión", color: "bg-gray-300" },
  { label: "Desmotivación", color: "bg-zinc-300" },
  { label: "Agitación", color: "bg-orange-300" },
  { label: "Serenidad", color: "bg-teal-300" },
  { label: "Inspiración", color: "bg-lime-300" },
  { label: "Euforia", color: "bg-rose-300" },
  { label: "Frialdad emocional", color: "bg-slate-300" }
];

export default function MiDiarioEmocional() {
  const [entries, setEntries] = useState([]);
  const [note, setNote] = useState("");

  const handleEmotionClick = (emotion) => {
    const time = new Date().toLocaleTimeString([], { hour: "2-digit", minute: "2-digit" });
    const date = new Date().toLocaleDateString();
    const newEntry = { emotion, time, date, note };
    setEntries([...entries, newEntry]);
    setNote("");
  };

  const emotionCounts = emotions.map(({ label }) => ({
    name: label,
    value: entries.filter(e => e.emotion === label).length
  })).filter(e => e.value > 0);

  return (
    <div className="min-h-screen p-4 bg-white">
      <h1 className="text-3xl font-bold text-center mb-4">Mi Diario Emocional</h1>

      <div className="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-2 mb-4">
        {emotions.map(({ label, color }) => (
          <Button
            key={label}
            className={`${color} text-black font-semibold`}
            onClick={() => handleEmotionClick(label)}
          >
            {label}
          </Button>
        ))}
      </div>

      <Textarea
        value={note}
        onChange={(e) => setNote(e.target.value)}
        placeholder="Escribe una nota o reflexión (opcional)"
        className="mb-4"
      />

      <Card className="mb-6">
        <CardContent>
          <h2 className="text-xl font-semibold mb-2">Resumen Diario</h2>
          {emotionCounts.length === 0 ? (
            <p className="text-gray-500">Aún no hay registros.</p>
          ) : (
            <ResponsiveContainer width="100%" height={250}>
              <BarChart data={emotionCounts}>
                <XAxis dataKey="name" />
                <YAxis allowDecimals={false} />
                <Tooltip />
                <Bar dataKey="value" fill="#8884d8" />
              </BarChart>
            </ResponsiveContainer>
          )}
        </CardContent>
      </Card>

      <h2 className="text-xl font-semibold mb-2">Historial</h2>
      <div className="space-y-2">
        {entries.map((entry, index) => (
          <Card key={index} className="border-l-4 pl-2" style={{ borderColor: emotions.find(e => e.label === entry.emotion)?.color }}>
            <CardContent>
              <p><strong>{entry.emotion}</strong> — {entry.time
