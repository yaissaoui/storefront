# Universal Editor - Configuration Guide

## Résumé des configs pour faire fonctionner le Universal Editor avec EDS + da.live

---

## 1. config.json

Configuration du backend Commerce (ACCS/SaaS) :

```json
{
  "public": {
    "default": {
      "commerce-core-endpoint": "https://na1-sandbox.api.commerce.adobe.com/QyHA1HUbXt5J6hX5jPNWXF/graphql",
      "commerce-endpoint": "https://na1-sandbox.api.commerce.adobe.com/QyHA1HUbXt5J6hX5jPNWXF/graphql",
      "commerce-assets-enabled": false,
      "commerce-b2b-enabled": true,
      "commerce-companies-enabled": true,
      "headers": {
        "all": {
          "Store": "default"
        },
        "cs": {
          "Magento-Store-Code": "main_website_store",
          "Magento-Store-View-Code": "default",
          "Magento-Website-Code": "base"
        }
      },
      "analytics": {
        "aep-ims-org-id": null,
        "aep-datastream-id": null,
        "base-currency-code": "EUR",
        "environment": "Testing",
        "environment-id": "QyHA1HUbXt5J6hX5jPNWXF",
        "store-code": "main_website_store",
        "store-id": "main_website_store",
        "store-name": "Default Store View",
        "store-url": "https://na1-sandbox.api.commerce.adobe.com/QyHA1HUbXt5J6hX5jPNWXF/",
        "store-view-code": "default",
        "store-view-id": "default",
        "store-view-name": "Default Store View",
        "website-code": "base",
        "website-id": "base",
        "website-name": "Main Website"
      },
      "plugins": {
        "picker": {
          "rootCategory": "2"
        }
      }
    }
  }
}
```

---

## 2. fstab.yaml

Mount points pour da.live :

```yaml
mountpoints:
  /:
    url: https://content.da.live/yaissaoui/storefront/
    type: markup

folders:
  /products/: /products/default
```

**Important :** Le mapping `/products/` → `/products/default` signifie que les pages produits dans da.live doivent être créées sous `/products/default/`.

---

## 3. default-site.json

Configuration complète du site :

```json
{
  "version": 1,
  "code": {
    "owner": "yaissaoui",
    "repo": "storefront",
    "source": {
      "type": "github",
      "url": "https://github.com/yaissaoui/storefront"
    }
  },
  "content": {
    "source": {
      "url": "https://content.da.live/yaissaoui/storefront/",
      "type": "onedrive"
    }
  },
  "folders": {
    "/products/": "/products/default"
  },
  "cdn": {
    "live": {
      "host": "main--storefront--yaissaoui.aem.live"
    },
    "preview": {
      "host": "main--storefront--yaissaoui.aem.page"
    }
  },
  "headers": {},
  "public": {
    "default": {
      "commerce-core-endpoint": "https://na1-sandbox.api.commerce.adobe.com/QyHA1HUbXt5J6hX5jPNWXF/graphql",
      "commerce-endpoint": "https://na1-sandbox.api.commerce.adobe.com/QyHA1HUbXt5J6hX5jPNWXF/graphql",
      "commerce-assets-enabled": false,
      "commerce-b2b-enabled": true,
      "commerce-companies-enabled": true,
      "headers": {
        "all": {
          "Store": "default"
        },
        "cs": {
          "Magento-Store-Code": "main_website_store",
          "Magento-Store-View-Code": "default",
          "Magento-Website-Code": "base"
        }
      },
      "analytics": {
        "aep-ims-org-id": null,
        "aep-datastream-id": null,
        "base-currency-code": "EUR",
        "environment": "Testing",
        "environment-id": "QyHA1HUbXt5J6hX5jPNWXF",
        "store-code": "main_website_store",
        "store-id": "main_website_store",
        "store-name": "Default Store View",
        "store-url": "https://na1-sandbox.api.commerce.adobe.com/QyHA1HUbXt5J6hX5jPNWXF/",
        "store-view-code": "default",
        "store-view-id": "default",
        "store-view-name": "Default Store View",
        "website-code": "base",
        "website-id": "base",
        "website-name": "Main Website"
      },
      "plugins": {
        "picker": {
          "rootCategory": "2"
        }
      }
    }
  },
  "robots": {
    "txt": "User-agent: *\nAllow: /\nDisallow: /drafts/\nDisallow: /enrichment/\nDisallow: /tools/\nDisallow: /plugins/experimentation/\n\nSitemap: https://main--storefront--yaissaoui.aem.live/sitemap-index.xml"
  },
  "access": {
    "admin": {
      "role": {
        "config_admin": [
          "yaissaoui"
        ]
      },
      "requireAuth": "auto"
    }
  }
}
```

