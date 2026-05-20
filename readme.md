# Rapport d'Examen DevOps - CI/CD & DevSecOps

**Étudiante :** Sarah  
**Date :** Mai 2026  
**Repository :** https://github.com/Sarahly2611/exam052026

---

## 1. Problèmes rencontrés & Analyses

 plusieurs blocages majeurs empêchaient l'exécution correcte des chaînes CI/CD :

* **Sécurité des Forks GitHub :** Les workflows GitHub Actions étaient désactivés par défaut sur le dépôt dupliqué (Fork), bloquant tout déclenchement automatique.
* **Authentification Docker Hub (2FA) :** L'utilisation des secrets classiques pour le `docker login` échouait systématiquement en raison des restrictions d'authentification à double facteur (2FA) sur le compte utilisateur.
* **Configuration des Runners :** Le pipeline ciblait un runner local nommé `runner-lab` (spécifique à l'environnement de l'instructeur) qui était injoignable depuis , laissant  en attente infinie.
* **Cibles Trivy incorrectes :** Le scanner Trivy pointait vers une image Docker distante inexistante au lieu de scanner l'image locale fraîchement construite.

---

## 2. Corrections appliquées

Pour restaurer un workflow  fonctionnel, les actions suivantes ont été menées :

1. **Activation des Actions :** Autorisation explicite de l'exécution des workflows sur le fork GitHub.
2. **Optimisation des Environnements d'Exécution :** Basculement de l'ensemble des jobs (`ci`, `scan`, `deploy`) sur des runners publics et gratuits de GitHub (`runs-on: ubuntu-latest`).
3. **Refactorisation du Pipeline de la branche `main` :** Nettoyage des étapes de push distantes bloquantes et validation locale du cycle Build & Test.
4. **Correction et alignement Trivy (branche `trivy`) :** Liaison du scanner Trivy (`aquasecurity/trivy-action`) sur l'image locale construite en direct (`ci-html:1.0.1`) avec une sévérité fixée sur `CRITICAL`.
5. **Résolution des conflits Git :** Recalibrage des index locaux et poussées forcées via les bonnes références de branches (`git push origin HEAD:trivy`).

---

## 3. Statut des Pipelines
* **Branche `main` :** Validée .
* **Branche `trivy` :** Validée 
avec génération du rapport de vulnérabilités.
