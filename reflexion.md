# Reflexion

## Warum ist ein Deployment in Kubernetes nicht einfach nur eine etwas andere Version von `docker run --restart=always`?
Ein Deployment verwaltet nicht nur einzelne Container, sondern orchestriert Replikate, Updates und Rollbacks deklarativ. Es denkt langfristig, nicht nur in Neustarts.

## Was tut das Deployment, wenn ein Pod plötzlich verschwindet – und warum ist das nicht einfach nur Magie?
Es erkennt den Verlust und ersetzt den Pod automatisch, basierend auf der gewünschten Replikazahl. Das ist kontrollierte Selbstheilung, keine Magie.

## Was konntest du beim Rolling Update mit `kubectl get pods -w` beobachten – und wie wird sichergestellt, dass es keinen kompletten Ausfall gibt?
Alte Pods wurden nacheinander beendet, während neue starteten. Durch das Rolling-Update-Verhalten bleibt mindestens ein Teil der App immer erreichbar.

## Wie sorgt der Kubernetes-Service dafür, dass dein Browser-Ping (über NodePort) den richtigen Pod trifft – selbst bei einem Update?
Der Service leitet Traffic an Pods mit dem passenden Label (`app=nginx-example`) weiter. Solange neue Pods dasselbe Label tragen, funktioniert die Weiterleitung automatisch.

## In der Deployment-YAML: Welche Angaben betreffen die Pod-Vorlage, und welche das Verhalten des Deployments?
Die Pod-Vorlage steht unter `spec.template` (Container, Image, Ports). Das Verhalten (z. B. `replicas`, `strategy`, `selector`) wird im oberen Bereich von `spec` definiert.

## Was passiert mit den Pods, wenn du das Deployment löschst – und warum ist das logisch?
Die Pods werden mitgelöscht, weil sie Teil des Deployments sind. Ohne Controller gibt es keinen Grund, die zugehörigen Pods zu behalten – klare Trennung von Steuerung und Ausführung.
