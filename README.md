export function simpleProblemExtractor(text:string) {
  // extremely small deterministic NLP: split by comma/and and return capitalized first tokens
  if (!text || typeof text !== "string") return [];
  const tokens = text.split(/\band\b|,|;/i).map(s=>s.trim()).filter(Boolean);
  // pick first noun-ish token (very naive)
  const results = tokens.map(t => {
    const first = t.split(" ").slice(0,3).join(" ");
    return first.charAt(0).toUpperCase() + first.slice(1);
  });
  return results;
}
