# base_scripts
Scripts shell e sql
Dump_Banco:
pg_dump.exe --host localhost --port 5432 --username postgres --format tar --file C:\Users\patricia_mello\Documents\GI2S_DIM_Caucaia.backup GI2S_DIM

Restore:
pg_restore -i -h localhost -p 5432 -U postgres -d GI2S_OPE -v "C:\Users\patricia_mello\Documents\GI2S_OPE_CAucaia.backup"
