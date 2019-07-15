#  _ssh-pem-keys-data.md

For EC2-instances:


ssh -i ../path-to-dir/<correct-pem-file>.pem  ec2-user@<ip-addr>

      Chmod the key to 600 or 400 - both work. 
      I've seen both in the docs.


      @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
      @         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
      @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
      Permissions 0644 for '../AWS_private/rancher-course.pem' are too open.
      It is required that your private key files are NOT accessible by others.
      This private key will be ignored.
      Load key "../AWS_private/rancher-course.pem": bad permissions
      Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
      