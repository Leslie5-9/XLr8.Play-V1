import express from "express";
import bodyParser from "body-parser";
import Ajv from "ajv";
import fs from "fs";
import path from "path";
import { runBrikInSandbox } from "./sandboxRunner";

const app = express();
app.use(bodyParser.json());

const schema = JSON.parse(fs.readFileSync(path.join(__dirname,"..","schema","brik.schema.json"), "utf8"));
const ajv = new Ajv({allErrors:true, strict:false});
const validate = ajv.compile(schema);

app.post("/api/v1/briks/validate", (req, res) => {
  const ok = validate(req.body);
  if (ok) return res.json({valid:true});
  return res.status(400).json({valid:false, errors: validate.errors});
});

app.post("/api/v1/briks/run", async (req, res) => {
  // body: { brik: {...}, inputs: {...} }
  try {
    const { brik, inputs } = req.body;
    const result = await runBrikInSandbox(brik, inputs);
    res.json(result);
  } catch (err:any) {
    res.status(500).json({error: err.message});
  }
});

const port = process.env.PORT || 3001;
app.listen(port, () => console.log(`Brik service listening on ${port}`));
