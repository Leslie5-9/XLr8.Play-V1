CREATE TABLE IF NOT EXISTS briks (
  id BIGSERIAL PRIMARY KEY,
  brik_id text NOT NULL,
  version text NOT NULL,
  content jsonb NOT NULL,
  created_by text,
  created_at timestamptz default now(),
  tags text[],
  checksum text,
  UNIQUE (brik_id, version)
);

CREATE INDEX ON briks USING gin(content jsonb_path_ops);
CREATE INDEX ON briks(tags);
