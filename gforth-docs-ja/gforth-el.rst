gforth.el
=========

2024年8月3日

下記は forth-mode で describe-mode したのをチョイ訳しただけです。

Gforth 0.7.9_20240418

M-x run-forth
M-x describe-mode in forth process buffer *forth*

概要
----

Inferior Forth mode defined in ‘gforth.el’:
Major mode for interacting with an inferior Forth process.

Emacsの中で Forth を動かす
--------------------------

Forth プロセスは、M-x run-forth で起動できます。

.. note::

   Customisation : このモードに入ると、 comint-mode-hook と inferior-forth-mode-hook フックが (この順序で) 実行されます。

あなたは Forth ソースを含む他のバッファから下位プロセスの Forth にテキストを送信できます。

- forth-switch-to-interactive 現在のバッファを Forth プロセス・バッファに切り替えます。
- forth-send-paragraph は、 現在の段落(paragraph)を Forth プロセスに送ります。
- forth-send-region は、 現在のリージョンを Forth プロセスに送ります。
- forth-send-buffer は、 現在のバッファを Forth プロセスに送ります。
- forth-send-paragraph-and-go, forth-send-region-and-go, forth-send-buffer-and-go は、 それぞれのテキストを送った後、 Forth プロセス・バッファに切り替えます。

For information on running multiple processes in multiple buffers, see
documentation for variable ‘forth-process-buffer’.


キーバインド
------------

+-------------+-------------------------------------------+
| key         | binding                                   |
+=============+===========================================+
| C-c         | Prefix Command                            |
+-------------+-------------------------------------------+
| C-d         | comint-delchar-or-maybe-eof               |
+-------------+-------------------------------------------+
| RET         | comint-send-input                         |
+-------------+-------------------------------------------+
| ESC         | Prefix Command                            |
+-------------+-------------------------------------------+
| <C-down>    | comint-next-input                         |
+-------------+-------------------------------------------+
| <C-up>      | comint-previous-input                     |
+-------------+-------------------------------------------+
| <delete>    | delete-forward-char                       |
+-------------+-------------------------------------------+
| <kp-delete> | delete-forward-char                       |
+-------------+-------------------------------------------+
| <mouse-2>   | comint-insert-input                       |
+-------------+-------------------------------------------+
| C-c C-l     | forth-load-file                           |
+-------------+-------------------------------------------+
| C-M-x       | forth-send-paragraph-and-go               |
+-------------+-------------------------------------------+
| C-c C-a     | comint-bol-or-process-mark                |
+-------------+-------------------------------------------+
| C-c C-c     | comint-interrupt-subjob                   |
+-------------+-------------------------------------------+
| C-c C-d     | comint-send-eof                           |
+-------------+-------------------------------------------+
| C-c C-e     | comint-show-maximum-output                |
+-------------+-------------------------------------------+
| C-c C-l     | comint-dynamic-list-input-ring            |
|             | (that binding is currently                |
|             | shadowed by another mode)                 |
+-------------+-------------------------------------------+
| C-c RET     | comint-copy-old-input                     |
+-------------+-------------------------------------------+
| C-c C-n     | comint-next-prompt                        |
+-------------+-------------------------------------------+
| C-c C-o     | comint-delete-output                      |
+-------------+-------------------------------------------+
| C-c C-p     | comint-previous-prompt                    |
+-------------+-------------------------------------------+
| C-c C-r     | comint-show-output                        |
+-------------+-------------------------------------------+
| C-c C-s     | comint-write-output                       |
+-------------+-------------------------------------------+
| C-c C-u     | comint-kill-input                         |
+-------------+-------------------------------------------+
| C-c C-w     | backward-kill-word                        |
+-------------+-------------------------------------------+
| C-c C-x     | comint-get-next-from-history              |
+-------------+-------------------------------------------+
| C-c C-z     | comint-stop-subjob                        |
+-------------+-------------------------------------------+
| C-c ESC     | Prefix Command                            |
+-------------+-------------------------------------------+
| C-c C-\     | comint-quit-subjob                        |
+-------------+-------------------------------------------+
| C-c SPC     | comint-accumulate                         |
+-------------+-------------------------------------------+
| C-c .       | comint-insert-previous-argument           |
+-------------+-------------------------------------------+
| C-M-l       | comint-show-output                        |
+-------------+-------------------------------------------+
| M-n         | comint-next-input                         |
+-------------+-------------------------------------------+
| M-p         | comint-previous-input                     |
+-------------+-------------------------------------------+
| M-r         | comint-history-isearch-backward-regexp    |
+-------------+-------------------------------------------+
| C-c M-o     | comint-clear-buffer                       |
+-------------+-------------------------------------------+
| C-c M-r     | comint-previous-matching-input-from-input |
+-------------+-------------------------------------------+
| C-c M-s     | comint-next-matching-input-from-input     |
+-------------+-------------------------------------------+


