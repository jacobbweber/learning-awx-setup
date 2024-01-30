# Setup Minikube



> Needed to enable the ingress controller to set the awx-operator resource ingress_type: ingress
> I noticed I need to start this service anytime the minikube vm is powered off or stopped. probably a way to make persist.

```powershell
minikube.exe addons enable ingress
```



## Validate After



> This is the powershell snippet I just copy and paste after I startup my minikube instance, im lazy.


