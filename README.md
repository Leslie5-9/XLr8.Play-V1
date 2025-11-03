import { simpleProblemExtractor } from "./registry/simpleProblemExtractor";

export async function runBrikInSandbox(brik:any, inputs:any) {
  // Minimal validation: check required inputs exist
  const missing = (brik.inputs || []).filter((i:any)=> i.required && inputs[i.name]===undefined).map((i:any)=>i.name);
  if (missing.length) throw new Error("Missing required inputs: "+missing.join(", "));
  const outputs:any = {};
  const logs:any[] = [];

  // handle simple map rules
  const rule = brik.logic?.rule;
  if (rule?.map) {
    for (const m of rule.map) {
      // from: "inputs.presentingComplaint" to: "outputs.consultationNote.body"
      const v = getPathValue({inputs}, m.from);
      setPathValue(outputs, m.to.replace(/^outputs\./, ""), v);
      logs.push({type:"map", from:m.from, to:m.to, value:v});
    }
  }

  // handle derive rules (only module-based)
  if (rule?.derive) {
    for (const d of rule.derive) {
      if (!d.module) { logs.push({warning:"derive rule missing module"}); continue; }
      if (d.module.startsWith("nlp:simple-problem-extractor")) {
        const src = getPathValue({inputs}, d.from);
        const derived = simpleProblemExtractor(src);
        setPathValue(outputs, d.to.replace(/^outputs\./, ""), derived);
        logs.push({type:"derive", module:d.module, from:d.from, to:d.to, value:derived});
      } else {
        logs.push({warning:"module not supported in sandbox: "+d.module});
      }
    }
  }

  return { outputs, logs };
}

function getPathValue(root:any, path:string) {
  const parts = path.split(".");
  let cur:any = root;
  for (const p of parts) {
    if (cur==null) return undefined;
    cur = cur[p];
  }
  return cur;
}

function setPathValue(root:any, path:string, value:any) {
  const parts = path.split(".");
  let cur = root;
  for (let i=0;i<parts.length;i++) {
    const p = parts[i];
    if (i===parts.length-1) { cur[p]=value; return; }
    if (!cur[p]) cur[p]={};
    cur = cur[p];
  }
}