Enabled minor modes
-------------------

Enabled minor modes: Async-Bytecomp-Package Auto-Composition
Auto-Compression Auto-Encryption Blink-Cursor Column-Number
Electric-Indent File-Name-Shadow Font-Lock Global-Eldoc
Global-Font-Lock Helm Helm-Minibuffer-History Line-Number Menu-Bar
Mouse-Wheel Shell-Dirtrack Tooltip

(Information about these minor modes follows the major mode info.)

プロセスの終了後に戻れば、 ポイントにテキストが送られます。 プロセスを誤って一時停止(susbpend)した場合は、 M-x comint-continue-subjob  を使用してプロセスを続行させます。

Async-Bytecomp-Package minor mode (no indicator):
Byte compile asynchronously packages installed with package.el.
Async compilation of packages can be controlled by
‘async-bytecomp-allowed-packages’.

If called interactively, enable Async-Bytecomp-Package mode if
ARG is positive, and disable it if ARG is zero or negative.  If
called from Lisp, also enable the mode if ARG is omitted or nil,
and toggle it if ARG is ‘toggle’; disable the mode otherwise.

Auto-Composition minor mode (no indicator):
Toggle Auto Composition mode.

If called interactively, enable Auto-Composition mode if ARG is
positive, and disable it if ARG is zero or negative.  If called from
Lisp, also enable the mode if ARG is omitted or nil, and toggle it if
ARG is ‘toggle’; disable the mode otherwise.

When Auto Composition mode is enabled, text characters are
automatically composed by functions registered in
‘composition-function-table’.

You can use ‘global-auto-composition-mode’ to turn on
Auto Composition mode in all buffers (this is the default).

Auto-Compression minor mode (no indicator):
Toggle Auto Compression mode.

If called interactively, enable Auto-Compression mode if ARG is
positive, and disable it if ARG is zero or negative.  If called from
Lisp, also enable the mode if ARG is omitted or nil, and toggle it if
ARG is ‘toggle’; disable the mode otherwise.

Auto Compression mode is a global minor mode.  When enabled,
compressed files are automatically uncompressed for reading, and
compressed when writing.

Auto-Encryption minor mode (no indicator):
Toggle automatic file encryption/decryption (Auto Encryption mode).

If called interactively, enable Auto-Encryption mode if ARG is
positive, and disable it if ARG is zero or negative.  If called from
Lisp, also enable the mode if ARG is omitted or nil, and toggle it if
ARG is ‘toggle’; disable the mode otherwise.

Blink-Cursor minor mode (no indicator):
Toggle cursor blinking (Blink Cursor mode).

If called interactively, enable Blink-Cursor mode if ARG is positive,
and disable it if ARG is zero or negative.  If called from Lisp, also
enable the mode if ARG is omitted or nil, and toggle it if ARG is
‘toggle’; disable the mode otherwise.

If the value of ‘blink-cursor-blinks’ is positive (10 by default),
the cursor stops blinking after that number of blinks, if Emacs
gets no input during that time.

See also ‘blink-cursor-interval’ and ‘blink-cursor-delay’.

This command is effective only on graphical frames.  On text-only
terminals, cursor blinking is controlled by the terminal.

Column-Number minor mode (no indicator):
Toggle column number display in the mode line (Column Number mode).