---

## 4. head.html

Instrumentation Universal Editor dans le `<head>` :

```html
<meta
  http-equiv="Content-Security-Policy"
  content="script-src 'nonce-aem' 'strict-dynamic' 'unsafe-inline' http: https:; base-uri 'self'; object-src 'none';"
  move-to-http-header="true"
>
<meta name="viewport" content="width=device-width, initial-scale=1.0" />

<!-- Universal Editor - Meta tags de connexion -->
<meta name="urn:adobe:aue:system" content="aem">
<meta name="urn:adobe:aue:config:service" content="https://universal-editor-service.adobe.io">

<!-- Universal Editor - CORS library (AVEC nonce CSP !) -->
<script nonce="aem" src="https://universal-editor-service.adobe.io/cors.js" async></script>

<!-- Le reste du head existant... -->
<script nonce="aem" type="speculationrules">
    {
        "prerender": [{
            "where": {
                "href_matches": "/*"
            },
            "eagerness": "moderate"
        }]
    }
</script>
<script nonce="aem" type="importmap">
    {
        "imports": {
            "@dropins/storefront-account/": "/scripts/__dropins__/storefront-account/",
            "@dropins/storefront-auth/": "/scripts/__dropins__/storefront-auth/",
            "@dropins/storefront-cart/": "/scripts/__dropins__/storefront-cart/",
            "@dropins/storefront-checkout/": "/scripts/__dropins__/storefront-checkout/",
            "@dropins/storefront-order/": "/scripts/__dropins__/storefront-order/",
            "@dropins/storefront-payment-services/": "/scripts/__dropins__/storefront-payment-services/",
            "@dropins/storefront-pdp/": "/scripts/__dropins__/storefront-pdp/",
            "@dropins/storefront-recommendations/": "/scripts/__dropins__/storefront-recommendations/",
            "@dropins/storefront-wishlist/": "/scripts/__dropins__/storefront-wishlist/",
            "@dropins/storefront-personalization/": "/scripts/__dropins__/storefront-personalization/",
            "@dropins/storefront-product-discovery/": "/scripts/__dropins__/storefront-product-discovery/",
            "@dropins/tools/": "/scripts/__dropins__/tools/"
        }
    }
</script>

<link rel="stylesheet" href="/styles/styles.css" />

<script nonce="aem" type="module">
    if (!(HTMLScriptElement.supports?.('importmap'))) {
        import('/scripts/__dropins__/tools/shims/importmap.js');
    }
</script>

<script nonce="aem" src="/scripts/aem.js" type="module"></script>
<script nonce="aem" src="/scripts/scripts.js" type="module"></script>
<script nonce="aem" src="/scripts/commerce.js" type="module"></script>
```

---

## 5. Configuration da.live (editor.path)

**URL :** `https://da.live/config#/yaissaoui/`

| Clé | Valeur |
|---|---|
| `editor.path` | `/yaissaoui/storefront/=https://experience.adobe.com/#/@sqlisaemeaptrsd/aem/editor/canvas/main--storefront--yaissaoui.ue.da.live` |

**Notes :**
- Remplace `@sqlisaemeaptrsd` par ton vrai DX Handle (trouvable sur experience.adobe.com)
- Le chemin `/yaissaoui/storefront/` doit correspondre au chemin réel du projet dans da.live
- La fin de l'URL `main--storefront--yaissaoui.ue.da.live` doit correspondre au sous-domaine EDS

---

## 6. scripts/aem.js - Ajout des attributs UE

Le `decorateBlock` dans `scripts/aem.js` doit ajouter les attributs `data-aue-*` pour que le Universal Editor détecte les blocks :

