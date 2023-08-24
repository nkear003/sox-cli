# SoX CLI for preparing samples for various hardware samplers

This shell script is designed to perform various audio processing tasks using command-line tools such as SoX and FFmpeg. It provides functionality for cloning directories, converting audio files, normalizing audio levels, and checking the integrity of WAV files, made specifically for preparing samples for hardware samplers.

**This is is a WIP. I created this for use on MacOS**

## Prerequisites

To use this shell script, make sure you have the following dependencies installed on your system:

- SoX (Sound eXchange) - for audio file manipulation and analysis
- FFmpeg - for audio file conversion
- rsync version of at least 3.1.0 - used to show progress when cloning directories

## Installation

To install and use this CLI tool, follow these steps:

1. Clone the repository to your local machine:

    ```shell
    git clone https://github.com/nkear003/sox-cli
    ```
 
2. Change into the cloned directory:

    ```shell
    cd sox-cli
    ```

3. Make the script executable

    ```shell
    chmod +x sox-cli
    ```

4. Create a symbolic link to the tool in the `/usr/local/bin` directory:

    ```shell
    # -f option will replace the link, if it already exists, in case you move the CLI
    sudo ln -sf "$(pwd)/sox-cli" /usr/local/bin/sox-cli
    ```

    This will allow you to execute the CLI tool from anywhere in your terminal.

## Usage

To run the script, open a terminal and execute the script using the following command:

```shell
sox-cli <command> <BASE_PATH>
```


Replace `<command>` with one of the following available commands:

- `clone-and-convert-ot`: Clones a directory with `-ot` appended to it and converts audio files within it to 44100 sample rate, and any file with a bit depth higher than 24 to a bit depth of 24. The `<BASE_PATH>` should point to the source directory. **When cloning, if the destination folder exists, it will be removed, but you will get a prompt to comfirm**

- `convert`: Converts audio files for Octatrack recursively within a directory specified by `<BASE_PATH>`. See `clone-and-convert-ot` for more details.

- `clone-and-normalize`: Clones a directory with `-normal` appended to it, and normalizes audio files within it. The `<BASE_PATH>` should point to the source directory. **When cloning, if the destination folder exists, it will be removed, but you will get a prompt to comfirm**

- `normalize`: Normalizes audio files recursively within a directory specified by `<BASE_PATH>`.

- `check-files-ot`: Checks the sample rate and bit depth of WAV files within a directory specified by `<BASE_PATH>`. It reports any files with a sample rate different from 44100 Hz or a bit depth higher than 24.

**Note:** Replace `<BASE_PATH>` with the path to the target directory where the operations will be performed.

## Additional Details

- The script utilizes the `rsync` command to clone directories and `soxi` to retrieve audio file information.

- The `clone_dir` function clones a directory by copying its contents to a destination directory specified as `<BASE_PATH>-ot`.

- The `check_and_fix_file_with_ffmpeg_if_needed` function checks if a WAV file has a malformed or missing WAV header and re-encodes it using FFmpeg if necessary.

- The `normalize_audio` function normalizes the audio levels of a WAV file using SoX. If the peak level is already within the acceptable range (-1 dBFS or lower), the file is considered already normalized. Otherwise, it normalizes the file and replaces the original file with the normalized version.

- The `convert_audio_file` function checks the sample rate and bit depth of an audio file. If the sample rate is different from 44100 Hz, it converts the sample rate to 44100 Hz using SoX. If the bit depth is higher than 24, it converts the bit depth to 24 bits.

- The `process_files` function recursively processes all files and directories within a given directory, performing necessary operations based on file types.

- The `normalize_all_files` function recursively normalizes all WAV files within a given directory.

- The `check_files` function recursively checks the sample rate and bit depth of WAV files within a given directory and reports any files with issues.

Feel free to modify and adapt the script according to your specific requirements.

## License

This script is provided under the [MIT License](https://opensource.org/licenses/MIT).

Please refer to the individual dependencies (SoX and FFmpeg) for their respective licenses.

**Note:** Ensure that you have the necessary permissions to execute the script and make any modifications to the audio files.
