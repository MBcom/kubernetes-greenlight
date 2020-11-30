# kubernetes Greenlight deployment

In this repository you can find scripts for deploying https://github.com/bigbluebutton/greenlight or https://github.com/mbcom/greenlight to a kubernetes cluster.

## Installation
1. create a new namespace in your kubernetes cluster
2. clone this repository to a computer with kubectl and helm installed
3. deploy a postgres db to your cluster with the following command
```bash
# if you have not already done before
# helm repo add bitnami https://charts.bitnami.com/bitnami
sed -i 's/a_password_that_i_should_change/save_password/g' greenlight-postgres.values
helm install -n <your namespace name> greenlight -f greenlight-postgres.values bitnami/postgresql
```
4. now you can/ must configure your greenlight, therefore a secret is used, make sure you all values are base64 encoded
   * set the values and save the file as `secret.yaml`, the options are the same as in the greenlight `.env` file
```yaml
apiVersion: v1
data:
  ALLOW_GREENLIGHT_ACCOUNTS: ZmFsc2U=
  ALLOW_MAIL_NOTIFICATIONS: dHJ1ZQ==
  BIGBLUEBUTTON_ENDPOINT: 
  BIGBLUEBUTTON_SECRET: 
  DB_ADAPTER: cG9zdGdyZXNxbA==
  DB_HOST: Z3JlZW5saWdodC1wb3N0Z3Jlc3Fs
  DB_NAME: Z3JlZW5saWdodA==
  DB_PASSWORD: 
  DB_USERNAME: Z3JlZW5saWdodA==
  DEFAULT_REGISTRATION: YXBwcm92YWw=
  LDAP_BASE: 
  LDAP_BIND_DN: 
  LDAP_METHOD: 
  LDAP_PASSWORD: 
  LDAP_PORT: 
  LDAP_ROLE_FIELD: 
  LDAP_SERVER: 
  LDAP_UID: 
  RAILS_LOG_TO_STDOUT: dHJ1ZQ==
  RELATIVE_URL_ROOT: Lw==
  ROOM_FEATURES: bXV0ZS1vbi1qb2luLHJlcXVpcmUtbW9kZXJhdG9yLWFwcHJvdmFsLGFueW9uZS1jYW4tc3RhcnQsYWxsLWpvaW4tbW9kZXJhdG9y
  SECRET_KEY_BASE: 
  SMTP_AUTH: 
  SMTP_DOMAIN: 
  SMTP_PORT: 
  SMTP_SENDER: 
  SMTP_SERVER: 
  SMTP_STARTTLS_AUTO: 
kind: Secret
metadata:
  name: greenlight-secret
  namespace: greenlight-ns
type: Opaque
```
5. run the following inside repository folder
```bash
sed -i 's/greenlight-ns/<your namespace name>/g' *.yaml
sed -i 's/greenlight-image/<greenlight image name>/g' greenlight-deployment.yaml
sed -i 's/greenlight-db-password/<greenlight db password>/g' greenlight-deployment.yaml
kubectl -n <your namespace name> apply -f *.yaml
```  
6. to use the provided ingress ressource edit the `greenlight-ingress.yaml.example` and fill in the required fields manually after that you can deploy it too  
  
If you use the greenlight of https://github.com/mbcom/greenlight you must also create the `bbb-nas.yaml.example` and the `bbb-nas-claim.yaml.example`. In `bbb-nas.yaml.example` specify your NAS serveraddress where your `/var/bigbluebutton/published/` files are placed. In `bbb-nas-claim.yaml.example` specify the right namespace as above.
Too you need to uncomment the commented lines in `greenlight-deployment.yaml` and apply it again.
As docker image you can use `mbcom/greenlight:v2.7.9`.

## Another Links
* you can find an GDPR compliant Greenlight at https://github.com/mbcom/greenlight , this Greenlight fork contains
   * enforced LDAP authentication
   * room owners are able to change their names before entering a meeting
   * all others are enforced to user their domain names as in Greenlight
   * button for MP4 video, e.g. created with https://github.com/MBcom/kubernetes-bbb-video-download
   * button for getting shared notes, e.g. copied with https://github.com/MBcom/kubernetes-bbb-video-download/blob/master/snippets/post_publish_copy_notes.rb and copy_notes.sh