```javascript
function decorateBlock(block) {
  const shortBlockName = block.classList[0];
  if (shortBlockName) {
    block.classList.add('block');
    block.dataset.blockName = shortBlockName;
    block.dataset.blockStatus = 'initialized';
    block.dataset.aueComponent = shortBlockName;
    block.dataset.aueResource = `${window.location.pathname}?aue-sel=/${window.location.pathname.replace(/^\//, '').replace(/\/$/, '')}`;
    wrapTextNodes(block);
    const blockWrapper = block.parentElement;
    blockWrapper.classList.add(`${shortBlockName}-wrapper`);
    const section = block.closest('.section');
    if (section) section.classList.add(`${shortBlockName}-container`);
  }
}
```

**Attributs ajoutés :**
- `data-aue-component` — Le type du block (ex: "hero", "columns", "cards")
- `data-aue-resource` — Le chemin de la ressource pour le UE

---

## Points critiques

### CSP (Content Security-Policy)
Le script `cors.js` **DOIT** avoir le `nonce="aem"` pour passer le CSP. Sans ça, le script est bloqué et le Universal Editor ne peut pas se connecter.

```html
<!-- CORRECT -->
<script nonce="aem" src="https://universal-editor-service.adobe.io/cors.js" async></script>

<!-- INCORRECT - sera bloqué par le CSP -->
<script src="https://universal-editor-service.adobe.io/cors.js" async></script>
```

### Fichiers UE instrumentation
Les fichiers suivants doivent exister dans le repo :
- `component-definition.json` (généré automatiquement)
- `component-models.json` (généré automatiquement)
- `component-filters.json` (généré automatiquement)
- `blocks/{block-name}/_{block-name}.json` (instrumentation par bloc)

### Format des block JSON
Chaque block éditable doit avoir un fichier `_{block-name}.json` avec le format :

```json
{
  "definitions": [
    {
      "title": "Block Name",
      "id": "block-name",
      "model": "block-name",
      "plugins": {
        "da": {
          "rows": 1,
          "columns": 1,
          "fields": [
            { "name": "image", "selector": "div>div>picture>img[src]" },
            { "name": "alt", "selector": "div>div>picture>img[alt]" },
            { "name": "text", "selector": "div>div>h1" }
          ]
        }
      }
    }
  ],
  "models": [
    {
      "id": "block-name",
      "fields": [
        {
          "component": "reference",
          "valueType": "string",
          "name": "image",
          "label": "Image",
          "multi": false
        },
        {
          "component": "text",
          "valueType": "string",
          "name": "text",
          "value": "",
          "label": "Text",
          "valueType": "string"
        }
      ]
    }
  ],
  "filters": []
}
```

### Contenu da.live
Le contenu doit être crée/édité avec le Universal Editor pour avoir les attributs `data-aue-*`. L'éditeur DA standard n'ajoute pas ces attributs. Quand tu édites une page avec le UE, il sauvegarde le HTML avec les bons attributs.

### Contenu da.live
Le contenu doit être crée/édité avec le Universal Editor pour avoir les attributs `data-aue-*`. L'éditeur DA standard n'ajoute pas ces attributs.

---

## URLs utiles

| Ressource | URL |
|---|---|
| da.live (contenu) | https://da.live |
| da.live config | https://da.live/config#/yaissaoui/ |
| Universal Editor | https://experience.adobe.com (menu → Universal Editor) |
| Site live | https://main--storefront--yaissaoui.aem.live/ |
| Site preview | https://main--storefront--yaissaoui.aem.page/ |
| UE CORS library | https://universal-editor-service.adobe.io/cors.js |
| UE Service ping | https://universal-editor-service.adobe.io/ping |

---

## Checklist de déploiement

1. [ ] `config.json` - Backend Commerce configuré (PaaS ou ACCS)
2. [ ] `fstab.yaml` - Mount points da.live configurés
3. [ ] `default-site.json` - Tous les placeholders remplacés
4. [ ] `head.html` - Meta tags UE + CORS library avec nonce
5. [ ] `editor.path` configuré dans da.live
6. [ ] Contenu créé dans da.live avec le Universal Editor
7. [ ] Fichiers `component-*.json` générés et commités
8. [ ] Pipeline GitHub Actions réussie
9. [ ] Site live accessible et fonctionnel
10. [ ] Universal Editor se connecte sans erreur
