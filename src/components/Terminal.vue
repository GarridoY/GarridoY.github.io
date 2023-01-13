<script lang="ts">
import { toRaw } from 'vue';

interface System {
    [key: string]: any
}

export default {
    data() {
        return {
            system: {
                "Users": {
                    "Garrido": {
                        "Desktop": {
                            "about.txt": "I'm Daniél",
                            "secret.txt": "SECRET"
                        },
                        "Documents": {

                        }
                    }
                }
            } as System,
            drive: "C:\\" as string,
            directories: [] as Array<string>,
            
            input: "" as string,
            output: "" as string,
            
            validCommands: ["cd", "ls", "help"] as Array<string>,
            previousCommands: [] as Array<string>,
            previousCommandIndex: 0,
        }
    },
    mounted() {
        // Push empty string such that first command is empty when looking through history.
        this.previousCommands.push("");
        // Start at 'home'
        this.resetPath();
    },
    computed: {
        prefix: function() {
            let path = "";
            for (let i = 0; i <= this.directories.length - 1; i++) {
                path += this.directories[i];

                // Don't add backslash if it the last directory.
                if (i != this.directories.length - 1) {
                    path += "\\";
                }
            }
            return this.drive + path + "> ";
        }
    },
    methods: {
        resetPath: function() {
            // Reset path to 'home'
            this.directories = [];
            this.directories.push("Users");
            this.directories.push("Garrido");
        },
        submitCommand: function() {
            // Must contain actual input.
            if (this.input.length <= 0) {
                return;
            }
            
            // Save command in history
            this.previousCommandIndex += 1;
            if (this.getPreviousCommand() != this.input) {
                this.previousCommands.push(this.input);
            }
            
            // Split input into command and parameters.
            const [command, parameters] = this.getCommand(this.input);

            // Check if command is recognized.
            if (!this.isValidCommand(command)) {
                this.output += this.prefix + command + " is not a recognized command. Type 'help' for available commands.\n"
                this.input = "";
                return;
            }

            // Output command.
            this.output += this.prefix + this.input + "\n";
            // Execute command.
            switch(command) {
                case "ls":
                    this.listFiles(this.getCurrentSystem());
                    break;
                case "cd":
                    this.changeDirectory(parameters[0]);
                    break;
                case "help":
                    this.printHelp();
                    break;
            }

            // Get ready for next command.
            this.input = "";
        }, 
        getCommand: function(input: string): [string, Array<string>] {
            const split = input.slice().split(" ");
            return [split[0], split.splice(1)];
        },
        listFiles: function(system: object) {
            // Output all files and directories.
            Object.keys(system).forEach((file) => {
                this.output += file + "\n";
            });
        },
        getCurrentSystem: function() {
            // Find current location in system by following list of directories.
            let system = toRaw(this.system);
            for (const directory of this.directories) {
                if (typeof system[directory] != "undefined") {
                    system = system[directory];
                }
            }
            return system;
        },
        changeDirectory: function(directory: string) {
            // cd command without directory. Go to home.
            if (typeof directory == "undefined") {
                this.resetPath();
                return;
            }

            // Navigating backwards.
            if (directory == "..") {
                this.directories.pop();
                return;
            }
            
            // Directory check
            if (directory.split(".").length > 1) {
                this.output += this.prefix + directory + " is not a directory.\n";
                return;
            }

            // Check if directory exists.
            if (typeof this.getCurrentSystem()[directory] == "undefined") {
                this.output += this.prefix + directory + " could not be found.\n";
                return;
            }

            // Navigate to directory.
            this.directories.push(directory);
        },
        printHelp: function() {
            this.output += "Here is a list of commands.\n" + JSON.stringify(this.validCommands) + "\n";
        },
        isValidCommand: function(command: string): boolean {
            return this.validCommands.includes(command);
        },
        getCurrentDirectory: function() {
            const length = this.directories.length;
            return this.directories[length - 1];
        },
        getPreviousCommand: function(): string {
            return this.previousCommands[this.previousCommandIndex];
        },
        keyUp: function() {
            this.input = this.previousCommands[this.previousCommandIndex];

            // Don't go beyond 0.
            if (this.previousCommandIndex == 0) {
                return;
            }

            this.previousCommandIndex -= 1;
        },
        keyDown: function() {
            // Don't select latest command.
            if (this.previousCommandIndex == this.previousCommands.length - 1) {
                this.input = "";
            }

            // Don't go out of range.
            if (this.previousCommandIndex >= this.previousCommands.length - 1) {
                return;
            }

            this.previousCommandIndex += 1;
            this.input = this.previousCommands[this.previousCommandIndex];
        }
    }
}
</script>

<template>
    <div class="col">
        <div class="row">
            <textarea id="output" class="terminal" disabled v-model="output" rows="10"></textarea>
        </div>
        <form class="row" @submit.prevent="submitCommand">
            <input class="col terminal" v-bind:value="prefix" disabled/>
            <input class="col terminal" autofocus placeholder="Type command here..." v-model="input" @keyup.up="keyUp()" @keyup.down="keyDown()" />
            

            <!-- For 'Enter' to submit -->
            <input type="submit" hidden />
        </form>
    </div>
</template>