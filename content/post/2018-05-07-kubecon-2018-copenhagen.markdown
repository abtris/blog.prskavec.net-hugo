---
title: "Kubecon 2018 - Copenhagen"
date: 2018-05-07T16:43:37+02:00
tags:
  - k8s
---

Letos jsem poprvé byl na Kubeconu, který se konal v týdnu od 30.4. do 4.5 v Kodaňi. Kodaň je pěkné město a hotel Bella Sky je nádherná stavba.

![](/images/kubecon18/BellaSkyHotel.jpg)

Účastníků bylo něco přes 6000 a příští rok v Barceloně se bude atakovat možná 10000.
Celá konference měla 4 dny. První den byli workshopy a lighting talky. Potom začal oficiální program, každé dopoledne keynoty v bloku do oběda a po něm sessions do večera. Druhý den konference byla velká party v centru Kodaně v zábavním parku Tivolli.

Množství paralelních přednášek bylo šílené, ale jejich kvalita velice různorodá.


## Hlavní trendy

Hlavní projekty jsou dnes pěkně v přehledu CNCF. Kubernetes (k8s) je první projekt, který dosáhl úrovně [Graduated](https://www.cncf.io/projects/).

- [landscape.cncf.io](https://landscape.cncf.io)

Největší hype je kolem service meshu. Který používat? Mám to vůbec použít? [Istio](https://istio.io/) nebo [Linkerd](https://linkerd.io/) nebo raději [Heptio Contour](https://github.com/heptio/contour)?

Spousta otázek na které není jednoduché odpovědět a můžete si naběhnout. Je potřeba být opatrný a postupovat krok za krokem, projekt po projektu.

- Service meshe (Istio, Linkerd, Contour)
- Prometheus je monitoring standard pro k8s - [řeší se longterm storage](https://github.com/improbable-eng/thanos), federace a gitops nad [dashboardy, alerty](https://www.youtube.com/watch?v=b7-DtFfsL6E)
- K8s operátory a další rozšíření  - mrkněte na [awesome-operators](https://github.com/operator-framework/awesome-operators)
- Hybrid cloud solutions - několik řešení jako Cloud Foundery, ale hodně nových startupů, které do toho jdou taky
- Security services - spousta projetků (TUF, Spiffe, Spire, Open Policy Agent) a i firem (Aquasecurity, Twistlock, Blackduck)
- Serverless - hlavně [cloud events specifikace](https://github.com/cloudevents/spec) je zajímavá pro rozvoj serverless
- ML - KubeFlow, k8s and Tensorflow - v keynote byl zmíněn hlavně release Kubeflow 0.1, ale stále asi hodně práce něž projekt bude pro každého

Další drobné věci, které mě zaujali [Go registry od JFrog](https://jfrog.com/blog/goproxy-artifactory-go-registries/), [Operator Framework](https://coreos.com/blog/introducing-operator-framework) of CoreOS, Redhat. A nový bezpečný sandbox runtime [gVisor](https://cloudplatform.googleblog.com/2018/05/Open-sourcing-gVisor-a-sandboxed-container-runtime.html) od Googlu.

### Ostatní zajímavé příspěvky o konferenci:

- [Copenhagen: KubeCon and CloudNativeCon 2018 Takeaways](http://aniszczyk.org/2018/05/06/copenhagen-kubecon-and-cloudnativecon-2018-takeaways/)
- [Everything announced at KubeCon + CloudNativeCon Europe 2018](https://venturebeat.com/2018/05/05/everything-announced-at-kubecon-cloudnativecon-europe-2018/)


### Videa z konference:

- [Kubecon18 playlist](https://www.youtube.com/watch?v=OUYTNywPk-s&list=PLj6h78yzYM2N8GdbjmhVU65KYm_68qBmo) - je jich 308 v době psaní článku.


## Závěr

Většina cloudových poskytovatelů má hostovaný Kubernetes (k8s) buď již v ostrém provozu nebo ho letos chystají. Ekosystém kolem k8s se velice rozšířil a nebude to jednoduché pojmout.
Pokud vás to zaujalo nebo jste Go lang programátoři, přijďte si o tom popovídat na [Golang meetup v Praze](https://www.meetup.com/Prague-Golang-Meetup/events/250136683/).
