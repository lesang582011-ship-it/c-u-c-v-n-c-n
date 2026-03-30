import React, { useState, useEffect } from "react";
import { motion } from "framer-motion";

// FINAL+: shop + quest + hiệu ứng
export default function BuildBoatUltimate() {
  const [coins, setCoins] = useState(0);
  const [owned, setOwned] = useState(["ninja"]);
  const [char, setChar] = useState("ninja");
  const [quest, setQuest] = useState({ goal: 200, progress: 0, reward: 150 });

  const characters = {
    ninja: { name: "🥷 Ninja", cost: 0 },
    tank: { name: "🛡️ Tank", cost: 200 },
    mage: { name: "🧙 Mage", cost: 300 }
  };

  // earn coins over time
  useEffect(() => {
    const t = setInterval(() => {
      setCoins((c) => c + 1);
      setQuest((q) => ({ ...q, progress: q.progress + 1 }));
    }, 100);
    return () => clearInterval(t);
  }, []);

  // complete quest
  useEffect(() => {
    if (quest.progress >= quest.goal) {
      setCoins((c) => c + quest.reward);
      setQuest({ goal: quest.goal + 200, progress: 0, reward: quest.reward + 100 });
    }
  }, [quest]);

  const buyChar = (id) => {
    if (owned.includes(id)) return;
    if (coins >= characters[id].cost) {
      setCoins(coins - characters[id].cost);
      setOwned([...owned, id]);
    }
  };

  return (
    <div className="p-6 flex flex-col items-center gap-4">
      <h1 className="text-2xl font-bold">🚀 Build Boat ULTIMATE</h1>

      <div className="flex gap-4">
        <div>💰 {coins}</div>
        <div>🧑 {characters[char].name}</div>
      </div>

      {/* Quest */}
      <div className="w-full max-w-md bg-white p-3 rounded-xl shadow">
        <div className="text-sm">🎯 Quest: {quest.progress}/{quest.goal}</div>
        <div className="text-xs">Reward: {quest.reward} coins</div>
      </div>

      {/* Shop */}
      <div className="w-full max-w-md bg-white p-3 rounded-xl shadow">
        <h2 className="font-bold">🛒 Shop</h2>
        {Object.keys(characters).map((id) => (
          <div key={id} className="flex justify-between text-sm">
            <span>{characters[id].name}</span>
            {owned.includes(id) ? (
              <button onClick={() => setChar(id)}>Chọn</button>
            ) : (
              <button onClick={() => buyChar(id)}>Mua ({characters[id].cost})</button>
            )}
          </div>
        ))}
      </div>

      {/* effect */}
      <motion.div
        className="w-32 h-32 bg-blue-400 rounded-full"
        animate={{ scale: [1, 1.2, 1] }}
        transition={{ repeat: Infinity, duration: 2 }}
      />

      <p className="text-sm text-gray-600">🔥 Game đã gần hoàn chỉnh 2D</p>
    </div>
  );
}
