# SPDX-License-Identifier: BlueOak-1.0.0
# SPDX-FileCopyrightText: 2024 Saulius Krasuckas <saulius2_at_ar-fi_point_lt> | sskras

A = Magistro baigiamasis darbas. Galutinio dokumento generavimas .pdf formatu.


    HERBO := https://vilniustech.lt/universitetas/universiteto-stilius/322464


all: tex

intro: desc
	@echo
	@echo "Target '?' lists other targets."

desc:
	@echo "${A}"

?: desc
	@echo
	@echo "Available targets:"
	@
	@# Via: https://stackoverflow.com/questions/4219255/how-do-you-get-the-list-of-targets-in-a-makefile/26340367#26340367
	@$(MAKE) -pRrqn -f $(firstword $(MAKEFILE_LIST)) : 2> /dev/null \
	| awk                                                           \
	'                                           \
	      BEGIN                                 \
	      {                                     \
	          RS=""         # Set separators    \
	          FS=":"                            \
	      }                                     \
	                                            \
	      /^# Files$$/                          \
	      {                                     \
	          GO=1          # Start processing  \
	      }                                     \
	                                            \
	      /^# Finished Make data base/          \
	      {                                     \
	          GO=0          # Stop processing   \
	      }                                     \
	                                            \
	      GO && ! /^(#|\.|\$@:)/                \
	      {                                     \
	          printf "    " # Indent            \
	          print $$1     # Print target name \
	      }                                     \
	'                                           \
	| sort

herbas: tex/imgs/Herbas.png

tex/imgs/Herbas.png:
	@ \
	echo "Herbo parsisiuntimas:"                                 && \
	cd tex/imgs                                                  && \
	curl -s ${HERBO}                                                \
	| awk                                                           \
	'                                           \
	    /Herbas.png/                            \
	    {                                       \
	        sub(/^.*https:/, "https:")        # \
	        sub(/Herbas.png.*/, "Herbas.png") # \
	        print                               \
	    }                                       \
	'                                           \
	| xargs curl -O

.PHONY: tex
tex: herbas
	@#pdflatex --halt-on-error
	@ \
	for N in 1 2; do                                                \
	    pdflatex                                                    \
	        --file-line-error                                       \
	        --interaction=nonstopmode                               \
	        --output-directory=tex/out                              \
	        tex/main.tex                                          ; \
	    : Via: https://tex.stackexchange.com/a/159354/43883       ; \
	done                                                          ; \
	: Via: https://tex.stackexchange.com/a/107969/43883           ; \
	echo                                                          ; \
	git status --short                                            ; \

count-tex:
	@texcount -char -stat tex/main.tex

count-pdf:
	@\
	REPLY="                                                         \
	$$(                                                             \
	    dd if=tex/out/main.pdf                                      \
	    | pdftotext - -enc UTF-8 -                                  \
	    | wc -m                                                     \
	  )"                                                          ; \
	echo                                                          ; \
	echo Chars found in .pdf: $${REPLY}                             \
