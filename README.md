# Metap

Metap provides metaclass propagation along class inheritance structure.
Metap uses closer-mop and changes *c2mop:ensure-class-using-class (class null) name &rest args* so it could conflicts with other library modifing same method.

## Motivation

You got some idea which use metaclass like

```
(defclass some-meta-class (standard-class) ())
(defmethod something1 ((class some-meta-class) ...) ...)
(defmethod something2 ((class some-meta-class) ...) ...)
```

and use this for some classes like

```
(defclass some-class1 () () (:metaclass 'some-meta-class))
(defclass some-class2 (some-class1) () (:metaclass 'some-meta-class))
(defclass some-class3 (some-class2) () (:metaclass 'some-meta-class))
(defclass some-class4 (some-class1) () (:metaclass 'some-meta-class))
... :metaclass :metaclass :metaclass :metaclass
```

This is boring.
Using metap, it can simply be written like

```
(defclass some-mixin () ())
(metap:register-m1-m2-pair some-mixin some-meta-class)

(defclass some-class1 (some-mixin) ())
(defclass some-class2 (some-class1) ())
(defclass some-class3 (some-class2) ())
(defclass some-class4 (some-class1) ())
```

Also see cl-singleton-mixin (https://github.com/hipeta/cl-singleton-mixin) which is written by using metap.

## APIs

### Symbols

- \*metap-m1-m2-paris\*
 * All pairs which registered in metap.

### Macros

- register-m1-m2-pair (m1class m2class)
 * Register m1class and m2class pair which you want enable metaclass propagation.
 * As a point to notice, the concrete metaclass of m1class becomes the subclass of m2class, whose name is like %M2CLASS-METAP. This class is created while interpreting this macro. This specification is needed for applying metaclass to m1class before processing other mop processes.

- validate-superclass* (&body validations)
 * Syntax sugar for c2mop:validate-superclass.

## Installation

1. Download metap and move the directory to quicklisp local-project directory.
1. (ql:quickload :metap)

## License

Metap is released under the MIT License, see LICENSE file.
