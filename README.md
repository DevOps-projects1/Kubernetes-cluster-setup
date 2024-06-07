## Kubernetes-cluster-setup

###Step 1 Creating Certificate for Alice:
      cd /root/certificates

      openssl genrsa -out alice.key 2048
      openssl req -new -key alice.key -subj "/CN=alice/O=developers" -out alice.csr
      openssl x509 -req -in alice.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out alice.crt -days 1000

###Step 2 Set ClientCA flag in API Server:
    nano /etc/systemd/system/kube-apiserver.service

    --client-ca-file /root/certificates/ca.crt

    systemctl daemon-reload
   systemctl restart kube-apiserver


  ##Step 3 Verification:
  kubectl get secret --server=https://127.0.0.1:6443 --client-certificate /root/certificates/alice.crt --certificate-authority /root/certificates/ca.crt --client-key /root/certificates/alice.key
