# Protonmail SMTP Server

https://github.com/shenxn/protonmail-bridge-docker/issues/6


1. Get the name of your deployed pod kubectl get pods
2. Run interactively on the pod (setup only) `kubectl exec --stdin --tty protonmail-bridge-deployment-6c79fd7f84-ftwcw -- /bin/bash`
3. Once logged in, execute the init command `bash /protonmail/entrypoint.sh init`
4. You should now see the CLI for protonmail-bridge, authenticate with login
5. (optional) if you're like me and use split address mode, change mode and info are good for printing the details.
6. Copy your SMTP server info (or IMAP, your choice)
7. delete the active pod so a new one gets created (which will properly fire up with your persisted settings)