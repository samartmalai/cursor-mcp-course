# Use Supabase with Next.js
https://supabase.com/docs/guides/getting-started/quickstarts/nextjs


Learn how to create a Supabase project, add some sample data, and query from a Next.js app.

## 1 Create a Supabase project
Go to database.new and create a new Supabase project.

When your project is up and running, go to the Table Editor, create a new table and insert some data.

Alternatively, you can run the following snippet in your project's SQL Editor. This will create a instruments table with some sample data.

-- Create the table
create table instruments (
  id bigint primary key generated always as identity,
  name text not null
);
-- Insert some sample data into the table
insert into instruments (name)
values
  ('violin'),
  ('viola'),
  ('cello');
alter table instruments enable row level security;
Make the data in your table publicly readable by adding an RLS policy:

create policy "public can read instruments"
on public.instruments
for select to anon
using (true);


## 2 Create a Next.js app
Use the create-next-app command and the with-supabase template, to create a Next.js app pre-configured with:

Cookie-based Auth
TypeScript
Tailwind CSS
npx create-next-app -e with-supabase
## 3 Declare Supabase Environment Variables
Rename .env.example to .env.local and populate with your Supabase connection variables:

Project URL
shawnesquivel's Org / Lexi
PROJECT PAUSED

Anon key
shawnesquivel's Org / Lexi
PROJECT PAUSED


.env.local
NEXT_PUBLIC_SUPABASE_URL=<SUBSTITUTE_SUPABASE_URL>
NEXT_PUBLIC_SUPABASE_ANON_KEY=<SUBSTITUTE_SUPABASE_ANON_KEY>

## 4 Query Supabase data from Next.js
Create a new file at app/instruments/page.tsx and populate with the following.

This will select all the rows from the instruments table in Supabase and render them on the page.


app/instruments/page.tsx

utils/supabase/server.ts
import { createClient } from '@/utils/supabase/server';
export default async function Instruments() {
  const supabase = await createClient();
  const { data: instruments } = await supabase.from("instruments").select();
  return <pre>{JSON.stringify(instruments, null, 2)}</pre>
}

## 5 Start the app
Run the development server, go to http://localhost:3000/instruments in a browser and you should see the list of instruments.