dnl Ensure that we use a more modern autoconf.
dnl We need autoconf >= 2.62 because of micro AC_PATH_PROGS_FEATURE_CHECK
AC_PREREQ([2.62])

dnl While it is useful to use 'autoscan' to create a configure.ac template,
dnl we cannot do so here because we try autoconf to build PDF from LuaLaTeX.

AC_DEFUN([PROJECT_VERSION], [2021-03-08])

dnl Document Key Words
AC_DEFUN([AC_MAKE_KEYWORDS],
[PDF_KEYWORDS='$1'
AC_MSG_NOTICE([Set the document key words])
AC_SUBST([PDF_KEYWORDS])
])
dnl Document Subject
AC_DEFUN([AC_MAKE_SUBJECT],
[[PDF_SUBJECT]='$1'
AC_MSG_NOTICE([Set the document subject])
AC_SUBST([PDF_SUBJECT])
])

dnl AC_FONT_CHECK([font_type_name_to_check])
dnl The varaible, uppercase of the font_type_name + FONT, will be set
dnl to the first found font, or left to be empty.
AC_DEFUN([AC_FONT_CHECK],
[
dnl We use the following command to check a font
dnl fc-list -q font_name
dnl So we need check fc-list command.
AS_IF([test "x$FC_LIST" = "x"],
AC_CHECK_PROG([FC_LIST], [fc-list], [ok], [failed]))
AS_IF([test "$FC_LIST" = "failed"],
AC_MSG_ERROR([could not find fc-list command]))
AC_MSG_CHECKING([the Chinese font $1])
{
tmp_IFS_=$IFS
IFS=:
for font in [$]$1[fonts];do
fc-list -q $font && m4_toupper([$1])[FONT]=$font && break
done
IFS=$tmp_IFS_
}
AS_IF([test x"[$]m4_toupper([$1])[FONT]" = x],
AC_MSG_ERROR([failed]),
AC_MSG_RESULT([ok]))
])

dnl LUALATEX_CHECK_PROG(VAR, PROG)
dnl VAR be set to value "failed" if not found PROG, others to value "ok"
AC_DEFUN([AC_LUALATEX_CHECK_PROG],
[
AC_CHECK_PROG(m4_toupper([$1]), [$1], [ok], [failed])
AS_IF([test "[$]m4_toupper([$1])" = "failed"],
AC_MSG_ERROR([could not find $1 command]))
])

AC_INIT([LuaLaTeX Template for the Chinese],
[0.0.1], [anzhou94@gmail.com],
[lualatex-template-cn],
[https://github.com/xning/lualatex-template-for-Chinese])
AC_COPYRIGHT([The project is licensed under BSD.])
AC_CONFIG_AUX_DIR([build-aux])
AC_PROG_GREP
AC_PROG_AWK
AC_PROG_INSTALL
AC_PROG_MKDIR_P
AC_CHECK_PROGS([TAR], [gtar tar], [failed])
AS_IF([test "$TAR" = "failed"],
AC_MSG_ERROR([could not find tar or gtar command]))

dnl We need one of md5sum, sha1sum, and sha512sum tools to build
dnl a checksum file when prepare a distribution.
dnl We can make sure that we always have md5sum tool
AC_CHECK_PROGS([CHECKSUM], [sha512sum sha256sum sha1sum md5sum], [failed])
AS_IF([test "$CHECKSUM" = "failed"],
AC_MSG_WARN([could not find any checksum command])
)
AC_SUBST([CHECKSUM])

dnl To run the test script, we need python.
AC_CHECK_PROG([PYTHON3_EXISTED], [python3], [ok], [failed])
AC_CHECK_PROG([PYTHON2_EXISTED], [python2.7], [ok], [failed])
AS_IF([test "$PYTHON2_EXISTED" = "failed" && test "$PYTHON3_EXISTED" = "failed" ],
AC_MSG_ERROR([neither python2.7 or python3 is found to run test script]))

AS_IF([test "$PYTHON3_EXISTED" = "ok"], PYTHON=python3, PYTHON=python2.7)

AS_IF([test "x$PYTHON" != 'x'],
[AC_MSG_CHECKING([module to operate PDF file])
AC_PATH_PROGS_FEATURE_CHECK([PYPDF_EXISTED], [$PYTHON],
[
cat <<EOF | $PYTHON && ac_cv_path_PYPDF_EXISTED=1
import sys
try:
  import pyPdf
except ImportError:
  pass
else:
  sys.exit(0)

try:
  import PyPDF2 as pyPdf
except ImportError:
  sys.exit(1)
else:
  sys.exit(0)
EOF
])
AS_IF([test "$ac_cv_path_PYPDF_EXISTED" = "1"], AS_ECHO([ok]),
AS_ECHO(bad)
AC_MSG_ERROR([Cannot run the test script because of no required python pyPdf module]))
])

AC_SUBST([PYTHON])

AC_LUALATEX_CHECK_PROG([lualatex])
AC_LUALATEX_CHECK_PROG([bibtex])
AC_LUALATEX_CHECK_PROG([makeglossaries])

dnl Need a smart solution for fonts checking and selecting.
AC_MSG_NOTICE([the Chinese fonts])
sansfonts="WenQuanYi Zen Hei:WenQuanYi Micro Hei:Droid Sans Fallback"
seriffonts="AR PL UMing CN:WenQuanYi Zen Hei Sharp"
monofonts="WenQuanYi Zen Hei Mono:WenQuanYi Micro Hei Mono:unifont"
kaisufonts="AR PL UKai CN"
AC_FONT_CHECK([sans])
AC_FONT_CHECK([serif])
AC_FONT_CHECK([mono])
AC_FONT_CHECK([kaisu])

AC_SUBST([SANSFONT])
AC_SUBST([SERIFFONT])
AC_SUBST([MONOFONT])
AC_SUBST([KAISUFONT])

MAKE_DEBUG="@"
AC_SUBST([MAKE_DEBUG])

AC_PROG_MAKE_SET
AC_SUBST([SET_MAKE])

AC_MAKE_KEYWORDS([TeX, LaTeX, LuaTeX, LuaLaTeX, CTeX, Chinese, PDF, wqy, WenQuanYi])
AC_MAKE_SUBJECT([LuaTex Template to produce PDF that support the Chinese])

AC_CONFIG_FILES([Makefile lualatex-template-cn.tex])
AC_CONFIG_FILES([tests/trailer.test], [chmod u+x tests/trailer.test])
AC_OUTPUT
