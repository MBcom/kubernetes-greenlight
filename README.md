# kubernetes Greenlight deployment

In this repository you can find scripts for deploying https://github.com/bigbluebutton/greenlight to a kubernetes cluster.

## Installation
1. create a new namespace in your kubernetes cluster
2. clone this repository to a computer with kubectl installed
3. run the following inside repository folder
```bash
sed -i 's/greenlight-ns/<your namespace name>/g' *.yaml
sed -i 's/greenlight-image/<greenlight image name>/g'
kubectl -n <your namespace name> apply -f *.yaml
```  
  
If you use the greenlight of https://github.com/mbcom/greenlight you must also create the `bbb-nas.yaml.example` and the `bbb-nas-claim.yaml.example`. In `bbb-nas.yaml.example` specify your NAS serveraddress where your `/var/bigbluebutton/published/` files are placed. In `bbb-nas-claim.yaml.example` specify the right namespace as above.
Too you need to uncomment the commented lines in `greenlight-deployment.yaml` and apply it again.
As docker image you can use `mbcom/greenlight:v2`.

## Another Links
* you can find an GDPR compliant Greenlight at https://github.com/mbcom/greenlight , this Greenlight fork contains
   * enforced LDAP authentication
   * room owners are able to change their names before entering a meeting
   * all others are enforced to user their domain names as in Greenlight
   * button for MP4 video, e.g. created with https://github.com/MBcom/kubernetes-bbb-video-download
   * button for getting shared notes, e.g. copied with https://github.com/MBcom/kubernetes-bbb-video-download/blob/master/snippets/post_publish_copy_notes.rb and copy_notes.sh