If called interactively, enable Column-Number mode if ARG is positive,
and disable it if ARG is zero or negative.  If called from Lisp, also
enable the mode if ARG is omitted or nil, and toggle it if ARG is
‘toggle’; disable the mode otherwise.

Electric-Indent minor mode (no indicator):
Toggle on-the-fly reindentation (Electric Indent mode).

If called interactively, enable Electric-Indent mode if ARG is
positive, and disable it if ARG is zero or negative.  If called from
Lisp, also enable the mode if ARG is omitted or nil, and toggle it if
ARG is ‘toggle’; disable the mode otherwise.

When enabled, this reindents whenever the hook ‘electric-indent-functions’
returns non-nil, or if you insert a character from ‘electric-indent-chars’.

This is a global minor mode.  To toggle the mode in a single buffer,
use ‘electric-indent-local-mode’.

File-Name-Shadow minor mode (no indicator):
Toggle file-name shadowing in minibuffers (File-Name Shadow mode).

If called interactively, enable File-Name-Shadow mode if ARG is
positive, and disable it if ARG is zero or negative.  If called from
Lisp, also enable the mode if ARG is omitted or nil, and toggle it if
ARG is ‘toggle’; disable the mode otherwise.

File-Name Shadow mode is a global minor mode.  When enabled, any
part of a filename being read in the minibuffer that would be
ignored (because the result is passed through
‘substitute-in-file-name’) is given the properties in
‘file-name-shadow-properties’, which can be used to make that
portion dim, invisible, or otherwise less visually noticeable.

Font-Lock minor mode (no indicator):
Toggle syntax highlighting in this buffer (Font Lock mode).

If called interactively, enable Font-Lock mode if ARG is positive, and
disable it if ARG is zero or negative.  If called from Lisp, also
enable the mode if ARG is omitted or nil, and toggle it if ARG is
‘toggle’; disable the mode otherwise.

When Font Lock mode is enabled, text is fontified as you type it:

 - Comments are displayed in ‘font-lock-comment-face’;
 - Strings are displayed in ‘font-lock-string-face’;
 - Certain other expressions are displayed in other faces
   according to the value of the variable ‘font-lock-keywords’.

To customize the faces (colors, fonts, etc.) used by Font Lock for
fontifying different parts of buffer text, use M-x customize-face.

You can enable Font Lock mode in any major mode automatically by
turning on in the major mode’s hook.  For example, put in your
~/.emacs:

 (add-hook 'c-mode-hook 'turn-on-font-lock)

Alternatively, you can use Global Font Lock mode to automagically
turn on Font Lock mode in buffers whose major mode supports it
and whose major mode is one of ‘font-lock-global-modes’.  For
example, put in your ~/.emacs:

 (global-font-lock-mode t)

Where major modes support different levels of fontification, you
can use the variable ‘font-lock-maximum-decoration’ to specify
which level you generally prefer.  When you turn Font Lock mode
on/off the buffer is fontified/defontified, though fontification
occurs only if the buffer is less than ‘font-lock-maximum-size’.

To add your own highlighting for some major mode, and modify the
highlighting selected automatically via the variable
‘font-lock-maximum-decoration’, you can use
‘font-lock-add-keywords’.

To fontify a buffer, without turning on Font Lock mode and
regardless of buffer size, you can use M-x font-lock-fontify-buffer.

To fontify a block (the function or paragraph containing point,
or a number of lines around point), perhaps because modification
on the current line caused syntactic change on other lines, you
can use M-o M-o.

You can set your own default settings for some mode, by setting a
buffer local value for ‘font-lock-defaults’, via its mode hook.

The above is the default behavior of ‘font-lock-mode’; you may
specify your own function which is called when ‘font-lock-mode’
is toggled via ‘font-lock-function’.

Global-Eldoc minor mode (no indicator):
Toggle Eldoc mode in all buffers.
With prefix ARG, enable Global Eldoc mode if ARG is positive;
otherwise, disable it.  If called from Lisp, enable the mode if
ARG is omitted or nil.

Eldoc mode is enabled in all buffers where
‘turn-on-eldoc-mode’ would do it.
See ‘eldoc-mode’ for more information on Eldoc mode.

