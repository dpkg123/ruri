.PHONY: all
all:
	@echo "\033[33mWarning: This test is under sudo, do not run it on your device.\033[0m"
	@echo "\033[33mYou have 5 seconds to press Ctrl+C to cancel.\033[0m"
	@sleep 5
	@cc -o test-root main.cpp -lstdc++ -g
	@sudo ./test-root

debug-gdb:
	cc -v -o test-root main.cpp -lstdc++ -g3 -O0
	echo "run" | sudo gdb ./test-root

debug:
	sudo bash -x test-root.sh
