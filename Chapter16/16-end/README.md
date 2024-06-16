# CHANGELOG

* Spring 3.3
* Java 21
* SBOM

```bash
for i in `echo "catalog-service config-service dispatcher-service edge-service order-service quote-function quote-service"`; do
cd $i && gradle rewriteRun && cd -
done
```
