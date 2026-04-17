import { useState, useEffect } from "react";
import { Plus, Minus, RotateCcw, Flag, Users } from "lucide-react";

const SCORE_ITEMS = [
  { id: "p_em", label: "Pung — Exposed Minor (2–8)", points: 2, group: "pung" },
  { id: "p_cm", label: "Pung — Concealed Minor", points: 4, group: "pung" },
  { id: "p_eM", label: "Pung — Exposed Major (Honours, 1 & 9)", points: 4, group: "pung" },
  { id: "p_cM", label: "Pung — Concealed Major", points: 8, group: "pung" },
  { id: "k_em", label: "Kong — Exposed Minor (2–8)", points: 8, group: "kong" },
  { id: "k_cm", label: "Kong — Concealed Minor", points: 16, group: "kong" },
  { id: "k_eM", label: "Kong — Exposed Major (Honours, 1 & 9)", points: 16, group: "kong" },
  { id: "k_cM", label: "Kong — Concealed Major", points: 32, group: "kong" },
  { id: "flowers", label: "All Flowers & Seasons", points: 4, group: "bonus" },
  { id: "dragons", label: "Any Pair of Dragons", points: 2, group: "bonus" },
  { id: "owner", label: "Pair of Owner's Wind", points: 2, group: "bonus" },
  { id: "round", label: "Pair of Wind of the Round", points: 2, group: "bonus" },
];

const PLAYER_COLORS = [
  { bg: "#c8553d", soft: "#f4d8d1", name: "East" },
  { bg: "#588157", soft: "#d9e5d8", name: "South" },
  { bg: "#3d5a80", soft: "#d4dbe6", name: "West" },
  { bg: "#bc8a5f", soft: "#ecdfd1", name: "North" },
];

const blankCounts = () =>
  SCORE_ITEMS.reduce((acc, item) => ({ ...acc, [item.id]: 0 }), {});

