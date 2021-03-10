AC_DEFUN([MIN_AUTOCONF_VERSION], [2.62])
AC_DEFUN([PROJECT_VERSION], [0.01])
AC_DEFUN([PROJECT_DESCRIPTION],
[LuaLaTeX Template for the Chinese])
AC_DEFUN([PROJECT_BUG_REPORT], [anzhou94@gmail.com])
AC_DEFUN([PROJECT_NAME],
[lualatex-template-cn])
AC_DEFUN([PROJECT_URL],
[https://github.com/xning/lualatex-template-for-Chinese])
AC_DEFUN([PROJECT_COPYRIGHT],
[The project is licensed under BSD])
m4_include([PDF定制])
dnl Ensure that we use a more modern autoconf.
dnl We need autoconf >= 2.62 because of micro AC_PATH_PROGS_FEATURE_CHECK
AC_PREREQ([2.62])

dnl Document Key Words
AC_DEFUN([AC_MAKE_KEYWORDS],
[[PDF_KEYWORDS]='$1'
AC_MSG_NOTICE([Set the document key words])
AC_SUBST([PDF_KEYWORDS])
])
dnl Document Subject
AC_DEFUN([AC_MAKE_SUBJECT],
[[PDF_SUBJECT]='$1'
AC_MSG_NOTICE([Set the document subject])
AC_SUBST([PDF_SUBJECT])
])

dnl SEARCH_AVAILABLE_FONT([font_type_name_to_check])
dnl The varaible, uppercase of the font_type_name + FONT, will be set
dnl to the first found font, or left to be empty.
AC_DEFUN([SEARCH_AVAILABLE_FONT],
[
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
AC_DEFUN([FAIL_IF_TOOL_NOT_FOUND],
[
AC_CHECK_PROG(m4_toupper(m4_translit([$1], [-], [_])), [$1], [$1], [])
AS_IF([test "x[$]m4_toupper(m4_translit([$1], [-], [_]))" = 'x'],
AC_MSG_ERROR([tool $1 not found]))
])

AC_INIT(
PROJECT_DESCRIPTION,
PROJECT_VERSON,
PROJECT_BUG_REPORT,
PROJECT_NAME,
PROJECT_URL)

AC_COPYRIGHT(PROJECT_COPYRIGHT)
AC_CONFIG_AUX_DIR([build-aux])
AC_PROG_GREP
AC_PROG_AWK
AC_PROG_INSTALL
AC_PROG_MKDIR_P
AC_CHECK_PROGS([TAR], [gtar tar], [failed])
AS_IF([test "$TAR" = "failed"],
AC_MSG_ERROR([could not find tar or gtar command]))

[PDF_AUTHOR]=AS_ESCAPE(['PDF_AUTHOR'])
[PDF_TITLE]=AS_ESCAPE(['PDF_TITLE'])
AC_SUBST([PDF_AUTHOR])
AC_SUBST([PDF_TITLE])

dnl We need one of md5sum, sha1sum, and sha512sum tools to build
dnl a checksum file when prepare a distribution.
dnl We can make sure that we always have md5sum tool
AC_CHECK_PROGS([CHECKSUM], [sha512sum sha256sum sha1sum md5sum], [failed])
AS_IF([test "$CHECKSUM" = "failed"],
AC_MSG_WARN([could not find any checksum command])
)
AC_SUBST([CHECKSUM])

dnl To run the test script, we need python to operate PDF file
AC_CHECK_PROGS([PYTHON], [python3 python2.7], [])
AS_IF([test "x$PYTHON" = 'x' ],
AC_MSG_ERROR([neither python2.7 or python3 is found to run test script]))

AC_MSG_CHECKING([python module to operate PDF file])
[PYPDF_EXISTED]=
AC_PATH_PROGS_FEATURE_CHECK([PYPDF_EXISTED], [$PYTHON],
[
ac_cv_path_PYPDF_EXISTED=0
cat <<EOF | $ac_path_VARIABLE && ac_cv_path_PYPDF_EXISTED=1
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

AC_SUBST([PYTHON])

FAIL_IF_TOOL_NOT_FOUND([lualatex])
FAIL_IF_TOOL_NOT_FOUND([bibtex])
FAIL_IF_TOOL_NOT_FOUND([makeglossaries])
FAIL_IF_TOOL_NOT_FOUND([fc-list])

dnl Need a smart solution for fonts checking and selecting.
AC_MSG_NOTICE([the Chinese fonts])
sansfonts="WenQuanYi Zen Hei:WenQuanYi Micro Hei:Droid Sans Fallback"
seriffonts="AR PL UMing CN:WenQuanYi Zen Hei Sharp"
monofonts="WenQuanYi Zen Hei Mono:WenQuanYi Micro Hei Mono:unifont"
kaisufonts="AR PL UKai CN"
SEARCH_AVAILABLE_FONT([sans])
SEARCH_AVAILABLE_FONT([serif])
SEARCH_AVAILABLE_FONT([mono])
SEARCH_AVAILABLE_FONT([kaisu])

AC_SUBST([SANSFONT])
AC_SUBST([SERIFFONT])
AC_SUBST([MONOFONT])
AC_SUBST([KAISUFONT])

MAKE_DEBUG="@"
AC_SUBST([MAKE_DEBUG])

AC_PROG_MAKE_SET
AC_SUBST([SET_MAKE])

AC_MAKE_KEYWORDS(PDF_KEYWORDS)
AC_MAKE_SUBJECT(PDF_SUBJECT)

AC_CONFIG_FILES([Makefile])
AC_CONFIG_FILES([tests/trailer.test], [chmod u+x tests/trailer.test])
AC_CONFIG_FILES(PDF_FILE)
AC_OUTPUT
