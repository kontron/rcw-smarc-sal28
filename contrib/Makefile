OBJCOPY ?= objcopy
SED ?= sed
MKIMAGE ?= mkimage

RCW_FILES := $(sort $(wildcard ../sl28-*bin))
RCW_NAMES := $(foreach f,$(RCW_FILES),$(notdir $f))
RCW_FILES_PADDED := $(addsuffix .pad,$(RCW_NAMES))

all: update-rcw.img

update-rcw.img: update-rcw.scr.pad $(RCW_FILES_PADDED)
	@echo "Concatenating final image.."
	@cat $< $(RCW_FILES_PADDED) > $@

update-rcw.cmd: update-rcw.cmd.tmpl list.txt max_selection.txt ../CHANGELOG.md
	@echo "Generating update script.."
	@$(SED) \
		-e '/%RCWLIST%/r list.txt' \
		-e '/%RCWLIST%/d' \
		-e "s/%MAXSELECTION%/$$(cat max_selection.txt)/" \
		-e "s/%RCWVERSION%/$$(sed -ne 's/^## v\([0-9]*\)/\1/p' ../CHANGELOG.md | head -n1)/" \
		$< > $@

update-rcw.scr: update-rcw.cmd
	@echo "Generating u-boot image of the update script.."
	@$(MKIMAGE) -T script -d $^ $@ >/dev/null

update-rcw.scr.pad: update-rcw.scr
	@echo "Padding $(notdir $<)"
	@$(OBJCOPY) -I binary -O binary --gap-fill 0xff --pad-to 0x1000 $^ $@

sl28-%.bin.pad: ../sl28-%.bin
	@echo "Padding $(notdir $<)"
	@$(OBJCOPY) -I binary -O binary --gap-fill 0xff --pad-to 0x800 $^ $@

list.txt max_selection.txt:
	@echo "Generating RCW list.."
	@i=1; \
	for r in $(RCW_NAMES); do \
		if [ $$i -lt 10 ]; then \
			echo "  ($$i) $$r"; \
		else \
			echo " ($$i) $$r"; \
		fi; \
		i=$$((i+1)); \
	done > list.txt; \
	echo $$((i-1)) > max_selection.txt

clean:
	rm -f list.txt max_selection.txt update-rcw.scr* update-rcw.cmd *.pad update-rcw.img
