.. -*- coding: utf-8; mode: rst; -*-

.. index:: emacs

Emacs Lisp
==========

emacs lisp の 型はコレだけ(?)
-----------------------------

- シンボル
- 整数
- 文字列
- ベクタ(リスト?)
- コンスセル
- 浮動小数点数

つまり、こうかな？
- シンボル
- 即値(整数、文字列、浮動小数点数)
- リスト(ベクタ、コンスセル)

.. ~/work/emacs-27.2/src/lisp.h

.. code-block:: C

   enum Lisp_Type
   {
     /* Symbol.  XSYMBOL (object) points to a struct Lisp_Symbol.  */
     Lisp_Symbol = 0,

     /* Type 1 is currently unused.  */

     /* Fixnum.  XFIXNUM (obj) is the integer value.  */
     Lisp_Int0 = 2,
     Lisp_Int1 = USE_LSB_TAG ? 6 : 3,

     /* String.  XSTRING (object) points to a struct Lisp_String.
        The length of the string, and its contents, are stored therein.  */
     Lisp_String = 4,

     /* Vector of Lisp objects, or something resembling it.
        XVECTOR (object) points to a struct Lisp_Vector, which contains
        the size and contents.  The size field also contains the type
        information, if it's not a real vector object.  */
     Lisp_Vectorlike = 5,

     /* Cons.  XCONS (object) points to a struct Lisp_Cons.  */
     Lisp_Cons = USE_LSB_TAG ? 3 : 6,

     /* Must be last entry in Lisp_Type enumeration.  */
     Lisp_Float = 7
  };

コンスセル
----------

.. code-block:: C

   struct Lisp_Cons
   {
     union
     {
       struct
       {
         /* Car of this cons cell.  */
         Lisp_Object car;

         union
         {
	   /* Cdr of this cons cell.  */
	   Lisp_Object cdr;

	   /* Used to chain conses on a free list.  */
	   struct Lisp_Cons *chain;
         } u;
       } s;
       GCALIGNED_UNION_MEMBER
     } u;
   };

シンボル
--------

.. code-block:: C

   struct Lisp_Symbol
   {
     union
     {
       struct
       {
         bool_bf gcmarkbit : 1;

	 /* Indicates where the value can be found:
	    0 : it's a plain var, the value is in the `value' field.
	    1 : it's a varalias, the value is really in the `alias' symbol.
	    2 : it's a localized var, the value is in the `blv' object.
	    3 : it's a forwarding variable, the value is in `forward'.  */
         ENUM_BF (symbol_redirect) redirect : 3;

         /* 0 : normal case, just set the value
	    1 : constant, cannot set, e.g. nil, t, :keywords.
	    2 : trap the write, call watcher functions.  */
         ENUM_BF (symbol_trapped_write) trapped_write : 2;

         /* Interned state of the symbol.  This is an enumerator from
	    enum symbol_interned.  */
         unsigned interned : 2;

         /* True means that this variable has been explicitly declared
	    special (with `defvar' etc), and shouldn't be lexically bound.  */
         bool_bf declared_special : 1;

         /* True if pointed to from purespace and hence can't be GC'd.  */
         bool_bf pinned : 1;

         /* The symbol's name, as a Lisp string.  */
         Lisp_Object name;

         /* Value of the symbol or Qunbound if unbound.  Which alternative of the
	    union is used depends on the `redirect' field above.  */
         union {
	   Lisp_Object value;
	   struct Lisp_Symbol *alias;
	   struct Lisp_Buffer_Local_Value *blv;
	   lispfwd fwd;
         } val;

         /* Function value of the symbol or Qnil if not fboundp.  */
         Lisp_Object function;

         /* The symbol's property list.  */
         Lisp_Object plist;

         /* Next symbol in obarray bucket, if the symbol is interned.  */
         struct Lisp_Symbol *next;
       } s;
       GCALIGNED_UNION_MEMBER
     } u;
   };



