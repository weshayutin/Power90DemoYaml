# Various test cases for backing up / restoring vms using selectors

## DataVolume precreated: backup all, restore one at a time
#### setup:
```
alias velero='oc -n openshift-adp exec deployment/velero -c velero -it -- ./velero'
cd cirros-test-with-dataVolumePV 
oc create namespace cirros-test-ds-pv
oc create -f cirros-datasource-datavolume.yaml
oc create -f cirros-test-1.yaml -f cirros-test-2.yaml -f cirros-test-3.yaml
velero backup create backup-cirros-test-pv-all --include-namespaces cirros-test-ds-pv --snapshot-move-data=true
oc delete -f cirros-test-1.yaml -f cirros-test-2.yaml -f cirros-test-3.yaml
```
#### to restore:
```
alias velero='oc -n openshift-adp exec deployment/velero -c velero -it -- ./velero'
velero restore create cirros-test-pv-1  --from-backup backup-cirros-test-pv-all --selector app=cirros-test-pv-1
velero restore create cirros-test-pv-2  --from-backup backup-cirros-test-pv-all --selector app=cirros-test-pv-2
velero restore create cirros-test-pv-3  --from-backup backup-cirros-test-pv-all --selector app=cirros-test-pv-3
```
