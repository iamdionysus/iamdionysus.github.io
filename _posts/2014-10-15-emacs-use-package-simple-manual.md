---
layout: post
title: How to use emacs use-package
---

## When using MELPA, how to install package automatically
Use ensure.

```
(use-package helm-projectile
  :ensure t
)
```

## How to bind keyboard shortcut
Use bind. Format is ("key combination" . function-name)

```
(use-package helm
  :bind(
    ("C-x m" . helm-mini)
	("C-x f" . helm-projectile)
	("C-x C-d" . helm-find-files)
  )
  :ensure t
)
```

## How to run something for the package
Use init. 

```
(use-package projectile-rails
  :init
  (add-hook 'projectile-mode-hook 'projectile-rails-on)
  :ensure t
)
```

For the multiple running, use progn inside init. 

```
(use-package flycheck
  :init
  (progn
     (add-hook 'after-init-hook #'global-flycheck-mode)
     (setq flycheck-check-syntax-automatically '(save))
     (add-hook 'flycheck-mode-hook 'flycheck-mode-keys)
  )
  :ensure t
)
```

If something has to run after the package is loaded, use defer.

```
(use-package color-theme-solarized
  :defer t
  :init
  (load-theme 'solarized-dark t)
  :ensure t
)
```