export default function GoulashScorer() {
  const [players, setPlayers] = useState(() =>
    PLAYER_COLORS.map((c) => ({ name: c.name, counts: blankCounts() }))
  );
  const [history, setHistory] = useState([]); // array of {round, scores: [n,n,n,n]}
  const [activePlayer, setActivePlayer] = useState(0);
  const [editingName, setEditingName] = useState(null);

  const calcScore = (counts) =>
    SCORE_ITEMS.reduce((sum, item) => sum + counts[item.id] * item.points, 0);

  const currentScores = players.map((p) => calcScore(p.counts));
  const totals = players.map(
    (_, i) => history.reduce((s, h) => s + h.scores[i], 0) + currentScores[i]
  );
  const runningTotals = players.map((_, i) =>
    history.reduce((s, h) => s + h.scores[i], 0)
  );

  const adjust = (playerIdx, itemId, delta) => {
    setPlayers((prev) => {
      const next = [...prev];
      const counts = { ...next[playerIdx].counts };
      counts[itemId] = Math.max(0, counts[itemId] + delta);
      next[playerIdx] = { ...next[playerIdx], counts };
      return next;
    });
  };

  const endRound = () => {
    if (currentScores.every((s) => s === 0)) return;
    setHistory((prev) => [
      ...prev,
      { round: prev.length + 1, scores: currentScores },
    ]);
    setPlayers((prev) => prev.map((p) => ({ ...p, counts: blankCounts() })));
  };

  const resetAll = () => {
    if (!confirm("Reset everything? This clears all rounds and scores.")) return;
    setPlayers(PLAYER_COLORS.map((c) => ({ name: c.name, counts: blankCounts() })));
    setHistory([]);
  };

  const clearCurrent = (playerIdx) => {
    setPlayers((prev) => {
      const next = [...prev];
      next[playerIdx] = { ...next[playerIdx], counts: blankCounts() };
      return next;
    });
  };

  const active = players[activePlayer];
  const activeColor = PLAYER_COLORS[activePlayer];

  return (
    <div
      style={{
        minHeight: "100vh",
        background: "#f5f1e8",
        fontFamily: "'EB Garamond', Georgia, serif",
        color: "#1a1a1a",
        padding: "16px",
      }}
    >
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=EB+Garamond:wght@400;500;600;700&family=Bebas+Neue&display=swap');
        * { box-sizing: border-box; }
        button { font-family: inherit; cursor: pointer; border: none; background: none; }
        .tab-btn { transition: all 0.2s ease; }
        .tab-btn:active { transform: scale(0.96); }
        .counter-row { transition: background 0.15s ease; }
        .counter-row:hover { background: rgba(0,0,0,0.02); }
      `}</style>

      {/* Header */}
      <div style={{ textAlign: "center", marginBottom: "20px", borderBottom: "2px solid #1a1a1a", paddingBottom: "14px" }}>
        <div style={{
          fontFamily: "'Bebas Neue', sans-serif",
          fontSize: "36px",
          letterSpacing: "4px",
          lineHeight: 1,
        }}>
          GOULASH
        </div>
        <div style={{ fontSize: "14px", fontStyle: "italic", color: "#666", marginTop: "4px" }}>
          Mahjong scoring · Round {history.length + 1}
        </div>
      </div>

      {/* Running Totals Board */}
      <div style={{
        display: "grid",
        gridTemplateColumns: "repeat(4, 1fr)",
        gap: "6px",
        marginBottom: "18px",
      }}>
        {players.map((p, i) => (
          <div
            key={i}
            onClick={() => setActivePlayer(i)}
            className="tab-btn"
            style={{
              padding: "10px 6px",
              background: activePlayer === i ? PLAYER_COLORS[i].bg : "#fff",
              color: activePlayer === i ? "#fff" : "#1a1a1a",
              border: `2px solid ${PLAYER_COLORS[i].bg}`,
              borderRadius: "2px",
              textAlign: "center",
              cursor: "pointer",
            }}
          >
            {editingName === i ? (
              <input
                autoFocus
                value={p.name}
                onChange={(e) => {
                  const name = e.target.value;
                  setPlayers((prev) => {
                    const next = [...prev];
                    next[i] = { ...next[i], name };
                    return next;
                  });
                }}
                onBlur={() => setEditingName(null)}
                onKeyDown={(e) => e.key === "Enter" && setEditingName(null)}
                style={{
                  width: "100%",
                  background: "transparent",
                  border: "none",
                  color: "inherit",
                  fontFamily: "'Bebas Neue', sans-serif",
                  fontSize: "14px",
                  letterSpacing: "2px",
                  textAlign: "center",
                  outline: "none",
                }}
              />
            ) : (
              <div
                onDoubleClick={(e) => { e.stopPropagation(); setEditingName(i); }}
                style={{
                  fontFamily: "'Bebas Neue', sans-serif",
                  fontSize: "14px",
                  letterSpacing: "2px",
                }}
              >
                {p.name}
              </div>
            )}
            <div style={{
              fontSize: "26px",
              fontWeight: 700,
              lineHeight: 1,
              marginTop: "4px",
            }}>
              {totals[i]}
            </div>
            <div style={{
              fontSize: "11px",
              opacity: 0.75,
              marginTop: "2px",
              fontStyle: "italic",
            }}>
              {runningTotals[i]} + {currentScores[i]}
            </div>
          </div>
        ))}
      </div>

      {/* Active player score card */}
      <div style={{
        background: "#fff",
        border: `3px solid ${activeColor.bg}`,
        borderRadius: "2px",
        overflow: "hidden",
        marginBottom: "16px",
      }}>
        <div style={{
          background: activeColor.bg,
          color: "#fff",
          padding: "10px 14px",
          display: "flex",
          justifyContent: "space-between",
          alignItems: "center",
        }}>
          <div style={{
            fontFamily: "'Bebas Neue', sans-serif",
            fontSize: "18px",
            letterSpacing: "3px",
          }}>
            {active.name} · THIS ROUND
          </div>
          <div style={{ fontSize: "24px", fontWeight: 700 }}>
            {currentScores[activePlayer]}
          </div>
        </div>

        {["pung", "kong", "bonus"].map((group) => (
          <div key={group}>
            <div style={{
              padding: "6px 14px",
              background: "#ebe5d6",
              fontFamily: "'Bebas Neue', sans-serif",
              fontSize: "12px",
              letterSpacing: "2px",
              color: "#666",
              borderTop: "1px solid #ddd",
            }}>
              {group === "pung" ? "PUNGS" : group === "kong" ? "KONGS" : "BONUSES"}
            </div>
            {SCORE_ITEMS.filter((it) => it.group === group).map((item) => {
              const count = active.counts[item.id];
              return (
                <div
                  key={item.id}
                  className="counter-row"
                  style={{
                    display: "grid",
                    gridTemplateColumns: "1fr auto",
                    alignItems: "center",
                    padding: "10px 14px",
                    borderBottom: "1px solid #f0ead9",
                    gap: "10px",
                  }}
                >
                  <div>
                    <div style={{ fontSize: "14px", lineHeight: 1.3 }}>
                      {item.label}
                    </div>
                    <div style={{ fontSize: "12px", color: "#888", fontStyle: "italic" }}>
                      {item.points} pts each
                      {count > 0 && ` · = ${count * item.points}`}
                    </div>
                  </div>
                  <div style={{ display: "flex", alignItems: "center", gap: "8px" }}>
                    <button
                      onClick={() => adjust(activePlayer, item.id, -1)}
                      disabled={count === 0}
                      style={{
                        width: "36px",
                        height: "36px",
                        borderRadius: "50%",
                        background: count === 0 ? "#eee" : activeColor.soft,
                        color: count === 0 ? "#bbb" : activeColor.bg,
                        display: "flex",
                        alignItems: "center",
                        justifyContent: "center",
                      }}
                    >
                      <Minus size={16} />
                    </button>
                    <div style={{
                      minWidth: "24px",
                      textAlign: "center",
                      fontSize: "18px",
                      fontWeight: 700,
                    }}>
                      {count}
                    </div>
                    <button
                      onClick={() => adjust(activePlayer, item.id, 1)}
                      style={{
                        width: "36px",
                        height: "36px",
                        borderRadius: "50%",
                        background: activeColor.bg,
                        color: "#fff",
                        display: "flex",
                        alignItems: "center",
                        justifyContent: "center",
                      }}
                    >
                      <Plus size={16} />
                    </button>
                  </div>
                </div>
              );
            })}
          </div>
        ))}

        {currentScores[activePlayer] > 0 && (
          <button
            onClick={() => clearCurrent(activePlayer)}
            style={{
              width: "100%",
              padding: "10px",
              fontSize: "12px",
              letterSpacing: "2px",
              fontFamily: "'Bebas Neue', sans-serif",
              color: "#666",
              background: "#f5f1e8",
              borderTop: "1px solid #ddd",
            }}
          >
            CLEAR {active.name}'S ROUND
          </button>
        )}
      </div>

      {/* Actions */}
      <div style={{ display: "flex", gap: "8px", marginBottom: "16px" }}>
        <button
          onClick={endRound}
          disabled={currentScores.every((s) => s === 0)}
          style={{
            flex: 2,
            padding: "14px",
            background: currentScores.every((s) => s === 0) ? "#ccc" : "#1a1a1a",
            color: "#fff",
            fontFamily: "'Bebas Neue', sans-serif",
            fontSize: "16px",
            letterSpacing: "3px",
            display: "flex",
            alignItems: "center",
            justifyContent: "center",
            gap: "8px",
            borderRadius: "2px",
          }}
        >
          <Flag size={16} /> END ROUND
        </button>
        <button
          onClick={resetAll}
          style={{
            flex: 1,
            padding: "14px",
            background: "#fff",
            border: "2px solid #1a1a1a",
            fontFamily: "'Bebas Neue', sans-serif",
            fontSize: "14px",
            letterSpacing: "2px",
            display: "flex",
            alignItems: "center",
            justifyContent: "center",
            gap: "6px",
            borderRadius: "2px",
          }}
        >
          <RotateCcw size={14} /> RESET
        </button>
      </div>

      {/* History */}
      {history.length > 0 && (
        <div style={{
          background: "#fff",
          border: "2px solid #1a1a1a",
          borderRadius: "2px",
          overflow: "hidden",
        }}>
          <div style={{
            padding: "10px 14px",
            background: "#1a1a1a",
            color: "#fff",
            fontFamily: "'Bebas Neue', sans-serif",
            fontSize: "14px",
            letterSpacing: "3px",
          }}>
            ROUND HISTORY
          </div>
          <div style={{
            display: "grid",
            gridTemplateColumns: "40px repeat(4, 1fr)",
            background: "#ebe5d6",
            padding: "6px 10px",
            fontSize: "11px",
            fontFamily: "'Bebas Neue', sans-serif",
            letterSpacing: "2px",
            color: "#666",
          }}>
            <div>RD</div>
            {players.map((p, i) => (
              <div key={i} style={{ textAlign: "center", color: PLAYER_COLORS[i].bg }}>
                {p.name}
              </div>
            ))}
          </div>
          {history.map((h) => (
            <div
              key={h.round}
              style={{
                display: "grid",
                gridTemplateColumns: "40px repeat(4, 1fr)",
                padding: "8px 10px",
                borderTop: "1px solid #f0ead9",
                fontSize: "15px",
              }}
            >
              <div style={{ fontWeight: 700, color: "#888" }}>{h.round}</div>
              {h.scores.map((s, i) => (
                <div
                  key={i}
                  style={{
                    textAlign: "center",
                    fontWeight: s === Math.max(...h.scores) && s > 0 ? 700 : 400,
                    color: s === Math.max(...h.scores) && s > 0 ? PLAYER_COLORS[i].bg : "#1a1a1a",
                  }}
                >
                  {s}
                </div>
              ))}
            </div>
          ))}
          <div style={{
            display: "grid",
            gridTemplateColumns: "40px repeat(4, 1fr)",
            padding: "10px",
            borderTop: "2px solid #1a1a1a",
            background: "#f5f1e8",
            fontSize: "18px",
            fontWeight: 700,
          }}>
            <div style={{ fontSize: "11px", fontFamily: "'Bebas Neue', sans-serif", letterSpacing: "2px", alignSelf: "center" }}>
              TOT
            </div>
            {runningTotals.map((t, i) => (
              <div key={i} style={{ textAlign: "center", color: PLAYER_COLORS[i].bg }}>
                {t}
              </div>
            ))}
          </div>
        </div>
      )}

      <div style={{
        textAlign: "center",
        marginTop: "20px",
        fontSize: "11px",
        color: "#999",
        fontStyle: "italic",
      }}>
        Tap a player name to switch · Double-tap to rename
      </div>
    </div>
  );
}
