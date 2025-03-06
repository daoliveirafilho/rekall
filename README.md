# rekall
rekall
create database testdb;
create user teste with encrypted password 'teste123';
grant all privileges on database testdb to teste;
grant all privileges on all tables in schema public to teste;
grant all privileges on all sequences in schema public to teste;
grant all privileges on all functions in schema public to teste;
\connect testdb
create table "public"."grupo" ("id" serial PRIMARY KEY, "grupo" char(255) NOT NULL, "date" timestamp NOT NULL DEFAULT NOW()) without oids;
create table "public"."host" ("id" uuid PRIMARY KEY, "groupId" integer NOT NULL, "v4" inet NOT NULL, "v6" inet NOT NULL, "lo" char(255) NOT NULL, "date" timestamp NOT NULL DEFAULT NOW()) without oids;
create table "public"."pass" ("id" uuid, "groupId" integer NOT NULL, "pa" char(255) NOT NULL, "date" timestamp NOT NULL DEFAULT NOW(), PRIMARY KEY ("id")) without oids;
INSERT INTO "public"."grupo" VALUES (nextval('grupo_id_seq'::regclass),'GRUPO',now());
SELECT COUNT(*) AS total FROM (SELECT * FROM "public"."grupo" ORDER BY "id" DESC) AS sub;
ALTER TABLE "public"."host" ADD CONSTRAINT "grupo2host" FOREIGN KEY ("groupId") REFERENCES "public"."grupo"("id")  ON UPDATE RESTRICT ON DELETE RESTRICT;
ALTER TABLE "public"."pass" ADD CONSTRAINT "grupo2pass" FOREIGN KEY ("groupId") REFERENCES "public"."grupo"("id")  ON UPDATE RESTRICT ON DELETE RESTRICT;
revoke all privileges on all functions in schema public from testuser;
revoke all privileges on all sequences in schema public from testuser;
revoke all privileges on all tables in schema public from testuser;
revoke all privileges on database testdb from testuser;
drop user testuser;
\q
\connect testdb
\connect testdb
ALTER TABLE "public"."host" RENAME COLUMN "groupId" TO "grupo";
ALTER TABLE "public"."pass" RENAME COLUMN "groupId" TO "grupo";
\q