Global-Font-Lock minor mode (no indicator):
Toggle Font-Lock mode in all buffers.
With prefix ARG, enable Global Font-Lock mode if ARG is positive;
otherwise, disable it.  If called from Lisp, enable the mode if
ARG is omitted or nil.

Font-Lock mode is enabled in all buffers where
‘turn-on-font-lock-if-desired’ would do it.
See ‘font-lock-mode’ for more information on Font-Lock mode.

Helm minor mode (indicator Helm):
Toggle generic helm completion.

If called interactively, enable Helm mode if ARG is positive, and
disable it if ARG is zero or negative.  If called from Lisp, also
enable the mode if ARG is omitted or nil, and toggle it if ARG is
‘toggle’; disable the mode otherwise.

All functions in Emacs that use ‘completing-read’,
‘read-file-name’, ‘completion-in-region’ and friends will use helm
interface when this mode is turned on.

However you can modify this behavior for functions of your choice
with ‘helm-completing-read-handlers-alist’.

Called with a positive arg, turn on unconditionally, with a
negative arg turn off.
You can toggle it with M-x ‘helm-mode’.

About ‘ido-mode’:
DO NOT enable ‘ido-everywhere’ when using ‘helm-mode’.  Instead of
using ‘ido-mode’, add the commands where you want to use ido to
‘helm-completing-read-handlers-alist’ with ‘ido’ as value.

Note: This mode is incompatible with Emacs23.

Helm-Minibuffer-History minor mode (no indicator):
Bind ‘helm-minibuffer-history-key’ in al minibuffer maps.
This mode is enabled by ‘helm-mode’, so there is no need to enable it directly.

If called interactively, enable Helm-Minibuffer-History mode if ARG is
positive, and disable it if ARG is zero or negative.  If called from
Lisp, also enable the mode if ARG is omitted or nil, and toggle it if
ARG is ‘toggle’; disable the mode otherwise.

Line-Number minor mode (no indicator):
Toggle line number display in the mode line (Line Number mode).

If called interactively, enable Line-Number mode if ARG is positive,
and disable it if ARG is zero or negative.  If called from Lisp, also
enable the mode if ARG is omitted or nil, and toggle it if ARG is
‘toggle’; disable the mode otherwise.

Line numbers do not appear for very large buffers and buffers
with very long lines; see variables ‘line-number-display-limit’
and ‘line-number-display-limit-width’.

Menu-Bar minor mode (no indicator):
Toggle display of a menu bar on each frame (Menu Bar mode).

If called interactively, enable Menu-Bar mode if ARG is positive, and
disable it if ARG is zero or negative.  If called from Lisp, also
enable the mode if ARG is omitted or nil, and toggle it if ARG is
‘toggle’; disable the mode otherwise.

This command applies to all frames that exist and frames to be
created in the future.

Mouse-Wheel minor mode (no indicator):
Toggle mouse wheel support (Mouse Wheel mode).

If called interactively, enable Mouse-Wheel mode if ARG is positive,
and disable it if ARG is zero or negative.  If called from Lisp, also
enable the mode if ARG is omitted or nil, and toggle it if ARG is
‘toggle’; disable the mode otherwise.

Shell-Dirtrack minor mode (no indicator):
Toggle directory tracking in this shell buffer (Shell Dirtrack mode).

If called interactively, enable Shell-Dirtrack mode if ARG is
positive, and disable it if ARG is zero or negative.  If called from
Lisp, also enable the mode if ARG is omitted or nil, and toggle it if
ARG is ‘toggle’; disable the mode otherwise.

The ‘dirtrack’ package provides an alternative implementation of
this feature; see the function ‘dirtrack-mode’.

Tooltip minor mode (no indicator):
Toggle Tooltip mode.

If called interactively, enable Tooltip mode if ARG is positive, and
disable it if ARG is zero or negative.  If called from Lisp, also
enable the mode if ARG is omitted or nil, and toggle it if ARG is
‘toggle’; disable the mode otherwise.

When this global minor mode is enabled, Emacs displays help
text (e.g. for buttons and menu items that you put the mouse on)
in a pop-up window.

When Tooltip mode is disabled, Emacs displays help text in the
echo area, instead of making a pop-up window.
