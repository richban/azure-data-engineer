# Replicated

Great fit for...: ✅ Small-dimension tables in a star schema with less than 2GB of storage after compression (~5x compression).
Watch out if...: ⛔ Many write transactions are on the table (insert/update/delete).⛔ You change DWU provisioning frequently.⛔ You use only 2-3 columns, but your table has many columns.⛔ You index a replicated table.