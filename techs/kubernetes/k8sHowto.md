# Change to nodeport

Get it into the service of the POD
kubectl edit svc sdnc -n jparghi


change to Nodeport..

spec:
  clusterIP: 10.43.158.180
  externalTrafficPolicy: Cluster
  ports:
  - name: sdnc
    nodePort: 31796
    port: 8282
    protocol: TCP
    targetPort: 8181
  selector:
    app: sdnc
    release: jparghi-sdnc
  sessionAffinity: None
  type: NodePort
