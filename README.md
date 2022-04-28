# ocp4

<h4> Article How to delete all kubernetes.io/events in etcd </h4>

https://access.redhat.com/solutions/6171352

<h4> How to defrag etcd to decrease DB size in OpenShift 4 </h4>

https://access.redhat.com/solutions/5564771

<h4> Ignition fails adding new nodes to UPI cluster after upgrading to OCP 4.6+ </h4>

https://access.redhat.com/solutions/5514051

<pre>$ export CLUSTERDOMAINAPI=api-int.clusterDomain:22623</pre>
<pre style="overflow-y: hidden"><code>
#!/bin/sh
mkdir -p /tmp/scaleup; cd /tmp/scaleup 
cat <<EOF >>worker.ign
{"ignition":{"config":{"merge":[{"source":"https://CLUSTERDOMAINAPI/config/worker"}]},"security":{"tls":{"certificateAuthorities":[{"source":"data:text/plain;charset=utf-8;base64,CERTinBASE64"}]}},"version":"3.1.0"}}
EOF
sed -i "s%CLUSTERDOMAINAPI%$CLUSTERDOMAINAPI%" worker.ign 
echo "q" | openssl s_client -connect $CLUSTERDOMAINAPI -showcerts | awk '/-----BEGIN CERTIFICATE-----/,/-----END CERTIFICATE-----/' | base64 --wrap=0 | tee ./api-int.base64
sed --regexp-extended --in-place='' "s%CERTinBASE64%$(cat ./api-int.base64)%" worker.ign
</code></pre>


