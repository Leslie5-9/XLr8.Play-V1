{
  "id": "xlr8.consultation.basic",
  "version": "1.0.0",
  "name": "Consultation (Basic)",
  "description": "Capture patient visit: clinician notes, diagnosis, basic vitals.",
  "kind": "ui",
  "inputs": [
    {"name":"patient","type":"patient","required":true},
    {"name":"presentingComplaint","type":"string","required":true,"uiHint":{"widget":"textarea"}}
  ],
  "outputs": [
    {"name":"consultationNote","type":"document"},
    {"name":"problemList","type":"array"}
  ],
  "ui": {
    "templateRef":"templates/consultation/basic/v1",
    "styleHints":{"color":"#0A74DA","layout":"left-sidebar"}
  },
  "logic": {
    "type":"compose",
    "rule": {
      "map":[
        {"from":"inputs.presentingComplaint","to":"outputs.consultationNote.body"}
      ],
      "derive":[
        {"from":"inputs.presentingComplaint","to":"outputs.problemList","module":"nlp:simple-problem-extractor@0.1.0"}
      ]
    }
  },
  "instructions":"Use this brik to create a clinician consultation note. Compose with triage brik to prefill vitals.",
  "tests":[
    {
      "name":"simple complaint",
      "inputFixture":{"patient":{"id":"p1"},"presentingComplaint":"Headache and nausea"},
      "expectedOutput":{"problemList":["Headache"]},
      "runInSandbox":true
    }
  ],
  "compatibility":{"major":1,"notes":"1.x compatible"},
  "created_by":"Leslie5-9",
  "created_at":"2025-11-03T18:00:00Z",
  "tags":["consultation","clinical","example"]
}
